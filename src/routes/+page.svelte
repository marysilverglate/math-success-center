<script>
    import { Button, Popover, Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell} from 'flowbite-svelte';
    import { slide } from 'svelte/transition';
    import { UsersGroupOutline, FolderOutline, AddressBookOutline, SearchOutline,ChevronDoubleDownOutline} from 'flowbite-svelte-icons';
    import NavbarComp from '$lib/NavbarComp.svelte';

</script>

<!-- NAVBAR -->
<NavbarComp/>

<!-- HOME PAGE -->
<svelte:head>
    <title>The Math Success Center - Home</title>
    <meta name="description" content="The Math Success Center, a free tutoring service provided by Lindenwood University. Tutors available for various university math courses, as well as computer science and physics."/>
    <link rel="icon" href={"favicon.png?v=1"} />
</svelte:head>

<!-- HERO SECTION -->
<div class="text-center border-b-2 border-[var(--lu-black)] mt-24">
    <!-- MSC Logo -->
    <img src="./MSC-logo.png" alt="The Math Success Center Logo" class="mx-auto lg:w-2/3 my-5 rounded-xl">
    <!-- Tagline -->
    <p class="italic m-6 text-sm sm:text-lg">Math Made <span class="gold-color">Simple.</span> Success Made <span class="gold-color">Possible</span></p>
    <ChevronDoubleDownOutline class="text-[var(--lu-gold)] size-10 animate animate-bounce mx-auto mt-10 mb-0"/>
</div>





<!-- MAIN CONTENT -->
<main class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 p-0 m-0">

    <!-- COL 1: Home intro para -->
    <div class="bg-gray-100 rounded-3xl sm:my-20 mx-auto my-10 p-5 w-5/6 md:p-10 lg:w-2/3 lg:p-16 md:col-span-1 lg:col-span-3 shadow-lg shadow-[var(--lu-gold)] h-fit ">
        <h1 class="text-center font-bold text-2xl md:text-4xl">Welcome to the Math Success Center!</h1>
        <p class="my-5 leading-10 indent-10 text-justify md:w-full black-color"> 
            We are a free tutoring service provided by Lindenwood University. Students seeking assistance in mathematics, physics, and computer science are encouraged to take advantage of this opportunity. Previously located in Young Hall, the Math Success Center moved to the Library and Academic Resources Center (LARC), Room 121, in August 2023 under the leadership of its new director, Mary Silverglate.
        </p>
    </div>



    <!-- COL 2 : Location and hours --> 
    <div class="lg:col-span-3 lg:w-3/4 lg:mx-auto md:mx-0 md:col-span-1 m-5 col-span-full text-center mx-auto w-5/6 justify-center">

        <div class="grid grid-cols-1 w-5/6 md:grid-cols-1 md:w-full lg:grid-cols-1 lg:w-full p-4 h-full mx-auto">
                
            <div class="grid grid-cols-1 md:grid-cols-1 lg:grid-cols-2">
                <!-- Embedded map -->
                <div class= "w-full h-9/10 text-center my-auto bg-gray-50 rounded-3xl shadow-md">
                    <h1>Location</h1>   
                    <center><iframe title="Embedded map of the LARC" src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3110.082400328129!2d-90.5025292240686!3d38.78474532174831!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x87df2f5848ef42f1%3A0x820c071c67652f4a!2sLibrary%20and%20Academic%20Resources%20Center%20(LARC)!5e0!3m2!1sen!2sus!4v1732214915053!5m2!1sen!2sus" width="95%" height="250" style="border:0;" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe></center>
                    <p class="m-3 text-sm md:text-lg text-[var(--lu-black)]">Room number: <span class="text-[var(--alt-lu-gold)]">LARC 121</span></p>
                </div>
    

                <!-- Hours -->
                <div class="lg:w-5/6 px-5 pt-0 pb-5 md:mt-3 mt-16 sm:mx-auto lg:text-xl rounded-3xl bg-gray-50 w-full h-9/10 text-center my-auto">
                    <h1>Operating Hours</h1>
                    <Table shadow hoverable={true} striped={true}>
            
                        <TableHead theadClass="black-bg white-color text-center md:text-lg">
                            <TableHeadCell> Day</TableHeadCell>
                            <TableHeadCell> Time</TableHeadCell>
                        </TableHead>
            
                        <TableBody tableBodyClass="divide-y text-center md:text-md">
            
                            <TableBodyRow class="hover:bg-[var(--lu-gold)]">
                                <!-- Md display only -->
                                <TableBodyCell>
                                    <p class="block lg:hidden">Mon. - Thurs.</p>
                                    <p class="hidden lg:block">Monday <br> - <br> Thursday</p>
                                </TableBodyCell>
                
                                
                                <!-- Lg display only -->
                                <TableBodyCell >
                                    <p class="block lg:hidden"> 9 A.M. - 6 P.M.</p>
                                    <p class="hidden lg:block">9 A.M. <br> - <br> 6 P.M.</p>
                                </TableBodyCell>

                            </TableBodyRow>

            
                            <TableBodyRow class="hover:bg-[var(--lu-gold)]"> 
                                <TableBodyCell>Friday</TableBodyCell>

                                <!-- Md screen  -->
                                <TableBodyCell class="block lg:hidden">9 A.M. - 4 P.M. </TableBodyCell>
                                <!-- Lg screen -->
                                <TableBodyCell class="hidden lg:block"><p>9 A.M.</p> <p>-</p> <p>4 P.M.</p> </TableBodyCell>
                            </TableBodyRow>
            
                            <TableBodyRow class="hover:bg-[var(--lu-gold)]">
                                <!-- Md display only -->
                                <TableBodyCell class="block lg:hidden">Sat. - Sun.</TableBodyCell>

                                <!-- Lg display only -->
                                <TableBodyCell class="hidden lg:block"><p>Saturday</p> <p>-</p> <p>Sunday</p> </TableBodyCell>

                                <TableBodyCell>Closed</TableBodyCell>
                            </TableBodyRow>
            
                        </TableBody>
            
                    </Table>
                </div>

            </div>
            
        </div>
        
    </div>

    

    <!-- COL 3: Buttons to other page of website -->
    <div class="lg:col-span-3 lg:mt-10 text-center col-span-3">

        <div>
            <h1 class="gold-color mb-5">Take a look around!</h1>
            <div class="flex flex-col lg:flex-row  w-5/6 md:w-1/3 mx-auto lg:w-1/2 lg:mx-auto justify-center items-center mb-5 mt-0">
        
                <!-- Tutor -->
                <p class="md:hidden w-2/3">Meet the Tutors!</p>
                <Button id="about-ref" class="m-2 w-1/3  md:w-2/3 md:h-8 lg:w-1/3 lg:h-12 hover:bg-[#b5a36a] bg-white black-color" color="dark" pill href="about-page"> 
                    <UsersGroupOutline class="w-4 h-4 me-2"/> The Tutors
                </Button>
                
                <!-- Popover for med device -->
                <Popover 
                    reference="#about-ref"
                    triggeredBy="#about-ref"
                    class="w-64 text-sm font-light text-center hidden md:block lg:hidden black-color" 
                    title="The Tutors"
                    placement="right"> 
                    Meet the tutors at the center!
                </Popover>
                <!-- Popover for lg device -->
                <Popover 
                    reference="#about-ref"
                    triggeredBy="#about-ref"
                    class="w-64 text-sm font-light text-center hidden lg:block black-color" 
                    title="The Tutors"
                    transition={slide}> 
                    Meet the tutors at the center!
                </Popover>
                
                
                <!-- TUTORING -->
                <p class="md:hidden mt-5 w-2/3">Use the tutor lookup tool or view the full tutor schedule!</p>
                <Button id="tutoring-ref" class="m-2 w-1/3 md:w-2/3 md:h-8 lg:w-1/3 lg:h-12 hover:bg-[#b5a36a]  bg-white black-color" color="dark" pill href="tutoring-page">
                    <SearchOutline class="w-4 h-4 me-2" /> Scheduling
                </Button>
                <!-- Popover for lg display -->
                <Popover
                    reference="#tutoring-ref"
                    triggeredBy="#tutoring-ref"
                    class="w-64 text-sm font-light text-center hidden lg:block black-color"
                    title="Tutoring Service" 
                    placement="top"
                    transition={slide}>
                    Use our lookup tool to find a tutor or time that fits your needs!
                </Popover>
                <!-- Popover for md display -->
                <Popover
                    reference="#tutoring-ref"
                    triggeredBy="#tutoring-ref"
                    class="w-64 text-sm font-light text-center hidden md:block lg:hidden black-color"
                    title="Tutoring Service" 
                    placement="left">
                    Use our lookup tool to find a tutor or time that fits your needs!
                </Popover>
                
                
                <!-- CONTACT -->
                <p class="md:hidden mt-5 w-2/3">Get in touch with the director or leave feedback!</p>
                <Button id="contact-ref" class="m-2 w-1/3 md:w-2/3 md:h-8 lg:w-1/3 lg:h-12 hover:bg-[#b5a36a]  bg-white black-color" color="dark" pill href="contact-page"> 
                    <AddressBookOutline class="w-4 h-4 me-2"/> Contact
                </Button>
                <!-- Popover med display -->
                <Popover class="w-64 text-sm font-light text-center hidden md:block lg:hidden black-color"
                    reference="#contact-ref"
                    triggeredBy="#contact-ref"
                    title="Contact Information"
                    placement="right"
                    >
                    Get in touch with the director or leave feedback!
                </Popover>
                <!-- Popover lg display -->
                <Popover class="w-64 text-sm font-light text-center hidden lg:block black-color"
                    reference="#contact-ref"
                    triggeredBy="#contact-ref"
                    title="Contact Information"
                    placement="top"
                    transition={slide}> 
                    Get in touch with the director or leave feedback!
                </Popover>
                
                <!-- RESOURCES -->
                <p class="md:hidden mt-5 w-2/3">Find additional learning tools or references in our curated collection of resources!</p>
                <Button id="resources-ref" class="m-2 w-1/3 md:w-2/3 md:h-8 lg:w-1/3 lg:h-12 hover:bg-[#b5a36a]  bg-white black-color" color="dark" pill href="resource-page"> 
                    <FolderOutline class="w-4 h-4 me-2"/> Resources 
                </Button>
                <!-- Popover md display -->
                <Popover
                    reference="#resources-ref"
                    triggeredBy="#resources-ref"
                    class="w-64 text-sm font-light text-center hidden md:block lg:hidden black-color"
                    title="Resources"
                    placement="left">
                    Find additional learning tools or references in our curated collection of resources!
                </Popover>
                <!-- Popover lg display -->
                <Popover
                    reference="#resources-ref"
                    triggeredBy="#resources-ref"
                    class="w-64 text-sm font-light text-center hidden lg:block black-color"
                    title="Resources"
                    placement="top"
                    transition={slide}> 
                    Find additional learning tools or references in our curated collection of resources!
                </Popover>
        
            </div>
    
            <div class="mx-auto hidden lg:block p-0 mb-16">
                <p class="italic animate-pulse">(Hover over each button to see its description)</p>
            </div>

        </div>

    </div>



</main>




<style>
    /* Change Button color */
    h1{
        font-weight: bold;
        font-size: x-large;
        margin-top: 30px;
        text-transform: uppercase;
        text-align: center;
    }
    span{
        font-weight: bold;
        text-transform: uppercase;
    }

</style>
