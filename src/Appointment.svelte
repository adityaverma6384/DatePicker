<script>
  import { onMount } from 'svelte';
  import flatpickr from 'flatpickr';
  import 'flatpickr/dist/flatpickr.min.css';

  // Dummy data for available time slots
  const availableTimeSlots = [
    '09:00 AM - 10:00 AM',
    '11:00 AM - 12:00 PM',
    '02:00 PM - 03:00 PM',
    // Add more time slots as needed
  ];

  let selectedDate = null;
  let selectedTimeSlot = null;
  let userName = '';
  let duration = null; // Added reactive variable for duration

  // Function to handle booking submission
  function submitBooking() {
    if (selectedDate && selectedTimeSlot && userName) {
      // Implement logic to save the appointment details (e.g., send to a server)
      duration = calculateDuration(selectedTimeSlot);
      console.log('Appointment booked:', { date: selectedDate, time: selectedTimeSlot, user: userName, duration });
    } else {
      alert('Please fill in all fields before submitting.');
    }
  }

  // Dummy function to simulate fetching available dates (replace with API call)
  function fetchAvailableDates() {
    // Simulate API call delay
    return new Promise(resolve => {
      setTimeout(() => {
        resolve(['2023-12-15', '2023-12-16', '2023-12-17']);
      }, 1000);
    });
  }

  let availableDates = [];

  // Fetch available dates on component mount
  onMount(async () => {
    availableDates = await fetchAvailableDates();

    // Initialize flatpickr for date selection
    flatpickr('.datepicker', {
      disable: availableDates.map(date => new Date(date)),
      onChange: selectedDates => {
        selectedDate = selectedDates[0];
      },
    });
  });

  // Function to calculate duration based on the selected time slot
  function calculateDuration(timeSlot) {
    const [startTime, endTime] = timeSlot.split(' - ');
    const [startHour, startMinute] = startTime.split(':').map(Number);
    const [endHour, endMinute] = endTime.split(':').map(Number);

    const start = startHour * 60 + startMinute;
    const end = endHour * 60 + endMinute;

    return (end - start) * 60; // Convert duration to seconds
  }
</script>

<style>
  /* Add your component styles here */
</style>

<div>
  <h1>Book an Appointment</h1>

  <label>
    Select Date:
    <input type="text" class="datepicker" readonly />
  </label>

  <label>
    Select Time:
    <select bind:value={selectedTimeSlot}>
      {#each availableTimeSlots as timeSlot (timeSlot)}
        <option value={timeSlot}>{timeSlot}</option>
      {/each}
    </select>
  </label>

  <label>
    Your Name:
    <input type="text" bind:value={userName} />
  </label>

  <button on:click={submitBooking}>Book Appointment</button>

  {#if duration !== null}
    <p>Duration: {duration} seconds</p>
  {/if}
</div>
