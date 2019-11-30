<script>
  import Book from "./Book.svelte";

  let title = "";
  let price = "";
  let description = "";

  let books = [];

  function sellBook(event) {
    const newBook = event.detail;
    console.log(newBook);

    books = [...books, newBook];
    console.log(books);

    title = "";
    price = "";
    description = "";
  }
</script>

<style>
  section {
    width: 30rem;
    margin: auto;
  }
  input {
    width: 100%;
  }
</style>

<section>
  <h1>Sell your book!</h1>
  <input type="text" id="title" placeholder="Title" bind:value={title} />
  <input type="number" id="price" placeholder="Price" bind:value={price} />
  <input
    type="text"
    id="title"
    placeholder="Description"
    bind:value={description} />
  <Book
    bookTitle={title}
    bookPrice={price}
    bookDescription={description}
    buttonText="Sell"
    on:Dispatch={sellBook} />
</section>

<hr />

<section>
  <h1>Books in sale!</h1>

  {#if books.length === 0}
    <p>No books</p>
  {:else}
    {#each books as book}
      <Book
        bookTitle={book.bookTitle}
        bookPrice={book.bookPrice}
        bookDescription={book.bookDescription}
        buttonText="Buy"
        on:Dispatch={() => {}} />
    {/each}
  {/if}
</section>

<!-- TEHTÄVÄ ALLE -->
<hr />

<section>
  <h1>Buyed books!</h1>
</section>
