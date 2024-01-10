<script>
    export let value;
    export let label = 'Date';

    function handleChange(event) {
        const selectedDate = event.target.value;
        value = selectedDate;
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

<!-- Always display the date field, but it will be blank if no date is selected -->
<p class="selected-date">Selected Date: {value || '- - -'}</p>
