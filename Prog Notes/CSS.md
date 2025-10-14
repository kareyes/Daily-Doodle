

dynamic class

```
.trigger {
	display: inline-block
	transition: rotate 0.2s ease;
	
	&.open {
		rorate: -90deg
	}

}


or use data variable

&[data-status='open']{
	rotate: -90deg
}

```