Here’s a basic outline of how to build a Task Management Web App with the features you requested using HTML, CSS, and JavaScript. I’ll break it down step-by-step:

1. HTML Structure
Create a basic HTML file to define the structure for adding, editing, deleting, and completing tasks.

html
Copy
Edit
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Management</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="task-container">
        <h1>Task Management</h1>
        <input type="text" id="task-input" placeholder="Add a task">
        <button onclick="addTask()">Add Task</button>
        <ul id="task-list"></ul>
    </div>
    <script src="script.js"></script>
</body>
</html>
2. CSS for Styling (styles.css)
Here’s some basic styling to make it look clean and functional:

css
Copy
Edit
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.task-container {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

input {
    padding: 10px;
    width: 250px;
    margin-right: 10px;
}

button {
    padding: 10px;
    background-color: #5cb85c;
    color: white;
    border: none;
    cursor: pointer;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: flex;
    justify-content: space-between;
    margin: 10px 0;
    padding: 10px;
    background-color: #f8f8f8;
    border-radius: 4px;
}

.completed {
    text-decoration: line-through;
    color: gray;
}
3. JavaScript (script.js)
This JavaScript handles adding, editing, deleting, completing tasks, and saving the task list to local storage.

javascript
Copy
Edit
// Load tasks from localStorage
window.onload = function() {
    loadTasks();
};

// Add a new task
function addTask() {
    const taskInput = document.getElementById("task-input");
    const taskValue = taskInput.value.trim();
    
    if (taskValue) {
        let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
        const newTask = { text: taskValue, completed: false };
        tasks.push(newTask);
        localStorage.setItem('tasks', JSON.stringify(tasks));
        taskInput.value = '';
        loadTasks();
    }
}

// Load tasks from localStorage
function loadTasks() {
    const taskList = document.getElementById("task-list");
    taskList.innerHTML = ''; // Clear the list before reloading

    const tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    tasks.forEach((task, index) => {
        const li = document.createElement("li");
        li.classList.toggle("completed", task.completed);
        li.innerHTML = `
            <span onclick="toggleCompletion(${index})">${task.text}</span>
            <button onclick="deleteTask(${index})">Delete</button>
        `;
        taskList.appendChild(li);
    });
}

// Mark task as completed or uncompleted
function toggleCompletion(index) {
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    tasks[index].completed = !tasks[index].completed;
    localStorage.setItem('tasks', JSON.stringify(tasks));
    loadTasks();
}

// Delete a task
function deleteTask(index) {
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    tasks.splice(index, 1);
    localStorage.setItem('tasks', JSON.stringify(tasks));
    loadTasks();
}
