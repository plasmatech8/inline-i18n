<script lang="ts">
	import { onMount } from 'svelte';
	import {
		setLanguage,
		getTranslationLanguages as getImplementedLanguages,
		checkTranslations,
		type Language
	} from '$lib/index';
	const langs: Language[] = ['en', 'fr', 'de'];

	onMount(() => setLanguage('en'));
	onMount(() => console.log(Array.from(getImplementedLanguages())));
	let debug: any;

	function handleGetTranslationLanguages() {
		debug = Array.from(getImplementedLanguages());
		console.log(debug);
	}
	function handleCheckTranslations() {
		debug = checkTranslations();
		console.log(debug);
	}
</script>

<h1>Inline-i18n Demo</h1>

<hr />

<section>
	<h2>Tools</h2>
	<button on:click={() => (debug = Array.from(getImplementedLanguages()))}>
		getImplementedLanguages()
	</button>
	<button on:click={() => (debug = checkTranslations())}> checkTranslations() </button>
	<pre><code>{JSON.stringify(debug, null, 4)}</code></pre>
</section>

<hr />

<section>
	<h2>Set Language</h2>
	{#each langs as l}
		<button on:click={() => setLanguage(l)}>setLanguage("{l}")</button>
	{/each}
</section>

<hr />

<section>
	<h2>Content</h2>
	<h3>Single Language</h3>
	<div class="side-by-side">
		<pre><code
				>{`<div>
    <p class="en">English: Hello world!</p>
    <p class="fr">French: Bonjour le monde!</p>
    <p class="de">German: Hallo Welt!</p>
</div>`}</code
			></pre>
		<div>
			<p class="en">English: Hello world!</p>
			<p class="fr">French: Bonjour le monde!</p>
			<p class="de">German: Hallo Welt!</p>
		</div>
	</div>

	<h3>Multiple Languages</h3>
	<div class="side-by-side">
		<pre><code
				>{`<div>
    <p class="en fr">English and French: Hello world! Bonjour le monde!</p>
    <p class="fr de">French and German: Bonjour le monde! Hallo Welt!</p>
</div>`}</code
			></pre>
		<div>
			<p class="en fr">English and French: Hello world! Bonjour le monde!</p>
			<p class="fr de">French and German: Bonjour le monde! Hallo Welt!</p>
		</div>
	</div>

	<h3>Missing a Language!</h3>
	<div class="side-by-side">
		<pre><code
				>{`<div>
    <p class="en">English: Hello world!</p>
    <p class="fr">French: Bonjour le monde!</p>
</div>`}</code
			></pre>
		<div>
			<p class="en">English: Hello world!</p>
			<p class="fr">French: Bonjour le monde!</p>
		</div>
	</div>
</section>

<style>
	button {
		margin-right: 5px;
	}

	pre {
		background-color: lightblue;
		padding: 10px;
	}

	.side-by-side {
		display: grid;
		grid-template-columns: 50% 50%;
		gap: 10px;
	}
</style>
