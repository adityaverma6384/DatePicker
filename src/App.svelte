<script>
	
	import DateInput from './DateInput.svelte';
	import DatePicker from './DatePicker.svelte';
  import Appointment from './Appointment.svelte';

    let selectedDate;
	
  </script>
  
  <main>
	
	<h1>Date Picker</h1>
  <h2>A:</h2>
  <DateInput bind:value={selectedDate} label="Enter your date of birth" />
  <p>Selected Date: {selectedDate}</p>
  <br> <br> <br>

  <h2>B:</h2>
  <DatePicker />



  </main>
  
  <style>
	main {
	  text-align: center;
	  padding: 20px;

	  max-width: 400px;
    margin: 2rem auto;
    font-family: 'Arial', sans-serif;
	}
  </style>
  