# ToDo App Technical Specification

This document outlines the functionality, data flow, and techniques used in this simple ToDo list application.

## 1. Core Components

The application is built with three core files:

-   `index.html`: Provides the structure and layout of the user interface.
-   `style.css`: Adds styling to make the application visually appealing.
-   `script.js`: Contains all the logic for adding, deleting, and marking tasks as complete.

## 2. JavaScript Functions

The core logic resides in `script.js`.

-   `addTask()`: Triggered when the "Add" button is clicked. It reads the text from the input field, creates a new list item (`<li>`), and appends it to the task list. It then clears the input field and saves the updated list.
-   `saveData()`: Saves the current state of the task list to the browser's `localStorage`. This ensures that tasks are not lost when the page is reloaded.
-   `showTask()`: Loads the tasks from `localStorage` when the application is first opened.
-   `listContainer.addEventListener('click', ...)`: An event listener on the task list that handles two actions:
    1.  **Toggling Completion**: If a task (`<li>`) is clicked, it toggles a `checked` class to visually mark it as complete.
    2.  **Deleting a Task**: If the 'x' (`<span>`) on a task is clicked, it removes the entire task from the list.

## 3. Data Flow

The application's data flow is straightforward:

1.  **Page Load**: `showTask()` is called to retrieve any saved tasks from `localStorage` and display them.
2.  **Adding a Task**: The user types a task and clicks "Add". The `addTask()` function creates a new HTML element for the task and adds it to the DOM.
3.  **State Change**: Any change to the list (adding, completing, or deleting a task) triggers the `saveData()` function.
4.  **Persistence**: `saveData()` serializes the HTML content of the task list and stores it in `localStorage` under the key `"data"`.

## 4. Key Techniques

-   **DOM Manipulation**: The application dynamically creates, modifies, and deletes HTML elements (`<li>`, `<span>`) using JavaScript.
-   **Event Handling**: It uses `addEventListener` to react to user clicks for adding, completing, and deleting tasks. Event delegation is used on the list container to efficiently handle clicks on any task.
-   **Local Storage**: `localStorage` is used for data persistence, allowing the app to remember the user's tasks between sessions.