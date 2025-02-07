<script lang="ts">
	import '../app.css';
    import FooterComp from '$lib/FooterComp.svelte';
	let { children } = $props();
</script>

<!-- Page content -->
{@render children()}

<!-- Footer -->
<FooterComp/>