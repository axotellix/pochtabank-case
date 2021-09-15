
<!-- [ SCRIPT ] -->
<script>
	// [ IMPORTS: components ]
    
	
	// [ IMPORTS: props ]
	export let placeholder = 'введите текст ...';


    // [ PROPS ]
    let value = '';


    // [ EVENTS ]
    $: togglePlaceholder( value );


    // [ METHODS ]
    const togglePlaceholder = async ( value ) => {
        if( value ) {
            document.querySelector('p.placeholder').style.display = 'none';
        } else {
            document.querySelector('p.placeholder').style.display = 'block';
        }
    }

</script>


<!-- [ TEMPLATE: Input > Chat ] -->
<div class = 'input'>
    <div contenteditable = 'true' bind:innerHTML={ value }></div>
    <p class="placeholder">{ placeholder }</p>
</div>
