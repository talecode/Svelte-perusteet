<script>
  export let bookTitle;
  export let bookPrice;
  export let bookDescription;
  export let buttonText;

  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  function submitBook() {
    dispatch("Dispatch", {
      bookTitle,
      bookPrice,
      bookDescription
    });
  }
</script>

<style>
  div {
    margin: 1rem;
    box-shadow: 0 2px 8px black;
    padding: 1rem;
    border-radius: 5px;
    background: white;
  }

  button {
    font: inherit;
    padding: 0.15rem 0.5rem;
    background: purple;
    border: 1px solid purple;
    color: white;
    cursor: pointer;
  }

  button:hover,
  button:active {
    box-shadow: 1px 1px 6px black;
  }
</style>

{#if bookTitle || bookPrice || bookDescription}

  <div>
    <h1>{bookTitle}</h1>
    <h2>{bookPrice}</h2>
    <p>{bookDescription}</p>
    {#if bookTitle && bookPrice && bookDescription}
      <button on:click={submitBook}>{buttonText}</button>
    {/if}
  </div>
{/if}
