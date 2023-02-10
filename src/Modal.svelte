<script>
	import { fade, blur, fly, slide, scale } from "svelte/transition";
</script>

<div class="modal-bg" transition:fade>
	<div class="modal" transition:fly={{ y: -100 }}>
		<button>Close</button>
		<!-- <slot><optional fallback</slot> -->
		<slot />
		<!-- <slot name="modal" /> -->
		<!-- Named slot -->
	</div>
</div>

<style>
	.modal-bg {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0, 0, 0, 0.8);
	}
	.modal {
		background: white;
		padding: 20px;
		border-radius: 15px;
	}
</style>
