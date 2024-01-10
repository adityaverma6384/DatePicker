<script>
    export let value;
    export let label = 'Date';
    let timer; // Timer to check date validity
    let countdown = null; // Countdown for displaying time taken
    let countdownDuration = 10; // Countdown duration in seconds
    let startTime = null; // Start time of the stopwatch
    let endTime = null; // End time of the stopwatch
    let timerRunning = false; // Flag to track if the timer is running

    function handleChange(event) {
        const selectedDate = event.target.value;
        value = selectedDate;

        // Start the stopwatch when the user begins filling the input
        if (!startTime && selectedDate !== '') {
            startTime = new Date();
            updateCountdown(); // Start updating the time taken
        }

        // Check if the timer is already running
        if (timer) {
            clearTimeout(timer); // Clear the existing timer
        }

        // Start a new timer that checks if a valid date is filled after a delay
        if (timerRunning) {
            timer = setTimeout(checkDateValidity, countdownDuration * 1000);
        }
    }

    function checkDateValidity() {
        if (value && !isNaN(Date.parse(value))) {
            endTime = new Date();
            console.log('Valid date filled:', value);
            calculateTimeTaken();
            // You can perform additional actions when a valid date is filled here
        } else {
            console.log('Date is not yet filled or not valid:', value);
        }
    }

    function calculateTimeTaken() {
        if (startTime && endTime) {
            const timeDifference = (endTime - startTime) / 1000; // Calculate time in seconds
            countdown = timeDifference.toFixed(2); // Format to two decimal places
        }
    }

    function updateCountdown() {
        if (startTime && timerRunning) {
            const currentTime = new Date();
            const timeDifference = (currentTime - startTime) / 1000; // Calculate time in seconds
            countdown = timeDifference.toFixed(2); // Format to two decimal places
            setTimeout(updateCountdown, 10); // Update the countdown every 10 milliseconds
        }
    }

    function toggleTimer() {
        timerRunning = !timerRunning; // Toggle the timer state
        if (timerRunning) {
            startTime = new Date(); // Start the timer
            updateCountdown();
            handleChange({ target: { value } });
        } else {
            clearTimeout(timer); // Stop the timer
            calculateTimeTaken();
        }
    }
</script>

<style>
    label {
        display: block;
        margin-bottom: 0.5rem;
    }

    input {
        width: 100%;
        padding: 0.5rem;
        font-size: 1rem;
        box-sizing: border-box;
        margin: 30px auto;
    }

    .selected-date {
        margin-top: 1rem;
        color: #333;
    }
</style>

<input id="dateInput" type="date" bind:value on:input={handleChange} />

<!-- Display the selected date and time taken -->
<p class="selected-date">
    Selected Date: {value || '- - -'}<br />
    Time Taken (seconds): {countdown !== null ? countdown : 'Not calculated yet'}
</p>

<!-- Button to start/stop the timer -->
<button on:click={toggleTimer}>
    {timerRunning ? 'Stop Timer' : 'Start Timer'}
</button>
