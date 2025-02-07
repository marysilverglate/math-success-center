<script lang="ts">
  import {AccordionItem, Accordion, Listgroup, Button} from 'flowbite-svelte';
  import {EnvelopeSolid} from 'flowbite-svelte-icons';
  import type { Snippet } from 'svelte';
  
  // giving properties
  let props:{children:Snippet, generalArray:string[], specialArray:string[]} = $props();

</script>

<!-- <p>{@render props.children()}</p> -->
<Accordion class="my-5">
  <slot name="about-me"><!-- optional fallback --></slot>
  
  {#if props.generalArray && props.generalArray.length > 0}
  <AccordionItem classInactive="hover:bg-[var(--lu-gold)]" classActive="bg-[var(--lu-gold)]">
    <h2 slot="header" class="mx-auto font-bold text-xl">Tutor Subjects</h2>
    <Listgroup items={props.generalArray} let:item class="w-full divide-gray-200 divide-none border-none">
      <div class="w-full border-none black-color">
              {item}
              <hr class="w-full">
          </div>
      </Listgroup>

      
    </AccordionItem>
  {/if}

  {#if props.specialArray && props.specialArray.length > 0}
    <AccordionItem classInactive="hover:bg-[var(--lu-gold)]" classActive="bg-[var(--lu-gold)]">
      <h2 slot="header" class="mx-auto font-bold text-xl">Specialty Subjects</h2>
      <Listgroup items={props.specialArray} let:item class="w-full divide-gray-200 divide-none border-none">
          <div class="w-full border-none black-color">
              {item}
              <hr class="w-full">
          </div>
      </Listgroup>

      
    </AccordionItem>
  {/if}

  <slot name="availability">
  </slot>

  <a href="tutoring-page">
    <Button class="black-bg hover:bg-[#b5a36a] my-3 hover:text-[var(--lu-black)]" color="dark" pill> <EnvelopeSolid class="w-4 h-4 me-2"/>Tutoring Schedule</Button>
  </a>
</Accordion>
<style>
  h2{
    font-size: large;
    color:#101820;
  }
</style>