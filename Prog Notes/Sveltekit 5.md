
automatic reload component upon navigating

``` layout.svelte
<nav data-sveltekit-reload> 
	<a href="/">home</a> <a href="/about">about</a>
</nav>

```

### Rest parameters

src/routes/ 
├ categories/ 
│ ├ animal/ 
│ ├ mineral/ 
│ ├ vegetable/ 
│ ├ [...catchall]/ 
│ │ ├ +error.svelte 
│ │ └ +page.server.js


### Breaking out of layouts

```
rename the page to this
+page@.svelte
```

### Universal load functions [Ref](https://svelte.dev/tutorial/kit/universal-load-functions)

### Using parent data [Ref](https://svelte.dev/tutorial/kit/await-parent)


## Dynamic multiple class

```

let open = $state(false)
<span class={['trigger', {open}]}></span>
```

need to update the effect state before the DOM update
```
 $effect.pre(() =>{
 
 //use tick() to qeue effect
 })
```

* structureClone() [Ref](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone)
	* Transferred objects are detached from the original object and attached to the new object; they are no longer accessible in the original object

*qeueMicrotask/tick

	