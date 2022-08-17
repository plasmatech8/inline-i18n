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

We want to validate when all translations are present. If a translation for a block of test is not implemented, let the developer know.

However, if there are multiple translation elements located within the same parent element, we cannot tell if all required text translations are implemented. e.g.
```html
<div>
    <p class="en">Hello</p>
    <p class="de">Hallo</p>
    <p class="fr">Bonjour</p>

    <p class="en">World!</p>
    <p class="de">Welt!</p>
</div>
```
(above, we cannot tell if the french tranlsation for "world" is implemented)

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

### 3. Modularisation of text content

Using a centralised configuration file can make it easier to manage translations because:
* All translations are co-located in a single file
* The same text can be used multiple times in separate components
* It keeps our code smaller, and avoids having to write many lines of code which bloats the codebase

The downside of this is that there is no context for each translation (and we may need to write comments to 
describe how a translation is used). It is also a more annoying process to add translations in a seperate file.

I do not know if the benefits of inline-i18n outweighs the drawbacks.

A centralised configuration file is definitely a more standardised approach.
