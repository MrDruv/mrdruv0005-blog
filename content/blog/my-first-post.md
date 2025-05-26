---
title: "How I Built a Secure To-Do App with XSS Protection"

date: 2025-05-23T12:00:00Z
draft: false
---

Hey folks! 👋 <br>
In this post, I’ll walk you through how I built a simple to-do list app using just plain JavaScript — no frameworks, no dependencies, just vanilla JS. I’ll also touch on how I made it safe from common security threats like XSS (Cross-Site Scripting).

Let’s dive in!

### The Goal

I wanted to create a To-do list where:

- Users can enter the tasks and click enter to add it.
- Clicking on tasks reveals extra options like setting due dates and adding notes .
- Users can even delete the tasks.

### Key Concepts

The app covers:

- DOM manipulation
- Event handling
- Dynamic rendering
- Input validation
- XSS protection via HTML escaping

Let give quick intro on this concepts.

**1.DOM manipulation:**
DOM(Document Object Model) Manipulation refers to changing HTML or content of web page using Javascript.You can add,remove or modify elements dynamically after page loaded.

**2. Event handling:**
Event handling means listening for actions like clicks,typing and form submissions, and then responding to them with code.

**3. Dynamic rendering:**
Dynamic rendering means updating parts of the web page automatically as the user interacts, without reloading the page.

**4. Input validation:**
Input validation checks if user input is correct before processing it — like making sure a name field isn't empty or an email is formatted properly.

**5. XSS Protection via HTML escaping:**
Cross-Site Scripting (XSS) is when attackers inject malicious JavaScript into your site. To prevent this, you escape special characters in user input before adding it to the DOM.

### The HTML Structure

I use two main elements in DOM:

```javascript
<input id="taskInput" type="text" placeholder="Add a task..." />
<ul id="taskList"></ul>
```

taskInput is the textfield, and the taskList is where tasks get listed.

Code: before using escapeHtml

```javascript
const taskInput = document.getElementById("taskInput");
const taskList = document.getElementById("taskList");

// stores todos here
const todos = [];

function render(todos, taskList) {
  taskList.innerHTML = "";

  todos.forEach((task) => {
    const li = document.createElement("li");
    li.innerHTML = `
            <div class="task-header">
                <label>
                <input type="checkbox">
                <span>${task.text}</span>
                </label>
            </div>
            <div class="task-details" style="display: none;">
            <div class="task-meta">
              Added on: ${new Date().toLocaleDateString()} <br />
              <label>Due-Date</label>
              <input id = "due-date" type="date">
              <label>notes</label>
              <textarea id="notesInput" placeholder="Add notes..."></textarea><br>
            </div>
            <button class="btn-delete" data-id="${task.id}">Delete</button>
            </div>
        `;

    li.addEventListener("click", () => {
      const details = li.querySelector(".task-details");
      details.style.display = "block";
    });
    li.addEventListener("dblclick", () => {
      const details = li.querySelector(".task-details");
      details.style.display = "none";
    });

    taskList.appendChild(li);
  });
}

function addTask(taskInput, todos, taskList) {
  const taskText = taskInput.value.trim();
  if (taskText == "") {
    alert("Please enter the task");
    return;
  }

  const newTask = {
    id: todos.length + 1,
    text: taskText,
  };
  todos.push(newTask); // add to array
  render(todos, taskList); //render updated list
  taskInput.value = "";
}

document.addEventListener("DOMContentLoaded", () => {
  //Enter key
  taskInput.addEventListener("keydown", (e) => {
    if (e.key === "Enter") {
      addTask(taskInput, todos, taskList);
    }
  });
});
```

Here, it is open to the vulnerabilities.
when the code:

```javascript
<a href="www.spam.com">clickforamazonvoucher</a>
```

This prints code link in task-box.So users are easily open to XSS attack.
So to nuetralize these attacks we are using escapeHTML.Where browser displays scripts in text format.

### Handling XSS with escapeHTML()

**Dangerous Input**

```javascript
<a href="www.spam.com">clickamazonvoucher</a>
```

**Escaping it**

```javascript
function escapeHTML(str) {
  return str.replace(/[&<>"']/g, function (match) {
    const escapeChars = {
      "&": "&amp;",
      "<": "&lt;",
      ">": "&gt;",
      '"': "&quot;",
      "'": "&#039;",
    };
    return escapeChars[match];
  });
}
```

This is custom HTML sanitizer that prevents script injection.It escapes all the dangerous characters. So the browser shows text instead of executing a script.

now, using use escapeHTML(str) in task-header.

```javascript
<div class="task-header">
  <label>
    <input type="checkbox">
      <span>${escapeHTML(task.text)}</span>
  </label>
</div>
```

### Adding Tasks

When the user presses Enter, this function is triggered:

```javascript
function addTask(taskInput, todos, taskList) {
  const taskText = taskInput.value.trim();
  if (taskText == "") {
    alert("Please enter the task");
    return;
  }

  const newTask = {
    id: todos.length + 1,
    text: taskText,
  };
  todos.push(newTask);
  render(todos, taskList);
  taskInput.value = "";
}
```

### Rendering Tasks

Here’s the core of the UI:

```javascript
function render(todos, taskList) {
  taskList.innerHTML = "";

  todos.forEach((task) => {
    const li = document.createElement("li");
    li.innerHTML = `
      <div class="task-header">
        <label>
          <input type="checkbox">
          <span>${escapeHTML(task.text)}</span>
        </label>
      </div>
      <div class="task-details" style="display: none;">
        <div class="task-meta">
          Added on: ${new Date().toLocaleDateString()} <br />
          <label>Due-Date</label>
          <input id="due-date" type="date">
          <label>notes</label>
          <textarea id="notesInput" placeholder="Add notes..."></textarea><br>
        </div>
        <button class="btn-delete" data-id="${task.id}">Delete</button>
      </div>
    `;

    li.addEventListener("click", () => {
      const details = li.querySelector(".task-details");
      details.style.display = "block";
    });
    li.addEventListener("dblclick", () => {
      const details = li.querySelector(".task-details");
      details.style.display = "none";
    });

    taskList.appendChild(li);
  });
}
```

This builds out each task, includes a checkbox, due date field, notes input, and a delete button. I also added a fun interaction — click to open, double-click to close the details section!

### Wrapping Up

Here’s what I learned while building this:

- Always sanitize user input!
- DOM manipulation is powerful when done cleanly.
- Vanilla JavaScript is more than enough for small, effective apps.

Thanks for reading!
Let’s keep building — securely!

-MrDruv
