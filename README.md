<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Мой список задач ✨</title>

  <!-- React + ReactDOM -->
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

  <!-- Babel -->
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

  <!-- Шрифт -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">

  <style>
    body {
      margin: 0;
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, rgba(147,112,219,0.7), rgba(0,191,255,0.6)),
                  url('https://images.unsplash.com/photo-1529101091764-c3526daf38fe?auto=format&fit=crop&w=1950&q=80') no-repeat center/cover;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
      animation: fadeIn 1s ease-in;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: scale(1.03); }
      to { opacity: 1; transform: scale(1); }
    }

    .container {
      width: 430px;
      background: rgba(255,255,255,0.9);
      border-radius: 20px;
      box-shadow: 0 8px 25px rgba(0,0,0,0.25);
      padding: 30px;
      backdrop-filter: blur(8px);
      animation: slideIn 0.8s ease;
      position: relative;
    }

    @keyframes slideIn {
      from { transform: translateY(50px); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }

    .emblem {
      position: fixed;
      top: 15px;
      right: 20px;
      width: 65px;
      height: 65px;
      border-radius: 50%;
      border: 3px solid #fff;
      overflow: hidden;
      box-shadow: 0 0 12px rgba(0,0,0,0.4);
      animation: spinIn 1s ease;
      transition: transform 0.5s;
    }

    .emblem:hover {
      transform: rotate(10deg) scale(1.1);
    }

    .emblem img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    h1 {
      text-align: center;
      color: #4b0082;
      text-shadow: 0 1px 2px rgba(0,0,0,0.2);
      margin-bottom: 10px;
    }

    .lang-switch {
      display: flex;
      justify-content: flex-end;
      margin-bottom: 10px;
    }

    .lang-switch button {
      background: linear-gradient(90deg, #7b68ee, #00bfff);
      color: white;
      border: none;
      padding: 7px 14px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 13px;
      transition: 0.3s;
    }

    .lang-switch button:hover {
      transform: scale(1.05);
      box-shadow: 0 0 8px #7b68ee;
    }

    form {
      display: flex;
      margin-bottom: 20px;
    }

    input {
      flex: 1;
      padding: 10px;
      font-size: 15px;
      border: 2px solid #7b68ee;
      border-radius: 8px 0 0 8px;
      outline: none;
      background: #f9f8ff;
    }

    button.add {
      background: linear-gradient(90deg, #7b68ee, #00bfff);
      color: white;
      border: none;
      border-radius: 0 8px 8px 0;
      cursor: pointer;
      padding: 10px 16px;
      font-size: 15px;
      transition: 0.3s;
    }

    button.add:hover {
      transform: scale(1.05);
      box-shadow: 0 0 10px #9370db;
    }

    ul {
      list-style: none;
      padding: 0;
      margin: 0;
    }

    li {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: white;
      border-left: 5px solid #7b68ee;
      border-radius: 10px;
      padding: 10px 15px;
      margin-bottom: 10px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      transition: transform 0.2s;
    }

    li:hover {
      transform: scale(1.02);
    }

    li span {
      flex: 1;
      font-size: 15px;
      color: #333;
      cursor: pointer;
    }

    li.done span {
      text-decoration: line-through;
      color: #777;
    }

    li input.edit {
      flex: 1;
      padding: 6px;
      font-size: 15px;
      border: 2px solid #00bfff;
      border-radius: 5px;
      outline: none;
    }

    li button {
      background: none;
      border: none;
      font-size: 18px;
      cursor: pointer;
    }

    li button.delete {
      color: #e74c3c;
    }

    li button.done {
      color: #2ecc71;
      margin-right: 8px;
    }

    .empty {
      text-align: center;
      color: #777;
      font-style: italic;
    }

    footer {
      text-align: center;
      font-size: 13px;
      color: #4b0082;
      margin-top: 10px;
      opacity: 0.8;
    }
  </style>
</head>

<body>
<!-- Эмблема -->
<div class="emblem">
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/56/Donald_Trump_official_portrait.jpg" alt="Trump">
  </div>
  

  <div id="root"></div>

  <script type="text/babel">
    const { useState, useEffect } = React;

    function App() {
      const [tasks, setTasks] = useState([]);
      const [newTask, setNewTask] = useState("");
      const [lang, setLang] = useState("ru");
      const [editIndex, setEditIndex] = useState(null);
      const [editText, setEditText] = useState("");

      const text = {
        ru: {
          title: "✨ Мой список задач",
          placeholder: "Введите задачу...",
          add: "Добавить",
          empty: "Нет задач",
          switchLang: "Switch to English",
          made: "Создано в Visual Studio"
        },
        en: {
          title: "✨ My To-Do List",
          placeholder: "Enter a task...",
          add: "Add",
          empty: "No tasks",
          switchLang: "Переключить на русский",
          made: "Built in Visual Studio"
        }
      };

      const t = text[lang];

      useEffect(() => {
        const saved = JSON.parse(localStorage.getItem("tasks"));
        if (saved) setTasks(saved);
      }, []);

      useEffect(() => {
        localStorage.setItem("tasks", JSON.stringify(tasks));
      }, [tasks]);

      const addTask = (e) => {
        e.preventDefault();
        if (!newTask.trim()) return;
        setTasks([...tasks, { text: newTask.trim(), done: false }]);
        setNewTask("");
      };

      const toggleDone = (index) => {
        setTasks(tasks.map((t, i) => i === index ? { ...t, done: !t.done } : t));
      };

      const removeTask = (index) => {
        setTasks(tasks.filter((_, i) => i !== index));
      };

      const startEdit = (index, text) => {
        setEditIndex(index);
        setEditText(text);
      };

      const saveEdit = (index) => {
        if (!editText.trim()) return;
        setTasks(tasks.map((t, i) => i === index ? { ...t, text: editText } : t));
        setEditIndex(null);
      };

      return (
        <div className="container">
          <div className="lang-switch">
            <button onClick={() => setLang(lang === "ru" ? "en" : "ru")}>
              {t.switchLang}
            </button>
          </div>

          <h1>{t.title}</h1>

          <form onSubmit={addTask}>
            <input
              type="text"
              value={newTask}
              onChange={(e) => setNewTask(e.target.value)}
              placeholder={t.placeholder}
            />
            <button type="submit" className="add">{t.add}</button>
          </form>

          <ul>
            {tasks.length === 0 ? (
              <p className="empty">{t.empty}</p>
            ) : (
              tasks.map((task, i) => (
                <li key={i} className={task.done ? "done" : ""}>
                  {editIndex === i ? (
                    <input
                      className="edit"
                      value={editText}
                      onChange={(e) => setEditText(e.target.value)}
                      onKeyDown={(e) => e.key === "Enter" && saveEdit(i)}
                      onBlur={() => saveEdit(i)}
                      autoFocus
                    />
                  ) : (
                    <span onDoubleClick={() => startEdit(i, task.text)}>
                      {task.text}
                    </span>
                  )}
                  <div>
                    <button className="done" onClick={() => toggleDone(i)}>✔</button>
                    <button className="delete" onClick={() => removeTask(i)}>❌</button>
                  </div>
                </li>
              ))
            )}
          </ul>

          <footer>React SPA • {t.made}</footer>
        </div>
      );
    }

    ReactDOM.createRoot(document.getElementById("root")).render(<App />);
  </script>
</body>
</html>
