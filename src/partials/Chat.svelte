
<!-- [ SCRIPT ] -->
<script>
	// [ IMPORTS: extensions ]
	import MessageStore from "../stores/MessageStore.js";

	// [ IMPORTS: components ]
	import Header from "./../components/Header.svelte";
	import MessageContainer from "./../components/MessageContainer.svelte";
	import Toolbar from "./../components/Toolbar.svelte";
	import Input from "./../components/Input.svelte";
	import Ico from "./../components/Ico.svelte";
	
	// [ IMPORTS: props ]


	// [ PROPS ]
	let is_analysing = false;
	let msg_txt  = '';
	let msg = {
		id: 0,
		message: 'message 0',
		emotion: {
			assessment: 'positive',
			positive: 0,
			neutral:  0,
			negative: 0,
		},
		date: '00:00'
	};


	// [ METHODS ]
	//@ get > message entered in Input
	const getMessage = ( e ) => {
		msg_txt = e.detail;
	} 
	//@ analyse > message
	const analyseMessage = async () => {
		
		// start > analysing message
		if( !is_analysing ) {
			// set > analysing state
			is_analysing = true;

			// create > new message
			msg.id 		= ($MessageStore[$MessageStore.length - 1]?.id + 1) || 0;
			msg.message = msg_txt;
			msg.date 	= getTime();

		} else {
			return;
		}

		// end > analysing message
		is_analysing = false;
		addMessage();

	}
	//@ add > message to Store
	const addMessage = () => {

		MessageStore.update(curMessages => {
			console.group('curMessages:');
			console.log([...curMessages]);
			console.groupEnd();

			console.group('msg:');
			console.log(msg);
			console.groupEnd();

			return [...curMessages, msg];
		});

	}
	//@ get > current time (hh:mm)
	const getTime = () => {
		let d = new Date();
		let h = d.getHours() < 10 ? ('0' + d.getHours()) : d.getHours();
		let m = d.getMinutes() < 10 ? ('0' + d.getMinutes()) : d.getMinutes();

		return `${h}:${m}`;
	}


	// [ LIFECYCLE HOOKS ]
	// onMount(() => {
	// 	const unsub = MessageStore.subscribe(data => {
	// 		messages = data;
	// 	});
	// })
	// onDestroy(() => {
	// 	unsub();
	// })


</script>


<!-- [ TEMPLATE: Chat > Root ] -->
<div class = 'chat'>

	<!-- [ header ] -->
	<Header title="Введите сообщение" />

	<!-- [ messages ] -->
	<MessageContainer messages={ $MessageStore } /> 

	<!-- [ toolbar: send message ] -->
	<Toolbar>
		<form class = 'wide single-line'>
			<!-- input: message -->
			<Input on:TransmitMessage={ getMessage } placeholder="Введите сообщение ..." />
			<!-- button: send -->
			<button on:click|preventDefault={ analyseMessage } class="send">
				<Ico type="send" />
			</button>
		</form>
	</Toolbar>

</div>
