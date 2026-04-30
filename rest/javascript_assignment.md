# Payroll System — CRUD Documentation
### A beginner-friendly guide to managing employees

---

## What is CRUD?

CRUD stands for the four basic things you can do with data:

| Letter | Meaning | Function in this system |
|--------|---------|------------------------|
| **C** | Create | `addEmployee()` |
| **R** | Read | `findEmployee()` and `listAllEmployees()` |
| **U** | Update | `updateEmployee()` |
| **D** | Delete | `removeEmployee()` |

---

## The Data

All employees are stored in one array at the top of the file:

```js
let employees = [];
let nextId    = 1;
```

> **Beginner tip:** An array (`[]`) is like a numbered list. Every time you add an employee, their record gets added to this list. `nextId` starts at 1 and goes up by 1 each time so every employee gets a unique ID.

---

## C — Create

### `addEmployee(name, department, basicSalary, hoursWorked)`

**What it does:** Creates a new employee record and saves it into the `employees` array.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | String | The employee's full name |
| `department` | String | Which department they work in |
| `basicSalary` | Number | Their monthly base salary in PKR |
| `hoursWorked` | Number | How many hours they worked this month |

**Returns:** The newly created employee object.

**Example:**
```js
addEmployee("Sara Khan", "HR", 50000, 176);
// Console output: Added: Sara Khan — ID: EMP1
```

**The employee record that gets created:**
```js
{
  id:          "EMP1",
  name:        "Sara Khan",
  department:  "HR",
  basicSalary: 50000,
  hoursWorked: 176,
  isActive:    true
}
```

> **Beginner tip:** The `id` is built by joining the text `"EMP"` with the current `nextId` number. After the employee is saved, `nextId++` adds 1 to it so the next employee gets a different ID.

**How to display the result on the page:**
```html
<p id="result"></p>
```
```js
let emp = addEmployee("Sara Khan", "HR", 50000, 176);

document.getElementById("result").textContent = "Added: " + emp.name + " with ID " + emp.id;
```

---

## R — Read

### `findEmployee(id)`

**What it does:** Loops through the `employees` array and returns the one employee whose `id` matches what you provide. Returns `null` if nobody is found.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | String | The employee ID to search for, e.g. `"EMP1"` |

**Returns:** The matching employee object, or `null`.

**Example:**
```js
let emp = findEmployee("EMP1");
// emp is now the Sara Khan object
```

> **Beginner tip:** This function uses a `for` loop to check each employee one by one. The `===` operator checks for an exact match — same value and same type.

**How to display the result on the page:**
```html
<p id="empName"></p>
<p id="empDept"></p>
```
```js
let emp = findEmployee("EMP1");

if (emp !== null) {
  document.getElementById("empName").textContent = "Name: " + emp.name;
  document.getElementById("empDept").textContent = "Department: " + emp.department;
} else {
  document.getElementById("empName").textContent = "Employee not found.";
}
```

---

### `listAllEmployees()`

**What it does:** Goes through the `employees` array and returns a new array of only the employees where `isActive` is `true`. Deactivated employees are skipped.

**Parameters:** None.

**Returns:** An array of active employee objects.

**Example:**
```js
let active = listAllEmployees();
// active is now an array of all non-deactivated employees
```

**How to display the result on the page using a list:**
```html
<ul id="employeeList"></ul>
```
```js
let active = listAllEmployees();
let list   = document.getElementById("employeeList");

list.innerHTML = ""; // clear the list first

for (let i = 0; i < active.length; i++) {
  list.innerHTML += "<li>" + active[i].id + " — " + active[i].name + " (" + active[i].department + ")" + "</li>";
}
```

> **Beginner tip:** `list.innerHTML = ""` clears whatever was in the list before. Then `+=` adds a new `<li>` item for each employee. If you used `innerText` here it would only show plain text — `innerHTML` lets you build real HTML tags.

---

## U — Update

### `updateEmployee(id, updates)`

**What it does:** Finds an employee by their ID and changes only the fields you pass in. All other fields stay exactly the same.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | String | The ID of the employee to update |
| `updates` | Object | An object with only the fields you want to change |

**Returns:** The updated employee object, or `null` if the employee was not found.

**Example:**
```js
updateEmployee("EMP2", { basicSalary: 85000 });
// Only the salary changes — name, department, etc. all stay the same

updateEmployee("EMP1", { department: "Finance" });
// Only the department changes
```

**How to display confirmation on the page:**
```html
<p id="updateMsg"></p>
```
```js
let emp = updateEmployee("EMP2", { basicSalary: 85000 });

if (emp !== null) {
  document.getElementById("updateMsg").textContent = emp.name + " salary updated to PKR " + emp.basicSalary;
} else {
  document.getElementById("updateMsg").textContent = "Update failed — employee not found.";
}
```

---

## D — Delete

### `removeEmployee(id)`

**What it does:** Marks an employee as inactive by setting their `isActive` field to `false`. It does **not** erase the record from the array — the history is kept.

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | String | The ID of the employee to deactivate |

**Returns:** `true` if the employee was found and deactivated, `false` if not found.

**Example:**
```js
removeEmployee("EMP3");
// Console output: Deactivated: Zara Malik
```

> **Beginner tip:** This is called a **soft delete**. The record stays in the array but `isActive` becomes `false`, so `listAllEmployees()` will skip over them. Real companies do this to keep a history of past employees.

**How to display confirmation on the page:**
```html
<p id="deleteMsg"></p>
```
```js
let success = removeEmployee("EMP3");

if (success) {
  document.getElementById("deleteMsg").innerText = "Employee EMP3 has been deactivated.";
} else {
  document.getElementById("deleteMsg").innerText = "Could not find employee EMP3.";
}
```

---

## DOM Quick Reference

These are the three ways to put content onto a webpage. Use the right one for the job:

| Property | What it does | Use it when |
|----------|-------------|-------------|
| `textContent` | Sets plain text — ignores any HTML tags | Displaying names, numbers, messages |
| `innerText` | Sets visible text — respects how the browser shows it | Similar to `textContent`, good for user-visible output |
| `innerHTML` | Sets text AND lets you write real HTML tags | Building lists, tables, or styled output |

**Example — the difference in practice:**
```js
let box = document.getElementById("box");

// These two look the same for simple text:
box.textContent = "Hello, Sara!";
box.innerText   = "Hello, Sara!";

// Only innerHTML can build HTML structure:
box.innerHTML   = "<strong>Hello</strong>, Sara!";  // "Hello" appears bold
box.textContent = "<strong>Hello</strong>, Sara!";  // shows the tag as plain text — not bold
```

> **Beginner tip:** When you are just showing data (names, IDs, numbers), always prefer `textContent`. Only switch to `innerHTML` when you need to build actual HTML like `<li>`, `<b>`, or `<br>` tags.

---


```