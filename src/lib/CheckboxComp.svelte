<script>
    import { Checkbox } from 'flowbite-svelte';
</script>
  
<div class="grid grid-cols-2 border border-black w-1/3 p-5">
    <!-- col 1 : Select Subject-->
    <div class="flex flex-col items-center">
        <h1 class="mb-2 font-bold">Select Subject(s):</h1>
        <Checkbox checked>Subject #1</Checkbox>
        <Checkbox>Subject #2</Checkbox>
        <Checkbox>Subject #3</Checkbox>
        <Checkbox>Subject #4</Checkbox>
        <Checkbox>Subject #5</Checkbox>
    </div>
    
    <!-- col 2 : Select Tutor -->
    <div class="flex flex-col items-center">
        <h1 class="mb-2 font-bold">Select Tutor(s):</h1>
        <Checkbox>Tutor #1</Checkbox>
        <Checkbox>Tutor #2</Checkbox>
        <Checkbox checked>Tutor #3</Checkbox>
        <Checkbox>Tutor #4</Checkbox>
        <Checkbox>Tutor #5</Checkbox>
        <Checkbox >Tutor #6</Checkbox>
        <Checkbox>Tutor #7</Checkbox>
        <Checkbox checked>Tutor #8</Checkbox>
    </div>

</div>