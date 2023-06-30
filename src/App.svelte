<script>
  import { onMount } from 'svelte';

  let quote;

  const fetchRandomQuote = async () => {
    const response = await fetch('./quotes.json/');
    const data = await response.json();
    const randomIndex = Math.floor(Math.random() * data.length);
    quote = data[randomIndex];
    localStorage.setItem('quote', JSON.stringify(quote));
  };

  onMount(() => {
    const storedQuote = localStorage.getItem('quote');
    if (storedQuote) {
      quote = JSON.parse(storedQuote);
    } else {
      fetchRandomQuote(); // Initial fetch
    }

    const interval = setInterval(fetchRandomQuote, 10000); // Fetch a new random quote every 10 seconds (for testing local storage)

    // Clean up the interval when the component is destroyed
    return () => {
      clearInterval(interval);
    };
  });
</script>

<main>
  {#if quote}
    <div class="quote">
      <p class="text">{quote.text}</p>
      <p class="author">- {quote.author}</p>
    </div>
  {:else}
    <p>Loading...</p>
  {/if}
</main>

<style>
  /* Add CSS styles for the main App component */
</style>
