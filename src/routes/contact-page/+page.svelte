<script>
    import NavbarComp from "$lib/NavbarComp.svelte";
    import { Button, Card } from "flowbite-svelte";
    import { EnvelopeSolid, UsersGroupSolid,  EditOutline } from "flowbite-svelte-icons"
</script>

<svelte:head>
    <title>Math Success Center - Contact</title>
    <meta name="description" content="The Math Success Center, a free tutoring service provided by Lindenwood University. Tutors available for various university math courses, as well as computer science and physics."/>
    <link rel="icon" href={"favicon.png?v=1"} />
</svelte:head>

<NavbarComp />

<!-- md:grid-cols-2 lg:grid-cols-1 -->
<main class="grid grid-cols-1 place-items-center text-center min-h-screen">
    
    <!-- COL #1: Director -->
    <div class="mx-10 mt-10">
        <h3>Meet the Director</h3>
        <Card class="text-center my-8 black-color shadow-xl shadow-lg shadow-[var(--lu-gold)]" size="xl">
        
            <!-- Name -->
            <h2 class="font-bold text-lg">Mary Silverglate</h2>
            <!-- Title -->
            <p class="mb-5">Instructor and Director of the Math Success Center</p>
        
            <!-- Card content container -->
            <div class="grid grid-cols-1 sm:grid-cols-2 ">
                <!-- Col 1: IMG -->
                <div class= "w-full h-96 mb-5 sm:2/3 md:w-5/6 lg:w-5/6 lg:mx-auto">
                    <img src="./about-page-img/Mary.jpg" alt="Insert Img Here" class="w-full h-full object-cover rounded-lg">
                </div>
                <!-- Col 2: DESCRIPTION -->
                <div class="my-auto m-5"> 
                    
                    <h1 class="mt-16">Get in Touch</h1>
                    <a href="mailto:msilverglate@lindenwood.edu?">
                        <Button class=" black-bg hover:bg-[var(--lu-gold)] hover:text-[var(--lu-black)]" color="dark" pill> <EnvelopeSolid class="w-4 h-4 me-2"/>Email</Button>
                    </a>


                    <h1>Leave Feedback</h1>
                    <a href="https://forms.gle/YAzaujeaxCYQvKHUA" target="_blank">
                        <Button class=" mb-5 black-bg hover:bg-[var(--lu-gold)] hover:text-[var(--lu-black)]" color="dark" outline={false}> <EditOutline class="w-4 h-4 me-2"/>To Feedback Form</Button>
                    </a>

                    <div class="mt-10">
                        <!-- SEO to Tutoring page -->
                        <Button class=" text-xs sm:text-sm m-2 w-1/2 sm:1/2 md:w-1/2 md:h-12 lg:w-1/2 lg:h-12 hover:bg-[var(--lu-gold)] hover:text-[var(--lu-black)] black-bg animate-bounce" color="dark" pill href="about-page" > 
                            <UsersGroupSolid class="w-4 h-4 me-2"/> Meet the Tutors!
                        </Button>

                    </div>
    
                </div>
            </div>
        </Card>

    
    </div>
</main>



<style>
    h1{
        font-weight: bold;
        font-size: x-large;
        margin-top: 30px;
        text-transform: uppercase;
    }
    h3{
        font-weight: bold;
        font-size: xx-large;
        margin-top: 30px;
        text-transform: uppercase;
        color: var(--lu-gold);
    }
</style>


