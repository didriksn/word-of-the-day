<script>
  import { onMount } from 'svelte';

  let quote;

  const fetchRandomQuote = async () => {
    const response = await fetch('./assets/quotes.json');
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
      fetchRandomQuote();
    }

    const interval = setInterval(fetchRandomQuote, 10000);
    return () => {
      clearInterval(interval);
    };
  });
</script>

<main>
  <div class="header">
    <nav>
      <div id="menuToggle">
        <input type="checkbox" />

        <span></span>
        <span></span>
        <span></span>

        <ul id="menu">
          <a href="https://didriksn.github.io/" target="_blank">Home</a>
          <div>ExampleLink1</div>
          <div>ExampleLink2</div>
          <div>ExampleLink3</div>
          <div>ExampleLink4</div>
        </ul>
      </div>
    </nav>


    <h1 class="title">WORD OF THE DAY</h1>
  </div>

  {#if quote}
    <div class="quote">
      <p class="text">{quote.text}</p>
      <p class="author">- {quote.author}</p>
    </div>
  {:else}
    <p>Loading...</p>
  {/if}

  <a class="cred" href="https://github.com/dwyl/quotes" target="_blank">Quotes</a>
</main>