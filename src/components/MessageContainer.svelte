
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
<div class="scroll-area">

	{#each messages as message (message.id)}
        <Message 
            message={ message.message } 
            emotion={ message.emotion } 
            date={ message.date }  
            isactive={ message.is_active ? 'active' : '' }
        />
    {/each}

</div>
</div>
