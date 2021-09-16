
<!-- [ SCRIPT ] -->
<script>
	// [ IMPORTS: extensions ]


	// [ IMPORTS: components ]

	
	// [ IMPORTS: props ]


	// [ PROPS ]
	let emotion = 'positive';


</script>


<!-- [ TEMPLATE: Message > MessageContainer ] -->
<div class = 'message'>

	<!-- <slot name="message"></slot>
	<slot name="metadata"></slot> -->
    
    <div class="message-text">
        message text
    </div>
	<div class="metadata">
        <div class="indicator">
            {#if emotion == 'positive'}
                <span class = 'emotion green'>положительное</span>
            {:else if emotion == 'neutral'}
                <span class = 'emotion yellow'>нейтральное</span>
            {:else}
                <span class = 'emotion red'>отрицательное</span>
            {/if}
        </div>
        <div class="date">22:00</div>
    </div>

</div>
