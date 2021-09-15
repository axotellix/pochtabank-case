
<!-- [ SCRIPT ] -->
<script>
	// [ IMPORTS: components ]
	import Header from "./../components/Header.svelte";
	import Toolbar from "./../components/Toolbar.svelte";
	import Input from "./../components/Input.svelte";
	import Ico from "./../components/Ico.svelte";
	
	// [ IMPORTS: props ]
	


</script>


<!-- [ TEMPLATE: Chat > Root ] -->
<div class = 'chat'>

	<!-- [ header ] -->
	<Header title="Введите сообщение" />

	<!-- [ messages ] -->

	<!-- [ toolbar: send message ] -->
	<Toolbar>
		<form class = 'wide single-line'>
			<!-- input: message -->
			<Input placeholder="Введите сообщение ..." />
			<!-- button: send -->
			<button class="send">
				<Ico type="send" />
			</button>
		</form>
	</Toolbar>

</div>
