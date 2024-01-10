<script>
  import Router from 'svelte-spa-router';
  import DateInput from './DateInput.svelte';
  import DatePicker from './DatePicker.svelte';

  let selectedDate;

  // Define your routes as a JavaScript object
  const routes = {
    '/variant-a': DateInput,
    '/variant-b': DatePicker
  };
</script>

<main>
  <h1>Date Picker</h1>
  <nav>
    <a href="#/variant-a">Variant A</a> | 
    <a href="#/variant-b">Variant B</a>
  </nav>

  <Router {routes}>
    <DateInput slot="component1" bind:value={selectedDate} />
    <DatePicker slot="component2" bind:date={selectedDate} />
  </Router>

  {#if selectedDate}
    <p>Selected Date: {selectedDate}</p>
  {/if}
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
  