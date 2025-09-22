# focusforge
focus
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FocusForge</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" />
  <style>
    body {
      background: #f8f9fa;
      font-family: 'Segoe UI', sans-serif;
    }
    .focus-mode {
      margin-bottom: 1rem;
    }
    .timer-display {
      font-size: 2rem;
      font-weight: bold;
    }
    .badge-custom {
      font-size: 1rem;
      margin: 0.2rem;
    }
  </style>
</head>
<body class="container py-4">
  <h1 class="text-center mb-4">🧠 FocusForge</h1>

  <!-- Mood Tracker -->
  <div class="mb-4">
    <h4>Mood & Energy Tracker</h4>
    <select id="mood" class="form-select">
      <option value="energized">Energized</option>
      <option value="tired">Tired</option>
      <option value="stressed">Stressed</option>
      <option value="calm">Calm</option>
    </select>
    <button class="btn btn-primary mt-2" onclick="suggestFocus()">Suggest Focus Mode</button>
    <p id="focusSuggestion" class="mt-2 text-success fw-bold"></p>
  </div>

  <!-- Pomodoro Timer -->
  <div class="mb-4">
    <h4>Pomodoro Timer</h4>
    <div class="d-flex align-items-center gap-3">
      <span class="timer-display" id="timer">25:00</span>
      <button class="btn btn-success" onclick="startTimer()">Start</button>
      <button class="btn btn-danger" onclick="resetTimer()">Reset</button>
    </div>
  </div>

  <!-- Task Prioritizer -->
  <div class="mb-4">
    <h4>Task Prioritizer (ABCD Method)</h4>
    <input type="text" id="taskInput" class="form-control mb-2" placeholder="Enter a task..." />
    <select id="priority" class="form-select mb-2">
      <option value="A">A - Urgent & Important</option>
      <option value="B">B - Important but not Urgent</option>
      <option value="C">C - Urgent but not Important</option>
      <option value="D">D - Neither Urgent nor Important</option>
    </select>
    <button class="btn btn-secondary" onclick="addTask()">Add Task</button>
    <ul id="taskList" class="list-group mt-3"></ul>
  </div>

  <!-- Productivity Badges -->
  <div class="mb-4">
    <h4>🎖 Productivity Badges</h4>
    <div id="badges"></div>
  </div>

  <script>
    // Mood-based focus suggestion
    function suggestFocus() {
      const mood = document.getElementById("mood").value;
      const suggestion = {
        energized: "Try Deep Work mode with 50-minute focus blocks.",
        tired: "Use Quick Tasks mode with 15-minute bursts and longer breaks.",
        stressed: "Switch to Calm mode with ambient music and 20-minute focus.",
        calm: "Creative Flow mode is ideal — explore new ideas or journaling."
      };
      document.getElementById("focusSuggestion").textContent = suggestion[mood];
    }

    // Pomodoro Timer
    let timer;
    let timeLeft = 25 * 60;
    function startTimer() {
      clearInterval(timer);
      timer = setInterval(() => {
        if (timeLeft <= 0) {
          clearInterval(timer);
          alert("Time's up! Take a break.");
          awardBadge("Pomodoro Master");
          return;
        }
        timeLeft--;
        const minutes = Math.floor(timeLeft / 60);
        const seconds = timeLeft % 60;
        document.getElementById("timer").textContent = `${minutes}:${seconds.toString().padStart(2, "0")}`;
      }, 1000);
    }
    function resetTimer() {
      clearInterval(timer);
      timeLeft = 25 * 60;
      document.getElementById("timer").textContent = "25:00";
    }

    // Task Prioritizer
    function addTask() {
      const task = document.getElementById("taskInput").value;
      const priority = document.getElementById("priority").value;
      if (!task) return;
      const li = document.createElement("li");
      li.className = "list-group-item d-flex justify-content-between align-items-center";
      li.innerHTML = `${task} <span class="badge bg-${getColor(priority)}">${priority}</span>`;
      document.getElementById("taskList").appendChild(li);
      document.getElementById("taskInput").value = "";
      awardBadge("Task Organizer");
    }
    function getColor(priority) {
      return { A: "danger", B: "warning", C: "info", D: "secondary" }[priority];
    }

    // Badges
    function awardBadge(name) {
      const badge = document.createElement("span");
      badge.className = "badge bg-success badge-custom";
      badge.textContent = name;
      document.getElementById("badges").appendChild(badge);
    }
  </script>
</body>
</html>
