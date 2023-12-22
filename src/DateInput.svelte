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
    }
  </style>
  
  <label for="dateInput">{label}</label>
  <input id="dateInput" type="date" bind:value on:input={handleChange} />
  