<script lang="ts">
	import '../app.css';
	import { Modals } from 'svelte-modals'
	import { ToastProvider } from '@skeletonlabs/skeleton-svelte';
	
	let { children } = $props();
</script>

<ToastProvider>
	<div class="flex flex-col gap-2 m-2 justify-center bg-white border-2 rounded-xs p-2 ">
		<div class="flex flex-row items-center gap-2">
			<h1 class="h1">Assistant to the Cultivator</h1>
			
			<span>Inventory</span>
		</div>
		<hr class="hr">
		{@render children()}
	</div>
</ToastProvider>


<Modals>
	<!-- shown when any modal is opened -->
	{#snippet backdrop({ close })}
	  <div class="backdrop" onclick={() => close()}></div>
	{/snippet}
  </Modals>

  <style>
	.backdrop {
	  position: fixed;
	  top: 0;
	  bottom: 0;
	  right: 0;
	  left: 0;
	  background: rgba(0,0,0,0.50)
	}
  </style>
