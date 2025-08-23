<script lang='ts'>
    import NavbarComp from "$lib/NavbarComp.svelte";

    import { Tabs, TabItem, Button, AccordionItem, Accordion  } from 'flowbite-svelte';
    import { WindowSolid, YoutubeSolid,  HammerOutline, ArrowRightOutline,FileDocOutline } from 'flowbite-svelte-icons';

    import { Modal } from 'flowbite-svelte';

    let defaultModal = false;
</script>

<svelte:head>
    <title>Math Success Center - Resources</title>
    <meta name="description" content="The Math Success Center, a free tutoring service provided by Lindenwood University. Tutors available for various university math courses, as well as computer science and physics."/>
    <link rel="icon" href={"favicon.png?v=1"} />
</svelte:head>

<NavbarComp />

<main class="w-5/6  mx-auto m-5">
    
    <!-- HERO SECTION -->
    <div class="text-center w-full mt-28">
        <img src="./msc-inside.jpeg" alt="Image of the interior of the Math Success Center" class="my-5 shadow-xl rounded-3xl">
    </div>


    <h1 class="font-bold text-3xl my-10 text-center uppercase gold-color"> Math Success Center Resources </h1>
    <p class="lg:w-2/3 text-justify mx-auto leading-loose indent-10">
        This page offers essential resources to help you excel in your math courses. Discover study guides, practice problems, and helpful tips on topics like algebra, calculus, and statistics. Use these tools to build your skills and boost your confidence in math.
    </p>
    <Accordion class="my-10 shadow-xl">
        <!-- ALGEBRA -->
        <AccordionItem open classActive="gold-bg black-color" classInactive="text-white">
            <span slot="header" class="uppercase mx-auto font-bold text-lg" id="navitemAlgebra">Algebra</span>
            
            <Tabs tabStyle="underline">
                <TabItem open defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex gap-2 uppercase text-white">
                        <YoutubeSolid size="md" />
                        Youtube
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ Lindenwood College Algebra Course Playlist
                            <a href="https://www.youtube.com/watch?v=z3tzYiooEHY&list=PLjFXKF5HhxEREC1Co8QUgntwhyd7liPqh&pp=gAQB" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Lindenwood Intermediate Algebra Course Playlist
                            <a href="https://www.youtube.com/watch?v=6-kWooJ46GY&list=PLjFXKF5HhxEQKU5sbZ3Qup8X6s0e6_sRU&pp=gAQB" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ The Organic Chemistry Tutor
                        <a href="https://youtube.com/playlist?list=PL0o_zxa4K1BUeF2o-MlNpbRiS-oE2Kn6J&si=_VPHQK1-2g3jiM7d" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>  
                    
                </TabItem>
                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex gap-2 uppercase text-white">
                        <WindowSolid size="md" />
                        Websites
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ Paul’s Online Math Notes (Lamar University)
                            <a href="https://tutorial.math.lamar.edu/" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Purple Math
                            <a href="https://www.mathhelp.com/college-algebra-help/?utm_campaign=purplemath&utm_source=_mh_calg&utm_medium=course" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>                 
        
                </TabItem>

                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <HammerOutline size="md" />
                        Tools
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>  
                            ○ Desmos (A graphing calculator and more)
                            <a href="https://www.desmos.com" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>
            </Tabs>
        </AccordionItem>

        <!-- Calculus -->
        <AccordionItem classActive="gold-bg black-color" classInactive="text-white">
            <span slot="header" class="uppercase mx-auto font-bold text-lg" id="navitemCalculus">Calculus</span>
            
            <Tabs tabStyle="underline">
                <TabItem open defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <YoutubeSolid size="md" />
                        Youtube
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ The Organic Chemistry Tutor
                            <a href="https://youtube.com/playlist?list=PL0o_zxa4K1BWYThyV4T2Allw6zY0jEumv&si=hafx1LTJx4HtFr1E" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Blackpenredpen
                            <a href="https://youtube.com/@blackpenredpen?si=4lPkEyfB4LBY1XXx" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                    
                </TabItem>
                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <WindowSolid size="md" />
                        Websites
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ Calculus.org
                            <a href="http://www.calculus.org" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Khan Academy (Calculus 1)
                            <a href="https://www.khanacademy.org/math/calculus-1" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Paul's Online Notes
                            <a href="https://tutorial.math.lamar.edu/Classes/CalcI/CalcI.aspx" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>

                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <HammerOutline size="md" />
                        Tools
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ Symbolab
                            <a href="https://www.symbolab.com/solver/calculus-calculator"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>  
                            ○ Desmos (A graphing calculator and more)
                            <a href="https://www.desmos.com" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>
            </Tabs>
        </AccordionItem>

        <!-- Statistics -->
        <AccordionItem classActive="gold-bg black-color" classInactive="text-white">
            <span slot="header" class="uppercase mx-auto font-bold text-lg">Statistics</span>
            
            <Tabs tabStyle="underline">
                <TabItem open defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <YoutubeSolid size="md" />
                        Youtube
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ Lindenwood Basic Statistics Course Playlist
                            <a href="https://www.youtube.com/watch?v=brDUSkylNMs&list=PLLYmZ8egDOoxo2QsEHAM4JgdzxJzf353J" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ The Organic Chemistry Tutor
                            <a href="https://youtube.com/playlist?list=PL0o_zxa4K1BVsziIRdfv4Hl4UIqDZhXWV&si=_5H2NOWDBepkvL10" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Brandon Foltz
                            <a href="https://youtube.com/playlist?list=PLIeGtxpvyG-L-ycD2bTJbu5uZZSRpC1wp&si=fdLqDY9gW2vREMeT" target="_blank" ><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ StatQuest with Josh Starmer
                            <a href="https://youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9&si=XYmT4VG2HaDFyA-z" target="_blank" ><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                
                    
                </TabItem>
                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <WindowSolid size="md" />
                        Websites
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ Math is Fun
                            <a href="https://www.mathsisfun.com/data/index.html" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Statistics How To
                            <a href="https://www.statisticshowto.com/probability-and-statistics/" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Khan Academy
                            <a href="https://www.khanacademy.org/math/statistics-probability" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                    
                </TabItem>

                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <HammerOutline size="md" />
                        Tools
                    </div>
                    <ul class="resource-list black-color pr-5">

                        <li>
                            <div>
                                <p>
                                    ○ Math 14100 Formula Sheet
                                </p>
                                <p class="text-sm">
                                    Useful if you are taking Basic Statistics.
                                </p>
                            </div>

                            <div>
                                <Button shadow={true} size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] shadow={true} y-2 hover:text-[var(--lu-black)] hover:animate-none" color="dark" on:click={() => (defaultModal = true)}>Show</Button>
                                <Modal title="14100 Formula Sheet" bind:open={defaultModal} autoclose>
                                    <center > <iframe src={"/14100 Formula sheet.pdf"} class="w-full h-screen"></iframe></center>
                                </Modal>
                                <a href="/14100 Formula sheet.pdf" target="_blank"><Button shadow={true} size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] my-1 hover:text-[var(--lu-black)] hover:animate-none" color="dark" outline={false}><FileDocOutline/></Button></a>
                            </div>
    
                        </li>

                        <li>
                            <div>
                                <p>
                                    ○ Math 24100 Formula Sheet
                                </p>
                                <p class="text-sm">
                                    Useful if you are taking Statistics for Natural Science.
                                </p>
                            </div>
                        
                            <div>
                                <Button shadow={true} size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] y-2 hover:text-[var(--lu-black)] hover:animate-none" color="dark" on:click={() => (defaultModal = true)}>Show</Button>
                                <Modal title="24100 Formula Sheet" bind:open={defaultModal} autoclose>
                                    <center> <iframe src={"/24100 Formula Sheet (all).pdf"} class="w-full h-screen" frameborder="0"></iframe></center>
                                </Modal>
                                <a href="/24100 Formula Sheet (all).pdf" target="_blank"><Button shadow={true} size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] my-1 hover:text-[var(--lu-black)] hover:animate-none" color="dark" outline={false}><FileDocOutline/></Button></a>
                            </div>
                            
                        </li>
                        
                    </ul>
                </TabItem>
            </Tabs>
        </AccordionItem>

        <!-- Computer Science -->
        <AccordionItem classActive="gold-bg black-color" classInactive="text-white">
            <span slot="header" class="uppercase mx-auto font-bold text-lg">Computer Science</span>
            
            <Tabs tabStyle="underline">
                <TabItem open defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <YoutubeSolid size="md" />
                        Youtube
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ The Coding Train
                            <a href="https://youtube.com/playlist?list=PLWFlMBumSLSbp_Cmwca3-zRiPDT10e85_&si=Nsu-GtYmYuoX3UsV" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ FreeCodeCamp.org
                            <a href="https://youtube.com/@freecodecamp?si=FxdRyYQUlpDw_XzF" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ CS Dojo
                            <a href="https://youtube.com/@csdojo?si=NuoyBsNYyDIlPhOj" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>

                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <WindowSolid size="md" />
                        Websites
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ Code Academy (Online code-learning platform)
                            <a href="https://www.codecademy.com" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ W3Schools 
                            <a href="https://www.w3schools.com" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ GeeksforGeeks
                            <a href="https://www.geeksforgeeks.org" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ CS50 Harvard Course
                            <a href="https://cs50.harvard.edu/college/2023/fall/syllabus/" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>

                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <HammerOutline size="md" />
                        Tools
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>  
                            ○ Visual Studio Code (IDE)
                            <a href="https://code.visualstudio.com" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Jupyter (IDE)
                            <a href="https://jupyter.org" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a> 
                        </li>
                        <li>
                            ○ GitHub (Version control)
                            <a href="https://github.com" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a> 
                        </li>
                        <li>
                            ○ LeetCode (Coding interview preparation)
                            <a href="https://leetcode.com" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a> 
                        </li>
                        
                    </ul>
                </TabItem>
            </Tabs>
        </AccordionItem>

        <!-- Physics -->
        <AccordionItem classActive="gold-bg black-color" classInactive="text-white">
            <span slot="header" class="uppercase mx-auto font-bold text-lg">Physics</span>
            
            <Tabs tabStyle="underline">
                <TabItem open defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <YoutubeSolid size="md" />
                        Youtube
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ DrPhysicsA
                            <a href="https://youtube.com/@drphysicsa?si=3n_bI4mqrscZl4gI" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Dot Physics
                            <a href="https://youtube.com/playlist?list=PLWFlMBumSLSbp_Cmwca3-zRiPDT10e85_&si=Nsu-GtYmYuoX3UsV" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>

                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <WindowSolid size="md" />
                        Websites
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            
                            ○ Hyperphysics
                            <a href="http://hyperphysics.phy-astr.gsu.edu/hbase/hframe.html" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                        <li>
                            ○ Physics Classroom
                            <a href="https://www.physicsclassroom.com/Physics-Video-Tutorial" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>

                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <HammerOutline size="md" />
                        Tools
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ PhET (Interactive Simulations)
                            <a href="https://phet.colorado.edu/en/simulations/filter?subjects=physics&type=html" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>
            </Tabs>
        </AccordionItem>

        <!-- Other -->
        <AccordionItem classActive="gold-bg black-color" classInactive="text-white">
            <span slot="header" class="uppercase mx-auto font-bold text-lg">Other</span>
            
            <Tabs tabStyle="underline">
                <TabItem open defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <YoutubeSolid size="md" />
                        Youtube
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            ○ Lindenwood Basic Mathematics Course Playlist
                            <a href="https://www.youtube.com/watch?v=qjk07NuEljU&list=PLjFXKF5HhxEQnkKTgGZhJI8c_-slQeZxp&pp=gAQB0gcJCV8EOCosWNin" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}><ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>

                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <WindowSolid size="md" />
                        Websites
                    </div>
                    <ul class="resource-list black-color pr-5">
                        <li>
                            <div>
                                <p>
                                    ○ Success in Mathematics (SLU)
                                </p>
                                <p class="text-sm">
                                    This guide from Saint Louis University (1993) offers comprehensive strategies for studying, problem-solving, test preparation, and seeking help in mathematics to foster understanding, skill development, and academic success.
                                </p>
                            </div>
                            <a href="https://mathstat.slu.edu/resources/success-in-mathematics" target="_blank"><Button size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] hover:text-[var(--lu-black)] hover:animate-none" shadow={true} color="dark" outline={false}> <ArrowRightOutline/> </Button></a>
                        </li>
                    </ul>
                </TabItem>


                <TabItem defaultClass="p-4" activeClasses="border-b-4 border-[#b5a36a] gold-color">
                    <div slot="title" class="flex items-center gap-2 uppercase text-white">
                        <HammerOutline size="md" />
                        Tools
                    </div>
                    <ul class="resource-list black-color pr-5">

                        <li>
                            <div>
                                <p>
                                    ○ Guide to Studying for Math
                                </p>
                                <p class="text-sm">
                                    Useful tips to use when studying mathematics.
                                </p>
                            </div>

                            <div>
                                <Button shadow={true} size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] shadow={true} y-2 hover:text-[var(--lu-black)] hover:animate-none" color="dark" on:click={() => (defaultModal = true)}>Show</Button>
                                <Modal title="How to Study for Math" bind:open={defaultModal} autoclose>
                                    <center > <iframe src={"/How to Study for Math.pdf"} class="w-full h-screen"></iframe></center>
                                </Modal>
                                <a href="/How to Study for Math.pdf" target="_blank"><Button shadow={true} size="sm" class="align-middle animate-pulse black-bg hover:bg-[#b5a36a] my-1 hover:text-[var(--lu-black)] hover:animate-none" color="dark" outline={false}><FileDocOutline/></Button></a>
                            </div>
    
                        </li>
                        
                    </ul>
                </TabItem>
            </Tabs>
        </AccordionItem>

    

    </Accordion>
</main>


    
<style>
    main{
        min-height: 100vh;
    }

    .resource-list li {
        padding: 10px;
        font-size: 18px;
        display: flex;
        justify-content: space-between;
    }
</style>