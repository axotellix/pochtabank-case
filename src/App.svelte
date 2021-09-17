
<!-- [ SCRIPT ] -->
<script>
	// [ IMPORTS: components ]
	import Chat from "./partials/Chat.svelte";
	import Dashboard from "./partials/Dashboard.svelte";
	
	// [ IMPORTS: props ]
	let analyticsReceived = false;
	let assessment = 'negative';
	let ratio = {
        positive: 70,
        neutral: 30,
        negative: 0,
    };
	let actions = ['action 1', 'action 2', 'action 3', 'action 4'];


	// [ METHODS ]
	//@ get > message analytics
	const getAnalytics = ( e ) => {

		// store > analytics results;
		const msg = e.detail;

		// store > all actions
		const action_book = {
			positive: ['positive action 1', 'positive action 2'],
			neutral:  ['neutral action 1', 'neutral action 2'],
			negative: ['negative action 1', 'negative action 2'],
		};

		// get > data
		assessment = msg.emotion.assessment;
		ratio.positive = msg.emotion.positive;
		ratio.neutral  = msg.emotion.neutral;
		ratio.negative = msg.emotion.negative;
		actions = action_book[assessment];

		// display > analytics in Dashboard
		analyticsReceived = true;

	}

</script>


<!-- [ TEMPLATE: Root ] -->
<main>

	<!-- [ section: chat ] -->
	<Chat on:TransmitAnalytics={ getAnalytics } />
	
	<!-- [ section: dashboard ] -->
	<Dashboard 
		{ analyticsReceived }
		{ assessment }
		{ ratio }
		{ actions }
	/>

</main>