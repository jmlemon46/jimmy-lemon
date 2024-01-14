<script>
	import { onMount } from 'svelte';
	let book;
	let isLoading = false;
  const apiKey ="AIzaSyC0oLh81mJwILuVeAB3V9dnAWx3fHwrcd8";
	export let bookID;


	onMount(async () => {
		isLoading = true;
		const response = await fetch(
			`https://www.googleapis.com/books/v1/volumes/${bookID}?key=${apiKey}`
		);
		if (response.ok) {
			const data = await response.json();
			book = data; // Assign the entire data object to 'book'
			console.log(book);
		} else {
			console.error('Error fetching data');
		}
		isLoading = false;
	});
</script>

{#if isLoading}
	<p>Loading...</p>
{:else if book}
	<div class="bookhighlight">
		<img
			src={book.volumeInfo.imageLinks?.thumbnail}
			alt={book.volumeInfo.title}
			class="bookthumb"
		/>
		<div class="bookinfo">
			<h2>{book.volumeInfo.title}</h2>
			<p class="booksubtitle">{book.volumeInfo.subtitle}</p>
			<p class="bookby">By: {book.volumeInfo.authors?.join(', ')}</p>
			<a href={book.volumeInfo.infoLink} class="bookbutton" target="_blank">Learn More</a>
		</div>
		<!-- Additional details here -->
	</div>
{:else}
	<p>No book found or error in fetching data. Check console for details.</p>
{/if}

<style>
	.bookhighlight {
		background: #fff;
		padding: 1rem;
		display: grid;
		grid-template-columns: 1fr 4fr;
		gap: 2rem;
		margin-bottom: 2rem;
		border: 2px solid var(--black);
	}

	.bookhighlight * {
		margin-bottom: 0px;
	}

	.bookthumb {
		border: 1px solid var(--black);
	}
	a.bookbutton {
		padding: 0.25rem 0.5rem;
		text-transform: uppercase;
		font-family: var(--primaryFont);
		font-weight: bold;
		background: var(--yellow);
		font-size: 0.9rem;
		margin-top: auto;
		justify-self: flex-end;
		display: inline;
	}

	.bookinfo {
		display: flex;
		flex-direction: column;
		align-items: flex-start;
	}

	.bookby {
		font-size: 1rem;
		font-style: italic;
		margin-top: -5px;
	}

	.booksubtitle {
		margin-top: -5px;
	}
</style>
