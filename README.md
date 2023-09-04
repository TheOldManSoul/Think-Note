<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Think Note</title>
    <style>
        body {
            background-color: black;
            color: red;
            font-family: Arial, sans-serif;
        }

        #container {
            display: flex;
            justify-content: space-between;
            padding: 20px;
        }

        #todo-list {
            flex: 1;
            margin-right: 20px;
        }

        #completed-failed-list {
            flex: 1;
            font-size: 14px;
        }

        .task {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .task span {
            flex: 1;
            padding: 5px;
            border: 1px solid red;
            margin-right: 10px;
        }

        .task .completed {
            background-color: green;
            color: white;
        }

        .task .failed {
            background-color: red;
            color: white;
        }

        #add-task {
            margin-bottom: 10px;
        }

        #task-input {
            background-color: black;
            color: red;
            border: 1px solid red;
            padding: 5px;
        }

        #website-title {
            text-align: center;
            font-size: 24px;
            margin-top: 10px;
        }

        #copyright {
            position: absolute;
            bottom: 10px;
            left: 10px;
            font-size: 12px;
        }

        #add-button {
            background-color: green;
            color: white;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div id="website-title">
        <h1>Think Note</h1>
    </div>
    <div id="container">
        <div id="todo-list">
            <h2>To-Do List</h2>
            <div id="add-task">
                <textarea id="task-input" placeholder="Enter a new task"></textarea>
                <button id="add-button">Add</button>
            </div>
            <div class="task">
                <span>Example</span>
                <button class="completed">✅</button>
                <button class="failed">❌</button>
            </div>
            <!-- Add more tasks as needed -->
        </div>
        <div id="completed-failed-list">
            <h2>Task History</h2>
            <ul id="history-list"></ul>
        </div>
    </div>
    <div id="copyright">
        &copy; OMS
    </div>
    <script>
        // JavaScript code for handling task completion and failure
        const tasks = document.querySelectorAll('.task');
        const historyList = document.getElementById('history-list');
        const taskInput = document.getElementById('task-input');
        const addButton = document.getElementById('add-button');

        // Delete one of the tasks
        tasks[0].remove();

        tasks.forEach(task => {
            const completedButton = task.querySelector('.completed');
            const failedButton = task.querySelector('.failed');
            const taskText = task.querySelector('span');

            completedButton.addEventListener('click', () => {
                taskText.classList.add('completed');
                taskText.textContent = 'Completed';
                setTimeout(() => {
                    task.remove();
                }, 10000);
                addToHistory(taskText.textContent, 'green');
            });

            failedButton.addEventListener('click', () => {
                taskText.classList.add('failed');
                taskText.textContent = 'Failed';
                addToHistory(taskText.textContent, 'blue');
            });
        });

        addButton.addEventListener('click', () => {
            const newTaskText = taskInput.value.trim();
            if (newTaskText !== '') {
                const newTask = document.createElement('div');
                newTask.classList.add('task');
                newTask.innerHTML = `
                    <span>${newTaskText}</span>
                    <button class="completed">✅</button>
                    <button class="failed">❌</button>
                `;
                document.getElementById('todo-list').appendChild(newTask);
                taskInput.value = '';

                // Add event listeners for the new task's buttons
                const completedButton = newTask.querySelector('.completed');
                const failedButton = newTask.querySelector('.failed');
                completedButton.addEventListener('click', () => {
                    newTask.querySelector('span').classList.add('completed');
                    newTask.querySelector('span').textContent = 'Completed';
                    setTimeout(() => {
                        newTask.remove();
                    }, 10000);
                    addToHistory(newTaskText + ' - Completed', 'green');
                });
                failedButton.addEventListener('click', () => {
                    newTask.querySelector('span').classList.add('failed');
                    newTask.querySelector('span').textContent = 'Failed';
                    addToHistory(newTaskText + ' - Failed', 'blue');
                });
            }
        });

        function addToHistory(text, color) {
            const listItem = document.createElement('li');
            const currentDate = new Date();
            const dateTimeString = currentDate.toLocaleString();
            listItem.style.color = color;
            listItem.innerHTML = `${text} (${dateTimeString})`;
            historyList.appendChild(listItem);
        }
    </script>
</body>
</html>
