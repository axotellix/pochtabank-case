
<!-- [ SCRIPT ] -->
<script>
	// [ IMPORTS: extensions ]
	import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
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
	let msg_txt  	 = '';
	let clearInput   = false;


	// [ EVENTS ]
	$: dispatch('TransmitAnalysingState', is_analysing);


	// [ METHODS ]
	//@ get > message entered in Input
	const getMessage = ( e ) => {
		msg_txt = e.detail;
	} 
	//@ analyse > message
	const analyseMessage = async ( m ) => { 

		// clear > Input
		clearInput = true; 

		// check > if analysing recently sent message (or the chosen one)
		let analysing_new = (typeof(m) === 'object') ? true : false;
		
		//? if > server currently available (no message being analysed)
		if( !is_analysing ) {

			// [ start > analysing message ]
			is_analysing = true;

			 // keep > emotion codes
			const emotions = {
				pos: 'positive',
				neg: 'negative',
			}

			 // prepare > text to analyse
			const text = analysing_new ? msg_txt : m;
			
			 // send > request
			let req = await fetch(`http://reworr.pythonanywhere.com/api/${text}`);
			let res = await req.json();

			 // process > results
			const pos = emotions[res.tonal] == 'positive' ? Math.floor((res.score * 100)) : 0;
			const neg = emotions[res.tonal] == 'negative' ? Math.floor((res.score * 100)) : 0;
			const neu = 100 - pos - neg;
			const results = {
				assessment: emotions[res.tonal],
				positive: pos,
				neutral:  neu,
				negative: neg,
			}

			 // create > new message
			let msg = {
				id: ($MessageStore[$MessageStore.length - 1]?.id + 1) || 0,
				message: text,
				emotion: {
					assessment: results.assessment,
					positive: 	results.positive,
					neutral:  	results.neutral,
					negative: 	results.negative,
				},
				date: getTime(),
				is_active: true,
			};


			// [ end > analysing message ]
			is_analysing = false;
			 // send > analysing result (to render analytics in Dashboard) 
			dispatch('TransmitAnalytics', msg);
			 // render > new message
			if( analysing_new ) {
				addMessage(msg);
			}

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
	const setMsg = ( e ) => {
		let message = '';
		const id = e.detail;

		for(let msg of $MessageStore) {
			if( msg.id != id ) {
				msg.is_active = false;
			} else {
				message = msg.message;
				msg.is_active = true;
			}
		}

		$MessageStore = $MessageStore;
		analyseMessage( message );
	}
	//@ set > current active message
	const setActiveMessage = ( messages , id ) => {
		for(let msg of messages) {
			if( msg.id != id ) {
				msg.is_active = false;
			}
		}
	}
	//@ reset > clear input property (back to false)
	const resetClearInput = () => {
		clearInput = false;
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
	<MessageContainer 
		on:SetMsg = { setMsg }
		messages = { $MessageStore } 
	/> 

	<!-- [ toolbar: send message ] -->
	<Toolbar>
		<form class = 'wide single-line'>
			<!-- input: message -->
			<Input 
				on:TransmitMessage={ getMessage } 
				on:TransmitClearInput={ resetClearInput } 
				{ clearInput } 
				placeholder="Введите сообщение ..."
			/>
			<!-- button: send -->
			<button on:click|preventDefault={ analyseMessage } class="send">
				<Ico type="send" />
			</button>
		</form>
	</Toolbar>

</div>
