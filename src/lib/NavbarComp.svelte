<script>
    import { page } from '$app/stores';
    import { Navbar, NavBrand, NavLi, NavUl, NavHamburger, Listgroup, Popover} from 'flowbite-svelte';
    $: activeUrl = $page.url.pathname;
</script>
  
<Navbar class="sm:px-4 py-2.5 fixed w-full z-20 top-0 start-0 border-b mb-6 bg-white">

    <!-- MSC logo -->
    <NavBrand href="/">
        <img src="./MSC-logo-tiny.png" alt="Logo" class="w-16 mx-2">
    </NavBrand>

    <!-- Collapsable menu for smaller devices -->
    <NavHamburger />

    <NavUl {activeUrl} class="black-color" nonActiveClass="hover:text-[var(--alt-lu-gold)]" activeClass="bg-[var(--alt-lu-gold)] md:bg-transparent text-white md:text-[var(--alt-lu-gold)] font-bold">
        <NavLi href="/">Home</NavLi>
        <NavLi href="/about-page">Meet the Tutors</NavLi>
        <NavLi href="/tutoring-page">Tutor Schedule</NavLi>
        <NavLi href="/contact-page">Contact</NavLi>
        <NavLi href="/resource-page" id="dropdown">Resources</NavLi>

        <Popover arrow={false} class="text-sm font-light" title="" triggeredBy="#dropdown">
            <Listgroup defaultClass='border-none black-color' items={['Algebra', 'Calculus', 'Statistics', 'Physics', 'Computer Science', 'Other']} let:item class="w-48">
                {item}
            </Listgroup>
        </Popover>
    </NavUl>
</Navbar>

<style>
</style>
