# Myinternshipproject4.1
To-do list
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <style>
        body {
            background-color: #f7ecd8;
            font-family: 'Arial', sans-serif;
        }

        #heading {
            text-align: center;
            margin-top: 20px;
        }

        #input1 {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
        }

        #li1 li {
            position: relative;
            padding: 20px;
            border: 2px solid #333;
            margin: 5px;
            border-radius: 15px;
            overflow: hidden;
            font-family: 'cursive';
            background-color: #fff;
            transition: background-color 0.3s ease;
        }

        #li1 li.completed {
            text-decoration: line-through;
            background-color: #e0e0e0;
        }

        .edit1, .delbtn, .statusBtn {
            background-color: #4285f4;
            color: white;
            border-radius: 20px;
            padding: 5px 15px;
            font-weight: 500;
            cursor: pointer;
            margin-right: 5px;
            border: none;
            outline: none;
        }

        .edit1:hover, .delbtn:hover, .statusBtn:hover {
            background-color: #2962ff;
        }

        .statusBtn {
            position: absolute;
            top: 50%;
            right: 70px;
            transform: translateY(-50%);
        }

        .checkbox {
            margin-right: 10px;
        }

        #in1 {
            border-radius: 6px;
            padding: 10px 15px;
            font-size: 15px;
            font-weight: 500;
            background-color: #fff;
            border: 1px solid #ccc;
        }

        #add1 {
            background-color: #4285f4;
            color: white;
            border-radius: 20px;
            padding: 5px 15px;
            cursor: pointer;
            border: none;
            outline: none;
        }

        #add1:hover {
            background-color: #2962ff;
        }
    </style>
</head>
<body>
    <div id="heading">
        <h1>To-Do List</h1>
    </div>
    <div id="input1">
        <input type="text" placeholder="Write anything you want!!" id="in1">
        <button id="add1">ADD</button>
    </div>
    <div id="list1">
        <ul id="li1"></ul>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const input1 = document.getElementById('in1');
            const btn1 = document.getElementById('add1');
            const list1 = document.getElementById('li1');

            const task = JSON.parse(localStorage.getItem('ayu')) || [];

            function savetask() {
                localStorage.setItem('ayu', JSON.stringify(task));
            }

            function displaytask() {
                list1.innerHTML = '';
                task.forEach((task1, index) => {
                    const li = document.createElement('li');
                    const checkbox = document.createElement('input');
                    checkbox.type = 'checkbox';
                    checkbox.checked = task1.startsWith("[Completed] ");
                    checkbox.classList.add('checkbox');
                    checkbox.addEventListener('change', () => {
                        // Toggle completed status when the checkbox is changed
                        task[index] = checkbox.checked
                            ? "[Completed] " + task1
                            : task1.replace("[Completed] ", "");
                        savetask();
                        displaytask();
                    });

                    li.appendChild(checkbox);

                    li.textContent = task1.replace("[Completed] ", "");
                    li.classList.toggle('completed', task1.startsWith("[Completed] "));

                    const editbtn = document.createElement('button');
                    editbtn.textContent = 'Edit';
                    editbtn.classList.add('edit1');
                    editbtn.addEventListener('click', () => {
                        const newtext = prompt('Edit the task : ', task1.replace("[Completed] ", ""));
                        if (newtext !== null) {
                            task[index] = checkbox.checked
                                ? "[Completed] " + newtext
                                : newtext;
                            savetask();
                            displaytask();
                        }
                    });

                    const delbtn = document.createElement('button');
                    delbtn.textContent = 'Delete';
                    delbtn.classList.add('delbtn');
                    delbtn.addEventListener('click', () => {
                        task.splice(index, 1);
                        savetask();
                        displaytask();
                    });

                    const statusBtn = document.createElement('button');
                    statusBtn.textContent = checkbox.checked ? 'Mark as Incomplete' : 'Mark as Completed';
                    statusBtn.classList.add('statusBtn');
                    statusBtn.addEventListener('click', () => {
                        // Toggle completed status
                        task[index] = checkbox.checked
                            ? task1.replace("[Completed] ", "")
                            : "[Completed] " + task1;
                        savetask();
                        displaytask();
                    });

                    li.appendChild(editbtn);
                    li.appendChild(delbtn);
                    li.appendChild(statusBtn);

                    list1.appendChild(li);
                });
            }

            btn1.addEventListener('click', function () {
                const text = input1.value.trim();
                if (text !== '') {
                    task.push(text);
                    savetask();
                    displaytask();
                    input1.value = '';
                }
            });

            displaytask();
        });
    </script>
</body>
</html>
