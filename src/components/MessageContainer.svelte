
<!-- [ SCRIPT ] -->
<script>
	// [ IMPORTS: extensions ]
    import MessageStore from "../stores/MessageStore.js";


	// [ IMPORTS: components ]
    import Message from "./Message.svelte";
	
	// [ IMPORTS: props ]


	// [ PROPS ]
	let messages = [];


    // [ STORES ]
	const unsub = MessageStore.subscribe(data => {
		messages = data;
	});


</script>


<!-- [ TEMPLATE: MessageContainer > Chat ] -->
<div class = 'section-messages'>

	<Message />

</div>
