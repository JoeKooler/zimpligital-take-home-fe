<script lang="ts">
	export let variant: 'vertical' | 'horizontal' = 'vertical';
	export let gap = '';
</script>

{#if (variant = 'vertical')}
	<div style={`padding-top: ${gap}`} />
{:else}
	<div style={`padding-left: ${gap}`} />
{/if}
