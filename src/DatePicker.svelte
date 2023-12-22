<!-- DatePicker.svelte -->

<script>
  import { writable } from 'svelte/store';

  // Svelte stores for day, month, and year
  const selectedDay = writable('');
  const selectedMonth = writable('');
  const selectedYear = writable('');

  // Function to generate an array of days for the dropdown
  function generateDaysArray() {
    return Array.from({ length: 31 }, (_, i) => (i + 1).toString());
  }

  // Function to generate an array of months for the dropdown
  function generateMonthsArray() {
    return Array.from({ length: 12 }, (_, i) => (i + 1).toString());
  }

  // Function to generate an array of years for the dropdown (adjust as needed)
  function generateYearsArray() {
    const currentYear = new Date().getFullYear();
    return Array.from({ length: 100 }, (_, i) => (currentYear - i).toString());
  }

  // Function to handle dropdown change and update the selected date part
  function handleDropdownChange(event, store) {
    const value = event.target.value;
    store.set(value);
  }
</script>

<style>
  .flash-card {
    display: flex;
    justify-content: space-between;
    max-width: 300px;
    margin: 20px auto;
    border: 2px solid rgb(194, 219, 52);
    padding: 10px;
    border-radius: 10px;
    background-color: rgb(81, 85, 86);
  }

  .flash-card select {
    flex: 1;
    padding: 10px; /* Increased padding for better spacing */
    margin: 0 5px;
    border: 1px solid rgb(219, 110, 52); /* Updated border color to match the flash card */
    border-radius: 5px; /* Slight border-radius for a rounded look */
    background-color: #fff; /* Background color for the dropdown */
    appearance: none; /* Remove default appearance */
    cursor: pointer; /* Show pointer cursor on hover */
  }

  .flash-card select:hover {
    background-color: #f2f2f2; /* Light background color on hover */
  }

  .flash-card select:focus {
    outline: none; /* Remove default focus outline */
    box-shadow: 0 0 5px rgba(52, 152, 219, 0.7); /* Add a subtle box-shadow on focus */
  }
</style>

<div class="flash-card">
  <select bind:value={$selectedDay} on:change={(e) => handleDropdownChange(e, selectedDay)}>
    {#each generateDaysArray() as day (day)}
      <option value={day}>{day}</option>
    {/each}
  </select>
  
  <select bind:value={$selectedMonth} on:change={(e) => handleDropdownChange(e, selectedMonth)}>
    {#each generateMonthsArray() as month (month)}
      <option value={month}>{month}</option>
    {/each}
  </select>
  
  <select bind:value={$selectedYear} on:change={(e) => handleDropdownChange(e, selectedYear)}>
    {#each generateYearsArray() as year (year)}
      <option value={year}>{year}</option>
    {/each}
  </select>
</div>

<p>Selected Date: {$selectedYear} - {$selectedMonth} - {$selectedDay}</p>
