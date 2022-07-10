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
</script>

<!-- Header -->
<h1 class="text-align: center;">
	Inline-i18n Demo
	<a href="https://github.com/plasmatech8/inline-i18n">
		<img
			src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png"
			alt="Github Link"
			height="40"
		/>
	</a>
</h1>
<p>
	Allows you to write classes like "en", "de", "fr" to specify which language the element will show
	for.
</p>
<p>
	When you call the function setLanguage('en'), it will show all elements containing the "en" class,
	and hide any elements which contain any other language code in the class name.
</p>
<p>
	See <a href="https://github.com/plasmatech8/inline-i18n">plasmatech8/inline-i18a</a> for more details.
</p>
<hr />

<!-- Set language -->
<section>
	<h2>Set Language</h2>
	{#each langs as l}
		<button on:click={() => setLanguage(l)}>setLanguage("{l}")</button>
	{/each}
</section>
<hr />

<!-- Content -->
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
<hr />

<!-- Other Tools -->
<section>
	<details>
		<summary>Other Tools</summary>
		<button on:click={() => (debug = Array.from(getImplementedLanguages()))}>
			getImplementedLanguages()
		</button>
		<button on:click={() => (debug = checkTranslations())}> checkTranslations() </button>
		<pre><code>{JSON.stringify(debug, null, 4)}</code></pre>
	</details>
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

	.flex {
		display: flex;
	}
</style>
