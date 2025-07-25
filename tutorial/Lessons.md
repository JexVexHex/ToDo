# Beginner's Tutorial: Build a ToDo App

Welcome! This tutorial will guide you through building a simple ToDo List application using HTML, CSS, and JavaScript.

## Lesson 1: Setting Up the HTML Structure

Every web page starts with HTML. It provides the basic structure.

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

## Lesson 2: Making it Look Good with CSS

CSS (`style.css`) is used to style our HTML elements.

-   We style the `body`, `.container`, and `.todo-app` to center the application and give it a nice background.
-   The input field and button are styled to look like a single unit.
-   We define a `.checked` class for tasks that are completed. This class adds a line-through style and changes the checkmark image.

## Lesson 3: Adding Logic with JavaScript

This is where the magic happens! We'll use `script.js` to make our app interactive.

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

Congratulations! You now have a fully functional ToDo application and understand the basic principles behind it.