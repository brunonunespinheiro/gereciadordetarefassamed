



<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gerenciador de Tarefas</title>
  <style>
    body {
      font-family: Agrandir, Quicksand;
      background-color: #0b48c5;
      margin: 50;
      padding: 50;
      color: var(--text-color, #333); /* Cor do texto */
    }
        #header {
      text-align: center;
      padding: 10px 0;
      background-color: var(--header-background, #3498db); /* Cor de fundo do cabeçalho */
    }

    #logo {
      display: inline-block;
      text-decoration: none;
      color: var(--logo-color, #fff); /* Cor do texto do logo */
      font-size: 24px;
    }

    #container {
      max-width: 500px;
      margin: 100px auto;
      background-color: var(--background-color, #fff); /* Cor de fundo */
      padding: 20px;
      border-radius: var(--border-radius, 8px); /* Raio da borda */
      box-shadow: var(--box-shadow, 0 0 10px rgba(0, 0, 0, 0.1)); /* Sombra da caixa */
    }

    input, button {
      padding: 8px;
      margin-bottom: 10px;
      display: block;
      font-size: var(--font-size, 17px); /* Tamanho da fonte */
      border: var(--input-border, 5px solid #ddd); /* Borda da entrada */
    }

    button {
      background-color: var(--button-background, #e32f2f); /* Cor de fundo do botão */
      color: var(--button-color, white); /* Cor do texto do botão */
      border: none;
      cursor: pointer;
    }

    button:hover {
      background-color: var(--button-hover-background, #0b48c5); /* Cor de fundo do botão ao passar o mouse */
    }

    .task {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 8px;
      border-bottom: var(--task-border, 1px solid #ddd); /* Borda da tarefa */
    }

    .completed {
      text-decoration: line-through;
      color: var(--completed-color, #888); /* Cor do texto de tarefa concluída */
    }

    .countdown {
      font-size: var(--countdown-font-size, 14px); /* Tamanho da fonte do contador */
      color: var(--countdown-color, #e74c3c); /* Cor do texto do contador */
    }
  </style>
</head>
<body>

  <div id="container">
    <h2>Gerenciador de Tarefas</h2>
    <input type="text" id="taskInput" placeholder="Tarefa">
    <input type="datetime-local" id="deadlineInput">
    <button onclick="addTask()">Adicionar</button>
    <button onclick="resetList()">Reiniciar Lista</button>

    <div id="taskList"></div>
  </div>

  <script>
    const tasks = [];

    function addTask() {
      const taskInput = document.getElementById('taskInput');
      const deadlineInput = document.getElementById('deadlineInput');
      
      const taskText = taskInput.value.trim();
      const deadline = deadlineInput.value;

      if (taskText !== '') {
        const newTask = { text: taskText, deadline, completed: false };
        tasks.push(newTask);
        taskInput.value = '';
        deadlineInput.value = '';
        renderTasks();
      }
    }

    function toggleTask(index) {
      tasks[index].completed = !tasks[index].completed;
      renderTasks();
    }

    function resetList() {
      tasks.length = 0;
      renderTasks();
    }

    function renderTasks() {
      const taskList = document.getElementById('taskList');
      taskList.innerHTML = '';

      tasks.forEach((task, index) => {
        const taskElement = document.createElement('div');
        taskElement.classList.add('task');

        const taskText = document.createElement('span');
        taskText.textContent = task.text + (task.deadline ? ` - Prazo: ${task.deadline}` : '');
        if (task.completed) {
          taskText.classList.add('completed');
        }

        const taskButton = document.createElement('button');
        taskButton.textContent = task.completed ? 'Reabrir' : 'Concluir';
        taskButton.onclick = () => toggleTask(index);

        const countdown = document.createElement('span');
        countdown.classList.add('countdown');
        if (task.deadline) {
          countdown.textContent = getTimeRemaining(task.deadline);
          setInterval(() => {
            countdown.textContent = getTimeRemaining(task.deadline);
          }, 1000);
        }

        taskElement.appendChild(taskText);
        taskElement.appendChild(taskButton);
        taskElement.appendChild(countdown);

        taskList.appendChild(taskElement);
      });
    }

    function getTimeRemaining(endtime) {
      const total = Date.parse(endtime) - Date.now();
      const seconds = Math.floor((total / 1000) % 60);
      const minutes = Math.floor((total / 1000 / 60) % 60);
      const hours = Math.floor((total / (1000 * 60 * 60)) % 24);
      const days = Math.floor(total / (1000 * 60 * 60 * 24));

      if (total <= 0) {
        return 'Prazo Expirado';
      }

      return `${days}d ${hours}h ${minutes}m ${seconds}s`;
    }

    // Initial render
    renderTasks();
  </script>

</body>
</html>
