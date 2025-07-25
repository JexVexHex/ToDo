# Beginner's Tutorial: Build a ToDo App

Welcome! This tutorial will guide you through building a simple ToDo List application using HTML, CSS, and JavaScript.

<!-- tutorial-metadata
level: beginner
duration: 60-90 minutes
prerequisites: basic HTML knowledge
topics: [html, css, javascript, dom-manipulation, local-storage, accessibility]
-->

## Introduction

In this tutorial, you'll learn:
- How to structure a web page with HTML
- How to style elements with CSS
- How to add interactivity with JavaScript
- How to store data in the browser
- How to make your app accessible to everyone

Let's build something awesome together! 🚀

## Lesson 1: Setting Up the HTML Structure

Every web page starts with HTML. It provides the basic structure, like the skeleton of a building.

### Key Concepts
- **Container elements**: `<div>` tags that group related content
- **Input elements**: `<input>` for user text entry
- **Lists**: `<ul>` for unordered lists and `<li>` for list items

### The Basic Structure

1.  **Create the main container**: In `index.html`, we have a `div` with the class `container` that holds our app.
2.  **Create the ToDo App structure**: Inside the container, another `div` with class `todo-app` holds the title (`h2`) and the input field.
3.  **Add the Input Field and Button**: We use an `<input>` tag for typing new tasks and a `<button>` to add them.
4.  **Create the Task List**: An unordered list (`<ul>`) with the `id="list-container"` is where our tasks will appear.

```html
<!-- index.html -->
<div class="container">
    <div class="todo-app">
        <h2>To-Do List <img src="images/icon.png"></h2>
        <div class="row">
            <input type="text" id="input-box" placeholder="Add your text">
            <button onclick="addTask()">Add</button>
        </div>
        <ul id="list-container">
            <!-- Tasks will be added here by JavaScript -->
        </ul>
    </div>
</div>
```

### 💡 Exercise 1.1: Understanding HTML Structure
<!-- exercise-start id="html-structure-basics" -->
**Objective**: Modify the HTML to add a subtitle under the ToDo List heading.

**Task**: Add a `<p>` tag with the text "Stay organized!" right after the `<h2>` element.

**Hint**: The `<p>` tag should be a sibling of the `<h2>` tag, not inside it.

**Expected Output**:
```html
<h2>To-Do List <img src="images/icon.png"></h2>
<p>Stay organized!</p>
```
<!-- exercise-end -->

### 💡 Exercise 1.2: Adding HTML Elements
<!-- exercise-start id="add-html-elements" -->
**Objective**: Add a task counter display to your HTML.

**Task**: Create a `<div>` with class "task-counter" after the list container that will show "0 tasks remaining".

**Starter Code**:
```html
<ul id="list-container">
    <!-- Tasks will be added here -->
</ul>
<!-- Add your task counter here -->
```

**Solution**:
```html
<ul id="list-container">
    <!-- Tasks will be added here -->
</ul>
<div class="task-counter">
    <span id="task-count">0 tasks remaining</span>
</div>
```
<!-- exercise-end -->

## Lesson 2: Making it Look Good with CSS

CSS (Cascading Style Sheets) is like the paint and decorations of your house - it makes everything look beautiful!

### Key Concepts
- **Selectors**: Target specific HTML elements
- **Properties**: Define how elements should look
- **Classes**: Reusable styles for multiple elements

-   We style the `body`, `.container`, and `.todo-app` to center the application and give it a nice background.
-   The input field and button are styled to look like a single unit.
-   We define a `.checked` class for tasks that are completed. This class adds a line-through style and changes the checkmark image.

### 💡 Exercise 2.1: Basic Styling
<!-- exercise-start id="basic-css-styling" -->
**Objective**: Style the task counter you created in Exercise 1.2.

**Task**: Add CSS to make the task counter centered with gray text.

**Requirements**:
- Text color: #666
- Text alignment: center
- Font size: 14px
- Margin top: 20px

**Starter Code**:
```css
.task-counter {
    /* Add your styles here */
}
```

**Solution**:
```css
.task-counter {
    color: #666;
    text-align: center;
    font-size: 14px;
    margin-top: 20px;
}
```
<!-- exercise-end -->

### 💡 Exercise 2.2: Hover Effects
<!-- exercise-start id="css-hover-effects" -->
**Objective**: Add a hover effect to the Add button.

**Task**: Make the button change color when you hover over it.

**Requirements**:
- Normal background: #009579
- Hover background: #007a63
- Add a smooth transition

**Solution**:
```css
button {
    background: #009579;
    transition: background 0.3s ease;
}

button:hover {
    background: #007a63;
}
```
<!-- exercise-end -->

## Lesson 3: Adding Logic with JavaScript

This is where the magic happens! JavaScript brings your static page to life.

### Key Concepts
- **DOM Manipulation**: Changing HTML elements with JavaScript
- **Event Listeners**: Responding to user actions
- **Functions**: Reusable blocks of code

### Step 1: Get References to HTML Elements

First, we need to tell our JavaScript which HTML elements we want to control.

```javascript
// script.js
const inputBox = document.getElementById("input-box");
const listContainer = document.getElementById("list-container");
```

### Step 2: Adding a Task

We create a function called `addTask()` that runs when the "Add" button is clicked.

```javascript
// script.js
function addTask() {
    if (inputBox.value === '') {
        alert("You must write something!");
    } else {
        let li = document.createElement("li"); // Create a new <li> element
        li.innerHTML = inputBox.value; // Set its text to the input value
        listContainer.appendChild(li); // Add it to our list
        let span = document.createElement("span"); // Create a 'delete' button
        span.innerHTML = "\u00d7";
        li.appendChild(span);
    }
    inputBox.value = ""; // Clear the input box
    saveData(); // Save the new list
}
```

### Step 3: Marking Tasks and Deleting Them

We listen for clicks on the entire list. This is more efficient than adding a listener to every single task.

```javascript
// script.js
listContainer.addEventListener("click", function(e) {
    if (e.target.tagName === "LI") {
        e.target.classList.toggle("checked"); // Toggle complete/incomplete
        saveData();
    } else if (e.target.tagName === "SPAN") {
        e.target.parentElement.remove(); // Delete the task
        saveData();
    }
}, false);
```

### Step 4: Saving and Loading Data

To make sure our tasks don't disappear when we refresh the page, we use the browser's `localStorage`.

```javascript
// script.js
function saveData() {
    // Save the list's HTML to localStorage
    localStorage.setItem("data", listContainer.innerHTML);
}

function showTask() {
    // Load the HTML from localStorage
    listContainer.innerHTML = localStorage.getItem("data");
}

showTask(); // Load tasks when the page opens
```

### 💡 Exercise 3.1: Your First JavaScript Function
<!-- exercise-start id="first-js-function" -->
**Objective**: Create a function that counts the number of tasks.

**Task**: Write a function called `updateTaskCount()` that counts all `<li>` elements and updates the task counter.

**Starter Code**:
```javascript
function updateTaskCount() {
    // 1. Get all li elements
    // 2. Count them
    // 3. Update the task-count span
}
```

**Hint**: Use `document.querySelectorAll('li').length` to count tasks.

**Solution**:
```javascript
function updateTaskCount() {
    const tasks = document.querySelectorAll('#list-container li');
    const count = tasks.length;
    const taskCountElement = document.getElementById('task-count');
    
    if (taskCountElement) {
        const taskText = count === 1 ? 'task' : 'tasks';
        taskCountElement.textContent = `${count} ${taskText} remaining`;
    }
}
```
<!-- exercise-end -->

### 💡 Exercise 3.2: Adding Task Validation
<!-- exercise-start id="task-validation" -->
**Objective**: Improve the `addTask()` function to prevent duplicate tasks.

**Task**: Modify the function to check if a task already exists before adding it.

**Requirements**:
- Check if the task text already exists in the list
- Show an alert if it's a duplicate
- The check should be case-insensitive

**Starter Code**:
```javascript
function addTask() {
    if (inputBox.value === '') {
        alert("You must write something!");
    } else {
        // Add duplicate check here
        
        let li = document.createElement("li");
        li.innerHTML = inputBox.value;
        listContainer.appendChild(li);
        // ... rest of the code
    }
}
```

**Solution**:
```javascript
function addTask() {
    if (inputBox.value === '') {
        alert("You must write something!");
    } else {
        // Check for duplicates
        const existingTasks = document.querySelectorAll('#list-container li');
        const newTaskText = inputBox.value.trim().toLowerCase();
        
        for (let task of existingTasks) {
            const taskText = task.firstChild.textContent.toLowerCase();
            if (taskText === newTaskText) {
                alert("This task already exists!");
                return;
            }
        }
        
        let li = document.createElement("li");
        li.innerHTML = inputBox.value;
        listContainer.appendChild(li);
        let span = document.createElement("span");
        span.innerHTML = "\u00d7";
        li.appendChild(span);
    }
    inputBox.value = "";
    saveData();
    updateTaskCount(); // Update the count
}
```
<!-- exercise-end -->

## Lesson 4: Making Your App Accessible (A11y)

Web accessibility (often shortened to "a11y") means making sure your website can be used by everyone, including people with disabilities who may use screen readers or other assistive technologies. In many places, this is a legal requirement.

**ARIA** (Accessible Rich Internet Applications) helps us make web apps more accessible by adding special attributes to our HTML. Your `index.html` file already uses some of these!

### Step 1: Use ARIA Labels for Clarity

Notice the `aria-label` attributes on the buttons in your `index.html`. For example:

```html
<button class="filter-btn" data-filter="active" aria-label="Show active tasks">Active</button>
```

While the button text says "Active", the `aria-label` gives a more descriptive instruction ("Show active tasks") for screen readers. This makes the button's purpose much clearer to users of assistive technology. Your application already does this for the filter buttons and the "Clear Completed" button.

We can also improve the "delete" button that gets added with each task.

### Step 2: Improve the HTML and JavaScript for New Tasks

Let's make our dynamically added tasks more accessible.

1.  **Use buttons for actions**: The "delete" icon should be a proper `<button>` so it's focusable and announced correctly by screen readers.
2.  **Add a descriptive `aria-label`**: Just like our other buttons, the delete button needs a clear label.

Here is the updated `addTask` function for `script.js` that creates an accessible delete button.

```javascript
// script.js
function addTask() {
    if (inputBox.value === '') {
        alert("You must write something!");
    } else {
        let li = document.createElement("li");
        li.textContent = inputBox.value; // Use textContent for security
        listContainer.appendChild(li);
        
        let button = document.createElement("button"); // Use a <button>
        button.innerHTML = "\u00d7"; // This is the 'x' character
        button.setAttribute("aria-label", "Delete task: " + inputBox.value); // Important for screen readers
        li.appendChild(button);
    }
    inputBox.value = "";
    saveData();
}
```

Finally, update the event listener to look for a `BUTTON` tag for deletion.

```javascript
// script.js
listContainer.addEventListener("click", function(e) {
    if (e.target.tagName === "LI") {
        e.target.classList.toggle("checked");
        saveData();
    } else if (e.target.tagName === "BUTTON") { // Check for BUTTON now
        e.target.parentElement.remove();
        saveData();
    }
}, false);
```

These small changes make a huge difference in usability for many people!

Congratulations! You now have a fully functional ToDo application and understand the basic principles behind it.

## Lesson 5: Advanced Features

Let's add some cool features to make our app even better!

### 💡 Exercise 5.1: Add Task Categories
<!-- exercise-start id="task-categories" -->
**Objective**: Add a dropdown to categorize tasks (Work, Personal, Shopping).

**Task**: 
1. Add a `<select>` element to the HTML
2. Modify `addTask()` to include the category
3. Display the category with each task

**Starter HTML**:
```html
<div class="row">
    <input type="text" id="input-box" placeholder="Add your text">
    <!-- Add select here -->
    <button onclick="addTask()">Add</button>
</div>
```

**Solution HTML**:
```html
<div class="row">
    <input type="text" id="input-box" placeholder="Add your text">
    <select id="category-select">
        <option value="general">General</option>
        <option value="work">Work</option>
        <option value="personal">Personal</option>
        <option value="shopping">Shopping</option>
    </select>
    <button onclick="addTask()">Add</button>
</div>
```

**Solution JavaScript**:
```javascript
function addTask() {
    if (inputBox.value === '') {
        alert("You must write something!");
    } else {
        const category = document.getElementById('category-select').value;
        let li = document.createElement("li");
        li.innerHTML = `<span class="category-tag ${category}">${category}</span> ${inputBox.value}`;
        li.setAttribute('data-category', category);
        listContainer.appendChild(li);
        
        let button = document.createElement("button");
        button.innerHTML = "\u00d7";
        button.setAttribute("aria-label", "Delete task: " + inputBox.value);
        li.appendChild(button);
    }
    inputBox.value = "";
    saveData();
}
```
<!-- exercise-end -->

### 💡 Exercise 5.2: Filter by Category
<!-- exercise-start id="filter-by-category" -->
**Objective**: Add buttons to filter tasks by category.

**Task**: Create filter buttons that show/hide tasks based on their category.

**Requirements**:
- Add filter buttons for each category
- Add an "All" button to show everything
- Highlight the active filter

**Solution**:
```javascript
function filterByCategory(category) {
    const tasks = document.querySelectorAll('#list-container li');
    
    tasks.forEach(task => {
        if (category === 'all' || task.getAttribute('data-category') === category) {
            task.style.display = 'flex';
        } else {
            task.style.display = 'none';
        }
    });
    
    // Update active filter button
    document.querySelectorAll('.filter-btn').forEach(btn => {
        btn.classList.remove('active');
    });
    event.target.classList.add('active');
}
```
<!-- exercise-end -->

## Project Challenge 🏆
<!-- challenge-start id="final-project" -->
**Build a Complete ToDo App with These Features**:

1. ✅ Add and delete tasks
2. ✅ Mark tasks as complete
3. ✅ Save tasks to localStorage
4. ✅ Task counter
5. ✅ Duplicate prevention
6. ✅ Categories
7. 🎯 Due dates (bonus)
8. 🎯 Priority levels (bonus)
9. 🎯 Search functionality (bonus)

**Evaluation Criteria**:
- All basic features work correctly
- Code is clean and well-commented
- App is accessible (ARIA labels, keyboard navigation)
- Responsive design for mobile devices
- At least one bonus feature implemented
<!-- challenge-end -->

## Conclusion

Congratulations! 🎉 You've learned the fundamentals of web development by building a real, working application. You now understand:

- How HTML provides structure
- How CSS makes things beautiful
- How JavaScript adds interactivity
- How to store data locally
- How to make apps accessible

### Next Steps
1. Try adding the bonus features from the challenge
2. Experiment with different designs
3. Learn about frameworks like React or Vue
4. Deploy your app online using GitHub Pages

Keep coding and building amazing things! 💪

## Quick Reference

### Useful JavaScript Methods
```javascript
// DOM Selection
document.getElementById('id')
document.querySelector('.class')
document.querySelectorAll('li')

// Element Creation
document.createElement('div')
element.appendChild(child)
element.remove()

// Events
element.addEventListener('click', function)
element.onclick = function

// Storage
localStorage.setItem('key', 'value')
localStorage.getItem('key')
```

### CSS Properties Cheat Sheet
```css
/* Layout */
display: flex;
justify-content: center;
align-items: center;

/* Spacing */
margin: 10px;
padding: 10px;

/* Typography */
font-size: 16px;
color: #333;
text-align: center;

/* Effects */
transition: all 0.3s;
box-shadow: 0 2px 5px rgba(0,0,0,0.1);
```