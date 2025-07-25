# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a vanilla JavaScript ToDo application with no build system or dependencies. It's a simple client-side web application that runs entirely in the browser.

## Development Commands

Since this is a vanilla HTML/CSS/JavaScript project with no build system:

- **Run locally**: Open `index.html` directly in a web browser or use a simple HTTP server like `python -m http.server 8000`
- **No build step required**: All files are served as-is
- **No package manager**: No npm, yarn, or other package managers are used
- **No tests**: No test framework is currently set up

## Architecture

### Core Files
- `index.html`: Main HTML structure with ToDo app layout
- `script.js`: All JavaScript functionality including DOM manipulation, state management, and localStorage
- `styles.css`: Complete styling with responsive design and animations

### Key Components

**State Management**: 
- Uses browser localStorage for persistence
- Global `tasks` array holds all task objects
- Each task has: `id`, `text`, `completed`, `createdAt`

**Core Functions**:
- `addTask()`: Creates new tasks from input
- `toggleTask(id)`: Toggles completion status
- `deleteTask(id)`: Removes tasks
- `renderTasks()`: Updates DOM with current tasks
- `getFilteredTasks()`: Handles filter logic (all/active/completed)

**Features**:
- Task filtering (all, active, completed)
- Double-click to edit tasks (uses `prompt()`)
- Clear completed tasks
- Sample tasks on first load
- Responsive design for mobile

## Development Notes

- All functionality is in vanilla JS with no frameworks
- Uses ES6+ features (arrow functions, template literals, destructuring)
- XSS protection via `escapeHtml()` function
- Event delegation for dynamically created elements
- LocalStorage for data persistence