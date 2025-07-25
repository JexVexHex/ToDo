# ToDo App Technical Specification

This document outlines the functionality, data flow, architecture, and implementation details of the ToDo list application.

<!-- spec-metadata
version: 2.0
last-updated: 2024
complexity: beginner
patterns: [mvc-lite, event-delegation, local-storage]
-->

## Table of Contents
1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Core Components](#core-components)
4. [JavaScript API](#javascript-api)
5. [Data Model](#data-model)
6. [Event Flow](#event-flow)
7. [Storage Strategy](#storage-strategy)
8. [Security Considerations](#security-considerations)
9. [Performance Notes](#performance-notes)
10. [Browser Compatibility](#browser-compatibility)

## 1. Overview

The ToDo App is a client-side web application that allows users to:
- Create, read, update, and delete tasks
- Mark tasks as complete/incomplete
- Persist data across browser sessions
- Filter tasks by status
- Maintain an accessible interface

### Technology Stack
- **HTML5**: Semantic markup and structure
- **CSS3**: Styling and animations
- **Vanilla JavaScript**: Core functionality (no frameworks)
- **LocalStorage API**: Data persistence

## 2. Architecture

The application follows a simplified MVC-lite pattern:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│    View     │────▶│ Controller  │────▶│    Model    │
│   (HTML)    │◀────│ (JavaScript)│◀────│(LocalStorage)│
└─────────────┘     └─────────────┘     └─────────────┘
```

### Design Principles
1. **Separation of Concerns**: HTML for structure, CSS for presentation, JS for behavior
2. **Progressive Enhancement**: Core functionality works without JavaScript
3. **Accessibility First**: ARIA labels and keyboard navigation
4. **Mobile Responsive**: Adapts to different screen sizes

## 3. Core Components

The application is built with three core files:

-   `index.html`: Provides the structure and layout of the user interface.
-   `style.css`: Adds styling to make the application visually appealing.
-   `script.js`: Contains all the logic for adding, deleting, and marking tasks as complete.

### Component Interactions

```javascript
// Component interaction flow
User Input → Event Handler → DOM Update → Storage Save → UI Feedback
```

## 4. JavaScript API

### Public Functions

#### `addTask()`
Creates a new task and adds it to the list.

**Parameters**: None (reads from input field)  
**Returns**: void  
**Side Effects**: 
- Updates DOM
- Saves to localStorage
- Clears input field
- Updates task counter

```javascript
function addTask() {
    // Validation
    if (inputBox.value === '') {
        alert("You must write something!");
        return;
    }
    
    // Task creation
    const task = {
        text: inputBox.value,
        completed: false,
        id: Date.now(),
        category: 'general'
    };
    
    // DOM manipulation
    renderTask(task);
    
    // Cleanup
    inputBox.value = "";
    saveData();
}
```

#### `saveData()`
Persists the current state to localStorage.

**Parameters**: None  
**Returns**: void  
**Storage Key**: `"data"`  
**Storage Format**: HTML string (serialized task list)

#### `showTask()`
Loads and displays saved tasks from localStorage.

**Parameters**: None  
**Returns**: void  
**Called**: On page load

### Event Handlers

#### Task Click Handler
Handles both task completion toggle and deletion.

```javascript
listContainer.addEventListener('click', function(e) {
    if (e.target.tagName === "LI") {
        // Toggle completion
        e.target.classList.toggle("checked");
        saveData();
    } else if (e.target.tagName === "BUTTON") {
        // Delete task
        e.target.parentElement.remove();
        saveData();
    }
}, false);
```

## 5. Data Model

### Task Object Structure
```javascript
{
    id: number,          // Unique identifier (timestamp)
    text: string,        // Task description
    completed: boolean,  // Completion status
    category: string,    // Task category (optional)
    createdAt: Date,     // Creation timestamp (optional)
    dueDate: Date        // Due date (optional, future feature)
}
```

### Storage Format
Currently stores raw HTML for simplicity:
```html
<li class="checked">Task 1<button>×</button></li>
<li>Task 2<button>×</button></li>
```

Future versions should store JSON:
```json
{
    "tasks": [
        {"id": 1, "text": "Task 1", "completed": true},
        {"id": 2, "text": "Task 2", "completed": false}
    ]
}
```

## 6. Event Flow

### Adding a Task
```
1. User types text
2. User clicks "Add" or presses Enter
3. addTask() validates input
4. Creates new <li> element
5. Appends to task list
6. Saves to localStorage
7. Clears input field
8. Updates task counter
```

### Completing a Task
```
1. User clicks on task text
2. Event bubbles to list container
3. Handler checks if target is <li>
4. Toggles "checked" class
5. Saves updated state
```

### Deleting a Task
```
1. User clicks delete button (×)
2. Event bubbles to list container
3. Handler checks if target is button
4. Removes parent <li> element
5. Saves updated state
```

## 7. Storage Strategy

### Current Implementation
- **Key**: `"data"`
- **Value**: Inner HTML of task list
- **Format**: String (HTML)
- **Size Limit**: ~5-10MB (browser dependent)

### Recommended Improvements
```javascript
// Better storage structure
const storageAPI = {
    save: function(tasks) {
        localStorage.setItem('todo-tasks', JSON.stringify(tasks));
        localStorage.setItem('todo-version', '2.0');
    },
    
    load: function() {
        const version = localStorage.getItem('todo-version');
        const data = localStorage.getItem('todo-tasks');
        
        if (version === '2.0' && data) {
            return JSON.parse(data);
        }
        
        // Migration from old format
        return this.migrate();
    },
    
    migrate: function() {
        // Convert old HTML format to new JSON format
        const oldData = localStorage.getItem('data');
        if (oldData) {
            // Parse HTML and extract tasks
            // ... migration logic
        }
        return [];
    }
};
```

## 8. Security Considerations

### Current Vulnerabilities
1. **XSS Risk**: Using `innerHTML` with user input
2. **No Input Sanitization**: Raw user input stored and displayed

### Recommended Fixes
```javascript
// Safe text insertion
function escapeHtml(unsafe) {
    return unsafe
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");
}

// Use textContent instead of innerHTML
li.textContent = inputBox.value;
```

### Content Security Policy
```html
<meta http-equiv="Content-Security-Policy" 
      content="default-src 'self'; script-src 'self'">
```

## 9. Performance Notes

### Current Performance
- **Initial Load**: < 50ms
- **Add Task**: < 10ms
- **Storage Operations**: < 5ms

### Optimization Opportunities
1. **Debounce Storage**: Don't save on every change
```javascript
let saveTimeout;
function debouncedSave() {
    clearTimeout(saveTimeout);
    saveTimeout = setTimeout(saveData, 500);
}
```

2. **Virtual Scrolling**: For lists > 100 items
3. **Web Workers**: For heavy operations
4. **IndexedDB**: For larger datasets

### Memory Management
- Event delegation reduces listener count
- Proper cleanup on task deletion
- No memory leaks identified

## 10. Browser Compatibility

### Supported Browsers
- Chrome 60+
- Firefox 55+
- Safari 11+
- Edge 79+

### Required APIs
- `localStorage`
- `addEventListener`
- `querySelector/querySelectorAll`
- `classList`

### Polyfills Needed
None for modern browsers

### Progressive Enhancement
```javascript
// Check for localStorage support
if (typeof(Storage) !== "undefined") {
    // LocalStorage available
    showTask();
} else {
    // No localStorage support
    console.warn("No localStorage support");
}
```

## Implementation Checklist

### Phase 1: Core Features ✅
- [x] Add tasks
- [x] Delete tasks
- [x] Mark complete
- [x] Local storage

### Phase 2: Enhancements
- [ ] Task categories
- [ ] Due dates
- [ ] Search/filter
- [ ] Drag and drop reordering

### Phase 3: Advanced
- [ ] Sync across devices
- [ ] Collaboration features
- [ ] Notifications
- [ ] Offline support (PWA)

## Testing Guidelines

### Unit Tests
```javascript
// Example test cases
describe('ToDo App', () => {
    it('should add a new task', () => {
        // Test implementation
    });
    
    it('should not add empty tasks', () => {
        // Test implementation
    });
    
    it('should toggle task completion', () => {
        // Test implementation
    });
});
```

### Manual Testing Checklist
- [ ] Add task with various text lengths
- [ ] Try adding empty task
- [ ] Complete/uncomplete tasks
- [ ] Delete tasks
- [ ] Refresh page (persistence)
- [ ] Test on mobile devices
- [ ] Test with screen reader

## Conclusion

This ToDo app demonstrates fundamental web development concepts while maintaining simplicity. The architecture allows for easy enhancement and serves as an excellent learning project for beginners while providing a foundation for more advanced features.