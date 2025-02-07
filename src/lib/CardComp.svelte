<script lang="ts">
    import { Card } from 'flowbite-svelte';
    import type { Snippet } from 'svelte';

    let props:{name:string, imgUrl:string, imgAlt:string, children:Snippet} = $props();

</script>
  
<Card class="m-5 p-5 shadow-lg shadow-[var(--lu-gold)]" >

    <!-- Image Container with aspect ratio -->
    <div class="w-full h-full mb-10">
        <img src={props.imgUrl} alt={props.imgAlt} class="w-full h-full object-cover rounded-lg" />
    </div>

    <!-- Tutor name -->
    <h5 class="mb-2 text-2xl font-bold tracking-tight gold-color dark:text-white">{props.name}</h5>

    <!-- Tutor info -->
    <p>{@render props.children()}</p>

</Card>

<style>
    h5{
        text-transform: uppercase;
        color: var(--alt-lu-gold);
    }
</style>