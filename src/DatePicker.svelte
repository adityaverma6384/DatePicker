<script>
  import { writable } from 'svelte/store';

  const selectedDay = writable('');
  const selectedMonth = writable('');
  const selectedYear = writable('');

  let timer; // Timer to track time spent

  let startTime = null; // Start time of the timer
  let endTime = null; // End time of the timer
  let timerRunning = false; // Flag to track if the timer is running
  let elapsedTime = 0; // Elapsed time in seconds

  function generateDaysArray() {
    return Array.from({ length: 31 }, (_, i) => (i + 1).toString());
  }

  function generateMonthsArray() {
    return Array.from({ length: 12 }, (_, i) => (i + 1).toString());
  }

  function generateYearsArray() {
    const currentYear = new Date().getFullYear();
    return Array.from({ length: 100 }, (_, i) => (currentYear - i).toString());
  }

  function handleDropdownChange(event, store) {
    store.set(event.target.value);

    // Start or resume the timer when a dropdown changes
    if (!timerRunning) {
      startTime = new Date();
      timerRunning = true;
      updateTimer();
    }
  }

  function updateTimer() {
    if (timerRunning) {
      const currentTime = new Date();
      elapsedTime = Math.floor((currentTime - startTime) / 1000); // Elapsed time in seconds
      timer = setTimeout(updateTimer, 100); // Update every 100 milliseconds
    }
  }

  function toggleTimer() {
    if (timerRunning) {
      clearTimeout(timer); // Stop the timer
      timerRunning = false;
    } else {
      startTime = new Date();
      timerRunning = true;
      updateTimer();
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap');

  .datepicker-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'Montserrat', sans-serif;
    background: linear-gradient(135deg, #6e8efb, #a777e3);
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 6px 15px rgba(0,0,0,0.2);
    max-width: 400px;
    margin: 30px auto;
    color: white;
  }

  .select-container {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin: 10px 0;
  }

  .datepicker-label {
    font-weight: 700;
    margin-left: 10px;
  }

  .select-group {
    display: flex;
    align-items: center;
    flex: 1;
  }

  select {
    flex-grow: 1;
    padding: 10px;
    margin: 0 5px;
    border: 2px solid #fff;
    border-radius: 5px;
    background-color: rgba(255, 255, 255, 0.2);
    color: white;
    appearance: none;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  select:hover {
    background-color: rgba(255, 255, 255, 0.3);
  }

  select:focus {
    outline: none;
    background-color: rgba(255, 255, 255, 0.4);
    box-shadow: 0 0 5px rgba(255, 255, 255, 0.6);
  }

  .selected-date {
    font-size: 1.2em;
    font-weight: bold;
    margin-top: 15px;
  }

  button {
    padding: 10px 20px;
  /*  background-color: #4caf50;  Green background color */
    color: black;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1em;
    transition: background-color 0.3s ease;
  }

  button:hover {
    background-color: #45a049; /* Darker green on hover */
  }


  .stopwatch {
    font-size: 1.2em;
    font-weight: bold;
    margin-top: 15px;
  }
</style>

<div class="datepicker-container">
  <div class="select-container">
    <div class="select-group">
      <div class="datepicker-label">Day</div>
      <select bind:value={$selectedDay} on:change={(e) => handleDropdownChange(e, selectedDay)}>
        {#each generateDaysArray() as day (day)}
          <option value={day}>{day}</option>
        {/each}
      </select>
    </div>

    <div class="select-group">
      <div class="datepicker-label">Month</div>
      <select bind:value={$selectedMonth} on:change={(e) => handleDropdownChange(e, selectedMonth)}>
        {#each generateMonthsArray() as month (month)}
          <option value={month}>{month}</option>
        {/each}
      </select>
    </div>

    <div class="select-group">
      <div class="datepicker-label">Year</div>
      <select bind:value={$selectedYear} on:change={(e) => handleDropdownChange(e, selectedYear)}>
        {#each generateYearsArray() as year (year)}
          <option value={year}>{year}</option>
        {/each}
      </select>
    </div>
  </div>

  <p class="selected-date">Selected Date: {$selectedYear} - {$selectedMonth} - {$selectedDay}</p>

  <p class="stopwatch">Stopwatch: {elapsedTime} seconds</p>

  <button on:click={toggleTimer}>
    {timerRunning ? 'Stop Timer' : 'Start Timer'}
  </button>
</div>
