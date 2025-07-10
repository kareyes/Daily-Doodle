
using arbitrary values when accessing child element from parent class


### [Using arbitrary variants](https://tailwindcss.com/docs/hover-focus-and-other-states#using-arbitrary-variants)


```html
<ul role="list">
{#each items as item}
<li class="[&.is-dragging]:cursor-grabbing">{item}</li>
{/each}

</ul>
```


```html
<div class="[&_p]:mt-4">
	<p>Lorem ipsum...</p>
	<ul>
		<li>
			<p>Lorem ipsum...</p>
		</li>
<!-- ... -->
	</ul>
</div>
```


# Issues 

## Reference library ( $lib ) conflict

1. Setup new path aliases (don't use $lib)  in `svelte.config.js`

```
const config = {
 // ... other config
 kit: {
  // ... other config
  alias: {
   "@repo": "./path/to/lib/*",
  },
 },
};
```

NOTE: for existing project you need to manually update all the files that are using the $lib to @repo including `component.json`

2. For new project run the CLI, You will be asked a few questions to configure `components.json`

```
Which base color would you like to use? › Slate
Where is your global CSS file? (this file will be overwritten) › src/app.css
Configure the import alias for lib: › @repo
Configure the import alias for components: › @repo/components
Configure the import alias for utils: › @repo/utils
Configure the import alias for hooks: › $@repo/hooks
Configure the import alias for ui: › @repo/components/ui
```


3. In app project, add reference library as new path aliases in `svelte.config.js`

```
const config = {
    preprocess: vitePreprocess(),
    kit: {
        adapter: adapter(),
        alias: {
            "@repo": "../../<package>/<ui-package>/src/lib/*",
        },
    },
};
export default config;
```

## CSS not displaying in app package

1. add new config file `tailwind.config.js`,

```

/** @type {import('tailwindcss').Config} */
const config = {
  darkMode: ["class"],
content: [
    "./src/**/*.{html,js,svelte,ts}",
    "../../<packages>/<ui-package>/src/**/*.{html,js,svelte,ts}"],
};

export default config
```

2. in app.css, copy all the content of app.css of your ui library.
3. add @config inside the app.css

```
@config "../tailwind.config.js";
```


