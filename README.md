# inline-i18n

Allows you to write classes like "en", "de", "fr" to specify which language the element
will show for.

When you call the function `setLanguage('en')`, it will show all elements containing the "en" class,
and hide any elements which contain any other language code in the class name.

e.g.
```svelte
<script>
	import { onMount } from 'svelte';
	import { setLanguage } from '$lib/index';
	onMount(() => setLanguage('en'));
</script>
<div>
    <p class="en">Hello</p> <!-- Visible -->
    <p class="de">Hallo</p> <!-- Hidden -->
    <p class="fr">Bonjour</p> <!-- Hidden -->
</div>
```

The class names are language codes as defined in [index.ts](src/lib/index.ts#L56)

## Drawbacks

### 1. Validation when there are multiple tags for the same language

We want to validate when all translations are present, and check when one is not implemented.

However, this what happens when multiple translation elements are located within the same parent
element? e.g.
```html
<div>
    <p class="en">Hello</p>
    <p class="de">Hallo</p>
    <p class="fr">Bonjour</p>

    <p class="en">World!</p>
    <p class="de">Welt!</p>
</div>
```

In this case, we cannot run validation because there is no clear container.

An alternative approach is to simply use components to simply the process. e.g.
```svelte
<Text>
    <span slot="en">Hello<span>
    <span slot="de">Hallo<span>
    <span slot="fr">Bonjour<span>
</Text>
<Text>
    <span slot="en">world!<span>
    <span slot="de">welt!<span>
    <span slot="fr">le monde!<span>
</Text>
```

This would make it easier to throw warnings/errors when a language is not implemented.

### 2. TODO: remember what I was going to write here...