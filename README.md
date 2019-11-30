# Svelte

## [1 Svelte](#1)

> ### [1.1 Perusteet](#11)
>
> ### [1.2 Svelten piirteitä](#12)
>
> ### [1.3 Svelte app](#13)
>
> ### [1.4 Tehtävät](#14)

<a id='1'></a>

## [1 Svelte](#1)

<a id='11'></a>
### [1.1 Perusteet](#svelte)

Svelte on uudenlainen "framework" joka toimii oikeastaan vain kääntäjänä(compiler). Sveltellä rakennettu sovellus kootaan etukäteen(is compiled beforehand), hyvin optimoiduksi standalone JavaScriptiksi. Tämän ansiosta selaimen suorittama työ saadaan paljon pienemmäksi, kun verrataan perinteisillä frameworkeillä toteutettuihin web-sovelluksiin, joissa selain suorittaa myös frameworkin omaa koodia. 

Monet perinteiset frameworkit käyttää virtual-DOM tekniikka, mutta Svelte muokkaa DOM:ia suoraan, kun sovelluksen state muuttuu. Kuten Svelten sivustolla on kuvattu, DOM muutos tehdään [kirurgisesti](https://svelte.dev/).

Optimoidut JavaScript tiedostot ovat kooltaan paljon pienempiä verrattuna muiden frameworkkejen buildaamiin tiedostoihin. Esimerkiksi [tämä](http://svelte-todomvc.surge.sh/) pieni Svelte todo appi on vain 3.6kb zipattuna, kun vertailun vuoksi React + ReactDOM ilman mitään koodia on 45kb zipattuna. [Lähde](https://svelte.dev/blog/frameworks-without-the-framework)

-[Vertailua bundlejen koosta](https://pianomanfrazier.com/post/comparing-svelte-stencil/)

Svelte v3.0.0 julkaistiin 21.4.2019 ja tuolloin Svelte alkoi vasta kerätä huomiota.

-[Svelten github tähdet](https://bestofjs.org/projects/svelte)

-[Framework vertailu](https://2018.stateofjs.com/front-end-frameworks/other-libraries/)

Sveltessä komponentit kirjoitetaan käyttäen HTLM, CSS ja Javasctiptiä.

Svelte itsessään on tarkoitettu hyvin pienien web-sovellusten tekoon. Laajempien web-sovellusten kehitys Svelteä käyttäen onnistuu [Sapper](https://sapper.svelte.dev/) frameworkin avulla.

<a id='12'></a>
### [1.2 Svelten piirteitä](#svelte)

#### Komponentit

Sveltessä komponentit ovat .svelte päättyisiä tiedostoja. Ne koostuu ```<script></script>```, ```<style></style>``` ja HTML tageista.

#### Binding

Luokasta templaattiin tehdään ```<h1>{title}</h1>``` syntaksilla ja templaatista luokkaan ```<input bind:value={title} />```

#### Logiikka

Sveltessä on kolme erilaista logiikkaa joita käytetään HTML joukossa.
```
{#if user.loggedIn}
	<button on:click={toggle}>
		Log out
	</button>
{:else}
	<button on:click={toggle}>
		Log in
	</button>
{/if}

{#each cats as { name }, i}
	<li><a>{i + 1}: {name}</a></li>
{/each}

{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}
```

#### Events

Eventtejä kutsutaan Sveltessä ```<button on:click={suoritettava funktio}></button>```. Myös komponentit voivat lähettää eventtejä. Tämä toteutetaan [EventDispatcherin](https://svelte.dev/tutorial/component-events) avulla.

<a id='13'></a>
### [1.3 Svelte app](#svelte)

Tehdään hyvin yksinkertainen kirjakauppa-appi, josta tulee ilmi Svelten yleisempiä piirteitä. Koska Sveltessä itsessään ei ole routing toimintoja niin tehdään appi yhdelle sivulle jakamalla se ```<hr/>``` tageilla. Svelteen löytyy monta hyvää kolmannen osapuolen routing kirjastoa esim [svelte-routing](https://github.com/EmilTholin/svelte-routing), mutta virallisesti kuuluisi käyttää Sapperin routeria.

Lataa VS Codeen Svelte extension

```
npx degit sveltejs/template book-app
cd book-app
npm install
npm run dev
```

Korvataan boilerplate koodit App.svelte:stä ja lisätään yksi uusi komponentti - Book.svelte
```
<script>
  import Book from "./Book.svelte";

  let title = "";
  let price = "";
  let description = "";

  let books = [];
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
  <input type="text" id="title" placeholder="Title" value={title} />
  <input type="number" id="price" placeholder="Price" value={price} />
  <input type="text" id="title" placeholder="Description" value={description} />
  <Book
    bookTitle={title}
    bookPrice={price}
    bookDescription={description}
    buttonText="Sell" />
</section>

<hr />

<section>
  <h1>Books in sale!</h1>
</section>

```

./scr juureen.
```
<script>
  export let bookTitle;
  export let bookPrice;
  export let bookDescription;
  export let buttonText;
  
  function submitBook() {}

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

<div>
  <h1>{bookTitle}</h1>
  <h2>{bookPrice}</h2>
  <p>{bookDescription}</p>
  <button on:click={submitBook}>{buttonText}</button>
</div>
```

Lisätään Svelten bind:value={} ja tehdään bindauksesta kaksisuuntainen!
```
<!-- App.svelte -->

<!-- input tageihin bind: -->
<input type="text" id="title" placeholder="Title" bind:value={title} />
```

Lisätään napille toiminnallisuutta. Nyt kun pidetään mielessä se, että Book komponentti on moni käyttöinen, niin napin suorittama funktio halutaan tehdä siinä komponentissä mihin se on importattu, eli tässä tapauksessa App.svelte:ssä.

Sveltessä tämä voidaan tehdä esimerkiki EventDispatcher avulla.
```
<!-- Book.svelte -->

import { createEventDispatcher } from "svelte";

const dispatch = createEventDispatcher();

function submitBook() {
  dispatch("Dispatch", {
    bookTitle,
    bookPrice,
    bookDescription
  })
}

<!-- App.svelte -->

  function sellBook(event) {
    const newBook = event.detail;
    console.log(newBook);

    books = [...books, newBook];
    console.log(books);

    title = "";
    price = "";
    description = "";
  }

  <Book
    bookTitle={title}
    bookPrice={price}
    bookDescription={description}
    buttonText="Sell"
    on:Dispatch={sellBook} />
```

Laitetaan sitten ostetut kirjat näkyville käyttäen Svelten each toimintoa
```
<!-- App.svelte -->
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
  
```

Käyttäjä ystävällistä toiminnallisuutta Book komponenttiin
```
<!-- Book.svelte -->

{#if bookTitle || bookPrice || bookDescription}
	<!-- DIV -->
{/if}

{#if bookTitle && bookPrice && bookDescription}
	<!-- BUTTON -->
{/if}
```

Lopuksi lisätään lähdöt tehtävälle...
```
<!-- App.svelte -->

<!-- TEHTÄVÄ -->
<hr />

<section>
   <h1>Buyed books!</h1>
</section>
<!-- TEHTÄVÄ -->
```

<a id='14'></a>
### [1.4 Tehtävät](#svelte)

#### Tehtävä
Tee tehtava.gif mukainen toiminnallisuus ja näkymä annettuihin harjoitus lähtöihin, jotka on kommentoitu koodi esimerkkiin. Jos et ollut tunnilla niin lataa esimerkki ``` git clone https://github.com/talecode/Svelte.git ```.

Vinkki - [$: total](https://svelte.dev/examples#reactive-declarations) on muuttuja, joka muuttuu automaattisesti, kun jokin siihen liityvä toinen muuttuja muuttuu. Sillä on tarkoitus laskea kokonaissumma ostetuista kirjoista. 

Palauta tehtävän koodit zipattuna (kaikki muu paitsi node_modules).
