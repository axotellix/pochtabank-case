
<!-- [ SCRIPT ] -->
<script>
	// [ IMPORTS: extensions ]


	// [ IMPORTS: components ]
    import Message from "./Message.svelte";
	
	// [ IMPORTS: props ]


	// [ PROPS ]
	export let messages = [];


</script>


<!-- [ TEMPLATE: MessageContainer > Chat ] -->
<div class = 'section-messages'>


	{#each messages as message}
        <Message 
            message={ message.message } 
            emotion={ message.emotion } 
            date={ message.date }  
        />
        { message.id + ": " + message.message }
    {/each}

</div>
