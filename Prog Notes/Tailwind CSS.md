
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


