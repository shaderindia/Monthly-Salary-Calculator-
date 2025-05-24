<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Income Calculator - Welcome!</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }

    .container {
      max-width: 800px;
      margin: auto;
    }

    input, button, select {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
    }

    .calendar-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .calendar {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 5px;
      margin-top: 10px;
    }

    .day-names {
      font-weight: bold;
      text-align: center;
      background-color: #f2f2f2;
      padding: 10px;
    }

    .day {
      padding: 10px;
      border: 1px solid #ccc;
      text-align: center;
      min-height: 80px;
      position: relative;
    }

    .day.weekend {
      background-color: #fce4ec; /* Light Pink for weekends */
    }

    .day input {
      width: 80%;
      margin-top: 5px;
    }

    .day span {
      font-weight: bold;
    }

    .result {
      font-weight: bold;
      color: green;
    }

    .welcome {
      margin-bottom: 20px;
      font-size: 1.4em;
      font-weight: bold;
      background-color: #f0f8ff; /* Light Blue */
      padding: 15px;
      border-radius: 5px;
      text-align: center;
      color: #333;
    }

    .instagram {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-top: 10px;
      font-size: 1em;
    }

    .instagram-logo {
      width: 20px;
      height: 20px;
      margin-right: 8px;
    }

    footer {
      margin-top: 30px;
      text-align: center;
      font-size: 0.9em;
      color: #666;
    }

    footer .instagram {
      margin-top: 10px;
      display: flex;
      justify-content: center;
      align-items: center;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- Welcome Section -->
    <div class="welcome" id="welcomeMessage">
      Welcome! 🌟 Start your journey to unlocking the true value of your time and effort. 
      Let's make every hour count! 💸
    </div>
    <div class="instagram">
      <img src="https://upload.wikimedia.org/wikipedia/commons/a/a5/Instagram_icon.png" alt="Instagram Logo" class="instagram-logo">
      <span>@nishix_vamp</span>
    </div>

    <h1>Dynamic Monthly Salary Calculator</h1>

    <!-- Name Input Section -->
    <label for="userName">Your Name:</label>
    <input type="text" id="userName" placeholder="Enter your name">
    <button onclick="saveName()">Save Name</button>

    <!-- Input Fields -->
    <label for="baseSalary">Base Salary (RON):</label>
    <input type="number" id="baseSalary" placeholder="Enter your in-hand monthly salary">

    <label for="workingDays">Total Working Days in a Month:</label>
    <input type="number" id="workingDays" placeholder="Enter total working days">

    <!-- Calendar Controls -->
    <div class="calendar-header">
      <div>
        <label for="year">Year:</label>
        <select id="year"></select>
      </div>
      <div>
        <label for="month">Month:</label>
        <select id="month"></select>
      </div>
    </div>

    <!-- Calendar Display -->
    <div id="calendar" class="calendar"></div>

    <!-- Calculate Button -->
    <button onclick="calculateSalary()">Calculate Monthly Salary</button>

    <!-- Result Display -->
    <p class="result" id="result"></p>
  </div>

  <!-- Footer -->
  <footer>
    Made with ❤️ by Nishikant Xalxo<br>
    <div class="instagram">
      <img src="https://upload.wikimedia.org/wikipedia/commons/a/a5/Instagram_icon.png" alt="Instagram Logo" class="instagram-logo">
      <span>@nishix_vamp</span>
    </div>
  </footer>

  <script>
    // Populate year and month dropdowns
    function populateYearAndMonth() {
      const yearSelect = document.getElementById('year');
      const monthSelect = document.getElementById('month');

      const currentYear = new Date().getFullYear();

      // Populate years (e.g., 2020 to 2030)
      for (let year = currentYear - 5; year <= currentYear + 5; year++) {
        const option = document.createElement('option');
        option.value = year;
        option.textContent = year;
        if (year === currentYear) option.selected = true;
        yearSelect.appendChild(option);
      }

      // Populate months
      const months = [
        "January", "February", "March", "April", "May",
        "June", "July", "August", "September", "October", "November", "December"
      ];
      months.forEach((month, index) => {
        const option = document.createElement('option');
        option.value = index;
        option.textContent = month;
        if (index === new Date().getMonth()) option.selected = true;
        monthSelect.appendChild(option);
      });
    }

    // Generate a calendar grid
    function generateCalendar() {
      const calendar = document.getElementById('calendar');
      calendar.innerHTML = ''; // Clear previous calendar

      const year = parseInt(document.getElementById('year').value);
      const month = parseInt(document.getElementById('month').value);

      const firstDay = new Date(year, month, 1).getDay(); // Day of the week (0 = Sunday, 1 = Monday, ...)
      const daysInMonth = new Date(year, month + 1, 0).getDate(); // Total days in the month

      // Adjust first day to match Monday as the start of the week
      const adjustedFirstDay = (firstDay === 0) ? 6 : firstDay - 1;

      // Add day names
      const dayNames = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"];
      dayNames.forEach(day => {
        const dayName = document.createElement('div');
        dayName.classList.add('day-names');
        dayName.textContent = day;
        calendar.appendChild(dayName);
      });

      // Empty slots for days before the first day of the month
      for (let i = 0; i < adjustedFirstDay; i++) {
        const emptyCell = document.createElement('div');
        calendar.appendChild(emptyCell);
      }

      // Generate days of the month
      for (let day = 1; day <= daysInMonth; day++) {
        const date = new Date(year, month, day);
        const isWeekend = date.getDay() === 0 || date.getDay() === 6; // Saturday or Sunday

        const dayCell = document.createElement('div');
        dayCell.classList.add('day');
        if (isWeekend) {
          dayCell.classList.add('weekend');
        }

        dayCell.innerHTML = `
          <span>${day}</span>
          <input type="number" placeholder="Hours" data-day="${day}">
        `;
        calendar.appendChild(dayCell);
      }
    }

    // Calculate dynamic monthly salary
    function calculateSalary() {
      const baseSalary = parseFloat(document.getElementById('baseSalary').value);
      const workingDays = parseInt(document.getElementById('workingDays').value);

      if (!baseSalary || !workingDays) {
        document.getElementById('result').textContent = 'Please fill in all fields correctly.';
        return;
      }

      // Standard hourly rate
      const hourlyRate = baseSalary / (workingDays * 8);

      let totalSalary = 0;

      // Calculate total salary based on daily hours
      document.querySelectorAll('#calendar input').forEach(input => {
        const hoursWorked = parseFloat(input.value) || 0;
        totalSalary += hoursWorked * hourlyRate;
      });

      document.getElementById('result').textContent = `
        Your Total Monthly Salary is: ${totalSalary.toFixed(2)} RON
        (Based on actual hours worked)
      `;
    }

    // Save the user's name in localStorage
    function saveName() {
      const userName = document.getElementById('userName').value.trim();
      if (userName) {
        localStorage.setItem('userName', userName);
        displayWelcomeMessage(userName);
      }
    }

    // Display a welcome message with the user's name
    function displayWelcomeMessage(name) {
      const welcomeMessage = document.getElementById('welcomeMessage');
      welcomeMessage.innerHTML = `
        Welcome, ${name}! 🌟 Start your journey to unlocking the true value of your time and effort. 
        Let's make every hour count! 💸
      `;
    }

    // Load the user's name on page load and display welcome message
    document.addEventListener('DOMContentLoaded', () => {
      populateYearAndMonth();
      generateCalendar();

      const savedName = localStorage.getItem('userName');
      if (savedName) {
        displayWelcomeMessage(savedName);
        document.getElementById('userName').value = savedName;
      }

      // Update calendar when year or month changes
      document.getElementById('year').addEventListener('change', generateCalendar);
      document.getElementById('month').addEventListener('change', generateCalendar);
    });
  </script>
</body>
</html>
