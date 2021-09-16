
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


	// [ METHODS ]
	//@ get > message entered in Input
	const getMessage = ( e ) => {
		msg_txt = e.detail;
	} 
	//@ analyse > message
	const analyseMessage = () => {
		
		if( !is_analysing ) {

			// start > analysing message
			is_analysing = true;

			// create > new message
			let msg = {
				id: ($MessageStore[$MessageStore.length - 1]?.id + 1) || 0,
				message: msg_txt,
				emotion: {
					assessment: 'positive',
					positive: 0,
					neutral:  0,
					negative: 0,
				},
				date: getTime(),
				is_active: true,
			};

			// end > analysing message
			is_analysing = false;
			addMessage(msg);

		} else {
			return;
		}

	}
	//@ add > message to Store
	const addMessage = ( msg ) => {

		MessageStore.update(curMessages => {
			return [...curMessages, msg];
		});

		setActiveMessage( $MessageStore , msg.id );

	}
	//@ set > current active message
	const setActiveMessage = ( messages , id ) => {
		for(let msg of messages) {
			if( msg.id != id ) {
				msg.is_active = false;
			}
		}
	}
	//@ get > current time (hh:mm)
	const getTime = () => {
		let d = new Date();
		let h = d.getHours() < 10 ? ('0' + d.getHours()) : d.getHours();
		let m = d.getMinutes() < 10 ? ('0' + d.getMinutes()) : d.getMinutes();

		return `${h}:${m}`;
	}


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
