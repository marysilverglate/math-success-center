<script>
    import NavbarComp from "$lib/NavbarComp.svelte";
    import { Checkbox, Listgroup, Tabs, TabItem} from 'flowbite-svelte';
    import { Table, TableBody, TableBodyCell, TableBodyRow, TableHead, TableHeadCell, Label } from 'flowbite-svelte';
    import { onMount } from 'svelte';
    
    // tutor filter
    let selectedSubjects = [];
    let result = [];
    
    // Function to handle changes
    function handleSelectionChange(event) {
        const isChecked = event.target.checked;
        const value = event.target.value;
    
        if (isChecked) {
            selectedSubjects = [...selectedSubjects, value]; // Add value reactively
        } else {
            selectedSubjects = selectedSubjects.filter(item => item !== value); // Remove value reactively
        }

        // clear result array before new search
        result.length=0;
        findTutor(event); // search for tutor
        // printResult(); // print all tutor found, if any
    }

    // Find all tutors that can tutor the specified subject at current time
    function findTutor(e){
        
        let currTime = getCurrHr() + (getCurrMin()/60);
        let currDay = getCurrDay();
        // debug
        // alert("Currtime: " + currTime);

        // Base case: not within operating days
        if (currDay === 0 || currDay === 6){
            // alert('The Math Success Center operates only on weekdays. \n\nPlease refer to the tutor-specific schedule toward the end of the page.');
            findTutorAfterHours();
            // e.target.checked=false;//uncheck
            printResult("The Math Success Center operates only on weekdays. Below are the tutor(s) available for the selected subject. Please refer to the tutor-specific schedule for their availability.");
            return; 
        }

        // Base case: not within operating hours  
        else if ( currDay >=1 && currDay <= 4){ //M-R
            if (currTime < 9 || currTime > 18){ 
                // e.target.checked=false; //uncheck
                findTutorAfterHours();
                printResult("The Math Success Center operates from 9 AM to 6 PM, Monday through Thursday. Below are the tutor(s) available for the selected subject. Please refer to the tutor-specific schedule for their availability.");
                return;
            }
        } else if (currTime < 9 || currTime > 16){ //F
                // alert('The Math Success Center operates between 9-4 PM on Fridays. \n\nPlease refer to the tutor-specific schedule toward the end of the page.');
                // e.target.checked=false; //uncheck
                findTutorAfterHours();
                printResult("The Math Success Center operates from 9 AM to 4 PM on Fridays. Below are the tutor(s) available for the selected subject. Please refer to the tutor-specific schedule for their availability.");
                return;
        }

        
        // both empty => nothing to do
        if (selectedSubjects.length <=0 ){
            result.length=0;
            printResult();
            return;
        }

        // only subject selected
        if(selectedSubjects.length > 0)
        {
            for (let tutor in tutorList){
                for (let subject in selectedSubjects){
                    let searchResult = tutorList[tutor].t_subject.indexOf(selectedSubjects[subject]);
                    if (searchResult !== -1){
                        // are they tutoring at current time

                        let dayCounter = 1;  //start at monday
                        tutorList[tutor].t_hours.forEach((dayOfWeek)=> {
                           
                            if (dayCounter === currDay){
                                
                                // && currDay === dayCounter
                                dayOfWeek.forEach( (timeBlock)=>{
                                    let startHr = Number(timeBlock[0]);
                                    let endHr = Number(timeBlock[1]);
                                    
                                    
                                    console.log('currDay: ' + currDay );
                                    if (startHr <= currTime && currTime <= endHr){
                                        // unqiue name
                                        if(result.indexOf(tutorList[tutor]) == -1) {
                                            console.log("currDay: " + currDay +  ' | dayCounter: ' + dayCounter + " | currTime: " + currTime +  "| Tutor: " + tutorList[tutor].t_name + " | startHr: " + startHr + " | endHr: " + endHr);
                                            result.push(tutorList[tutor]); 
                                            return;
                                        }
                                    }

                                });
                                
                            }
                            
                            dayCounter++;
                        });
                        dayCounter=0;
                        
                    }
                }
            }
            if (result.length > 0)
                printResult();
            else{
                const resultField = document.getElementById('result-field');
                resultField.innerText = "";
                let liEl = document.createElement('li');
                liEl.style.padding = "5px";
                liEl.innerText = "Unfortunately, there are no available tutors for the selected subject(s). Please refer to the tutor-specific schedule to plan for a different time.";
                resultField.appendChild(liEl);
                e.target.checked=false; //uncheck
                return;
            }
            return;
        }
        
     
        // IF YOU WANT TO IMPLEMENT SEARCHING FOR A PARTICULAR TUTOR (FUTURE FEATURE)
        // for (let tutor in tutorList){

        //     // check if they match specified tutor
        //     let isSpecifiedTutor = selectedTutors.indexOf(tutorList[tutor].t_name);
            
        //     // found
        //     if (isSpecifiedTutor > 0){
        //         // check if they tutor specified subject at current time
                
        //         for (let subject in selectedSubjects){
        //             let subjectResult = tutorList[tutor].t_subject.indexOf(selectedSubjects[subject]);

        //             // subject found
        //             if (subjectResult > 0 ) {
                        
        //                 // // check time
                        
        //                 // tutorList[tutor].t_hours.forEach((dayOfWeek)=> {
        //                 //     dayOfWeek.forEach( (timeBlock)=>{
        //                 //         let startHr = timeBlock[0];
        //                 //         let endHr = timeBlock[0];
        //                 //         if (startHr <= getCurrHr() && getCurrHr() <= endHr)
        //                 //             result.push(tutorList[tutor]);
        //                 //     });
        //                 // });

        //                 // FOR NOW!!!
        //                 result.push(tutorList[tutor]);
        //             }
        //         }

        //     }
        // }

    }

    // Find all tutors that can tutor the select subjects
    function findTutorAfterHours(){
        for (let tutor in tutorList){
                for (let subject in selectedSubjects){
                    let searchResult = tutorList[tutor].t_subject.indexOf(selectedSubjects[subject]);
                    if (searchResult !== -1){ //this tutor can each this subject

                        // tutors can match many of the selected subject, ensure unique tutor in our result
                        let unique = true;
                        for(let tutorResult in result){
                            // let searchResultVal= result.indexOf(tutorList[tutor].t_name);
                            if (result[tutorResult].t_name === tutorList[tutor].t_name){
                                unique = false; 
                                break;
                            }
                        }
                        if (unique)
                            result.push(tutorList[tutor]); 
                    }
                }
        }
    }


    // Prints the result of of the findTutor function to the result field
    function printResult(message){

        const resultField = document.getElementById('result-field');
        resultField.innerText = "";
        
        // base case: nothing to print
        if (result.length <= 0 || !result){
            let liEl = document.createElement('li');
            liEl.style.padding = "5px";
            liEl.innerText = "Please select subject(s) above";
            resultField.appendChild(liEl);
            return;
        }

        if(message){
            // alert('has message: ' + message);
            let liEl = document.createElement('li');
            liEl.style.padding = "7px";
            liEl.style.fontSize = "16px";
            liEl.className = "black-color";
            liEl.innerText = message;
            resultField.appendChild(liEl);
        }


        // print all the tutors
        for (let val in result){

            let liEl = document.createElement('li');
            liEl.style.padding = "5px";
            liEl.style.fontWeight = "bold";
            liEl.style.fontSize = "18px";
            liEl.style.color = "#b5a36a";
            

            liEl.innerText = "> " + result[val].t_name;
            resultField.appendChild(liEl);
        }
    }

	// these automatically update when 'time' changes, because of the `$:` prefix
	let time = $state(new Date());
    let currTime = $derived(time.toLocaleTimeString());

	onMount(() => {
		const interval = setInterval(() => {
			time = new Date();
		}, 1000);

		return () => {
			clearInterval(interval);
		};
	});

    // get the live time
    function getCurrTime(){
        return currTime;
    }

    // get the current hour
    function getCurrHr(){
        let time = new Date();
        return time.getHours();
    }

    // get the current hour
    function getCurrMin(){
        let time = new Date();
        return time.getMinutes();
    }

    // get current date  [month/day/year]
    function getCurrDate(){
        let now = new Date(); 
        return (now.getMonth() + 1) + "/" + now.getDate() + "/" + now.getFullYear();
    }

    // get the current day of the week
    function getCurrDay(){
        let day = new Date();
        return day.getDay();
    }
    


    const kevin = {
        // Tutor name
        t_name : "Kevin", 
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 1", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [
                            [11, 12]
                          ],

                    /*T*/ [
                            [9,10], [10,11], [11,12], [12,13]
                          ],

                    /*W*/ [
                            [11, 12]
                          ],

                    /*R*/ [
                            [9,10], [10,11], [11,12], [12,13]
                          ],

                    /*F*/ [
                            [11,12], [12,13]
                          ]
                  ]
    };

    const brooke = {
        // Tutor name
        t_name : "Brooke", 
        // Tutor subject(s)
        t_subject : ["Calculus 1", "Physics", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [9, 10], [14, 15], [15,16], [16,17], [17,18]
                          ],
                    /*T*/ 
                          [
                            [9,10], [10,11], [14.5, 15], [15,16]
                          ],
                    /*W*/ 
                          [
                            [9,10]
                          ],
                    /*R*/ 
                          [
                            [9,10], [10,11], [14.5, 15], [15,16]
                          ], 
                    /*F*/ 
                          [
                            [9,10]
                          ] 
                   ]
    };

    const mattie = {
        // Tutor name
        t_name : "Mattie", 
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 2", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [12,13], [13,14], [14,15]
                          ],
                    /*T*/ 
                          [
                            [14,15], [15,16], [16,17], [17,18]
                          ],
                    /*W*/ 
                          [
                            [14,15], [15,16], [16,17], [17,18]
                          ],
                    /*R*/ 
                          [
                            [12.5, 13], [13,14]
                          ], 
                    /*F*/ 
                          [
                            [9,10], [10, 10.5]
                          ] 
                   ]
    };

    const bennett = {
        // Tutor name
        t_name : "Bennett", 
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 1", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [9,10], [10,11], [11,11.5]
                          ],
                    /*T*/ 
                          [
                            [13, 14], [14, 15], [15,15.5]
                          ],
                    /*W*/ 
                          [
                            [9, 10], [10, 11], [11,11.5]
                          ],
                    /*R*/ 
                          [
                            [11, 12], [13.5, 14], [14, 15], [15, 15.5]
                          ], 
                    /*F*/ 
                          [
                            [10.25, 11], [11,11.5], [13.5, 14], [14, 15], [15, 15.5]
                          ] 
                   ]
    };

    const seth = {
        // Tutor name
        t_name : "Seth", 
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 2", "Calculus 1", "Physics", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [10, 11], [13,14]
                          ],
                    /*T*/ 
                          [
                            [13, 14], [14, 14.5]
                          ],
                    /*W*/ 
                          [
                            [10,11], [11,11.5]
                          ],
                    /*R*/ 
                          [
                            [13,14], [14,15], [15,16], [16, 17], [17, 18]
                          ], 
                    /*F*/ 
                          [
                            [10,11], [13,14], [14,15], [15,16]
                          ] 
                   ]
    };

    const jamie = {
        // Tutor name
        t_name : "Jamie", 
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 2", "Calculus 1", "Physics", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [11,12], [13,14], [15.5,16], [16,17], [17,18]
                          ],
                    /*T*/ 
                          [
                            [13, 14]
                          ],
                    /*W*/ 
                          [
                            [11,12], [13,14], [15.5, 16], [16,17], [17,18]
                          ],
                    /*R*/ 
                          [
                            [13,14], [16,17], [17,18]
                          ], 
                    /*F*/ 
                          [
                            [11,12]
                          ] 
                   ]
    };

    const drJ = {
        // Tutor name
        t_name : "Dr. J (Jeevanjee)", 
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 2", "Calculus 1", "Physics", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                          ],
                    /*T*/ 
                          [
                          ],
                    /*W*/ 
                          [
                            [12, 13], [13,14], [14,15]
                          ],
                    /*R*/ 
                          [
                          ], 
                    /*F*/ 
                          [
                            [13,14], [14,15]
                          ] 
                   ]
    };

    const katherine = {
        // Tutor name
        t_name : "Katherine", 
        // Tutor subject(s)
        t_subject : ["College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [16,17], [17,18]
                          ],
                    /*T*/ 
                          [
                            [11,12], [16,17], [17,18]
                          ],
                    /*W*/ 
                          [
                            [12,13], [13,14], [16.5,17], [17,18]
                          ],
                    /*R*/ 
                          [
                            [11,12], [16,17], [17,18]
                          ], 
                    /*F*/ 
                          [
                            [12,13]
                          ] 
                   ]
    };

    const mary = {
        // Tutor name
        t_name : "Mary", 
        // Tutor subject(s)
        t_subject : ["College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus", "Calculus 1", "Calculus 2"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [12,13], [14,15], [15,15.5], []
                          ],
                    /*T*/ 
                          [
                            [12,13] 
                          ],
                    /*W*/ 
                          [
                            [12,13], [14,15], [15,15.5]
                          ],
                    /*R*/ 
                          [
                            [12,13]
                          ], 
                    /*F*/ 
                          [
                            [15,16]
                          ] 
                   ]
    };

    




    // create list to hold tutor infomation
    const tutorList = [ 
                     kevin, 
                     brooke, 
                     mattie, 
                     drJ, 
                     bennett, 
                     jamie, 
                     seth, 
                     katherine,
                     mary
                      ];
        


   
</script>

<svelte:head>
    <title>Math Success Center - Tutoring</title>
    <meta name="description" content="The Math Success Center, a free tutoring service provided by Lindenwood University. Tutors available for various university math courses, as well as computer science and physics."/>
    <link rel="icon" href={"favicon.png?v=1"} />
</svelte:head>
<NavbarComp/>

<main>

    <!-- Hero image -->
    <div class="w-5/6  mx-auto mt-32">
        <div class="text-center w-full">
            <img src="./msc-inside-2.jpeg" alt="Image of the interior of the Math Success Center" class="my-5 shadow-xl rounded-3xl">
        </div>
    </div>

    <div class="flex flex-col items-center">
        <h2 class="text-4xl font-bold mt-10"> Tutor Schedule </h2>
        <p class="w-3/4 md:w-2/3 lg:w-1/2 indent-7 m-4 leading-loose"> Each tutor is capable of teaching all areas of math; however, not all tutors can cover specialized subjects such as Calculus 2, Physics, and Computer Science. Additionally, each tutor has different available tutoring times. Please select subject(s) to find the tutor(s) or time slots that best fit your schedule and meet your needs.</p>
    </div>

    
    
    <!-- Select tutor or subject -->
    
    <div class="grid grid-cols-1 sm:text-sm w-5/6 lg:2/3  p-5 mx-auto">
        <!-- col 1 : Select Subject-->
        <div class="flex flex-col items-center">
            <h2 class="text-2xl font-bold mt-5"> Search for a Tutor! </h2>
            <p class="m-4">Please select all the subjects you need help with, and we will find tutors for you!</p>
        
            <div class="border border-gray-300 p-4 rounded-xl flex flex-col shadow-lg bg-white">
                <!-- GENERAL SUBJECTS -->
                 
                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" type="checkbox" value="College Algebra" on:change={(e) => handleSelectionChange(e)}/>
                    College Algebra (MTH 15100)
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Intermediate Algebra" on:change={(e) => handleSelectionChange(e)}/>
                    Intermediate Algebra (MTH 11000)
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Mathematical Structures for Teachers I" on:change={(e) => handleSelectionChange(e)}/>
                    Mathematical Structures for Teachers I (MTH 14800)
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Mathematical Structures for Teachers II" on:change={(e) => handleSelectionChange(e)}/>
                    Mathematical Structures for Teachers II (MTH 14900)
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Precalculus" on:change={(e) => handleSelectionChange(e)}/>
                    Precalculus (MTH 14200)
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Basic Statistics" on:change={(e) => handleSelectionChange(e)}/>
                    Basic Statistics (MTH 14100)
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Statistics for Natural Science" on:change={(e) => handleSelectionChange(e)}/>
                    Statistics for Natural Science (MTH 24100)    
                </Label>
                
                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Survey of Calculus" on:change={(e) => handleSelectionChange(e)}/>
                    Survey of Calculus (MTH 17300)
                </Label>
                
                
                
                <!-- SPECIALTY SUBJECT HOURS -->
                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Calculus 1" on:change={(e) => handleSelectionChange(e)}/>
                    Calculus 1
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Calculus 2" on:change={(e) => handleSelectionChange(e)}/>
                    Calculus 2
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Physics" on:change={(e) => handleSelectionChange(e)}/>
                    Physics
                </Label>

                <Label class="flex items-center">
                    <Checkbox class="p-2 text-[var(--alt-lu-gold)]" value="Computer Science" on:change={(e) => handleSelectionChange(e)}/>
                    Computer Science 
                </Label>
            </div>
        </div>
        
        <!-- col 2 : Select Tutor
        <div class=" flex flex-col items-center justify-center">
            <h1 class="mb-2 font-bold">Select Tutor(s):</h1>
            <div class="w-full h-full border border-black p-2">
                <Checkbox class="p-2" value="Brooke" on:change={(e) => handleSelectionChange(e, "tutor")}>Brooke</Checkbox>
                <Checkbox class="p-2" value="Mattie" on:change={(e) => handleSelectionChange(e, "tutor")}>Mattie</Checkbox>
                <Checkbox class="p-2" value="Seth" on:change={(e) => handleSelectionChange(e, "tutor")}>Seth</Checkbox>
                <Checkbox class="p-2" value="Jamie" on:change={(e) => handleSelectionChange(e, "tutor")}>Jamie</Checkbox>
                <Checkbox class="p-2" value="Katherine" on:change={(e) => handleSelectionChange(e, "tutor")}>Katherine</Checkbox>
                <Checkbox class="p-2" value="Kevin" on:change={(e) => handleSelectionChange(e, "tutor")}>Kevin</Checkbox>
                <Checkbox class="p-2" value="Mary" on:change={(e) => handleSelectionChange(e, "tutor")}>Mary</Checkbox>
                <Checkbox class="p-2" value="Bennett" on:change={(e) => handleSelectionChange(e, "tutor")}>Bennett</Checkbox>
                <Checkbox class="p-2" value="Dr. J (Jeevanjee)" on:change={(e) => handleSelectionChange(e, "tutor")}>Dr. J (Jeevanjee)</Checkbox>
            </div>
        </div>  -->
    </div>

    <!-- Display current date and time -->
    <div class="flex justify-center col-span-2 m-5">
        <!-- current date -->
        <h3 class="px-2 py-2 lg:px-10">
            <b>Today:</b> <span class="gold-color text-lg ">{getCurrDate()}</span> 
        </h3>
    
        <!-- current time -->
        <h3 class="px-2 py-2 lg:px-10">
            <b>Time:</b> <span class="gold-color text-lg">{getCurrTime()}</span> 
        </h3>

    </div>
    

    <h1>Results</h1>
    <div class="mb-10 p-5 border border-gray-200 shadow-md rounded-xl w-2/3 sm:w-1/2 md:w-2/5 max-w-96 flex mx-auto justify-center">
        <ul id="result-field">
            <li>Please select the subject(s) above</li>
        </ul>
    </div>

    <hr class="">

    <!-- Tutor Schedule -->
    <h2 class="text-4xl font-bold mt-10 text-center"> Tutor-Specific Scheudle </h2> 

    <p class="mt-5 w-1/2 mx-auto text-center">Here is the full tutoring schedule if you'd like to plan ahead or prefer to manually search for a tutor.</p>

    <!-- Tutor Scheudle Tablet and Desktop view -->
    <div class="flex justify-center mb-20">
          
        <div class="shadow-xl shadow-[var(--lu-gold)] rounded-xl">
            <Table shadow class="hidden lg:block md:text-xs w-3/4">
            
                <!-- Days of the week -->
                <TableHead class="text-center"> 
                    
                    <TableHeadCell>Hours</TableHeadCell>
                    <TableHeadCell>Monday</TableHeadCell>
                    <TableHeadCell>Tuesday</TableHeadCell>
                    <TableHeadCell>Wednesday</TableHeadCell>
                    <TableHeadCell>Thursday</TableHeadCell>
                    <TableHeadCell>Friday</TableHeadCell>

                </TableHead>
                <TableBody tableBodyClass="divide-y">

                    <TableBodyRow>
                        <TableBodyCell>9:00 - 10:00 A.M.</TableBodyCell>

                        <TableBodyCell>
                            <Listgroup items={['Brooke', 'Bennett']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>

                        <TableBodyCell>
                            <Listgroup items={['Brooke', 'Kevin']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>

                        <TableBodyCell>
                            <Listgroup items={['Brooke', 'Bennett']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>

                        <TableBodyCell>
                            <Listgroup items={['Brooke', 'Kevin']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>

                        <TableBodyCell>
                            <Listgroup items={['Brooke', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>

                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Seth', 'Bennett']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Kevin', 'Brooke']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Seth', 'Bennett']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Brooke', 'Kevin']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Seth', 'Mattie (-10:30)', 'Bennett (10:15-)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>
                    
                    <TableBodyRow>
                        <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Kevin', 'Bennett (-11:30)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Kevin', 'Katherine']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Kevin', 'Bennett (-11:30)', 'Seth (-11:30)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Kevin', 'Katherine', 'Bennett']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Kevin', 'Bennett (-11:30)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Mary', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Kevin', 'Mary']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Katherine', 'Mary', 'Dr. J']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Kevin', 'Mary', 'Mattie (12:30-)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Kevin', 'Katherine']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Mattie', 'Jamie', 'Seth']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Seth', 'Bennett']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Katherine', 'Dr. J']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Seth', 'Bennett (1:30-)', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Dr. J', 'Seth', 'Bennett (1:30-)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Mattie', 'Brooke', 'Mary']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Seth (-2:30)', 'Brooke (2:30-)', 'Bennett', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Mary', 'Dr. J', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Seth', 'Brooke (2:30-)', 'Bennett']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Seth', 'Dr. J', 'Bennett']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Mary (-3:30)', 'Brooke', 'Jamie (3:30-)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Brooke', 'Bennett (-3:30)', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Mary (-3:30)', 'Jamie (3:30-)', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Brooke', 'Seth', 'Bennett (-3:30)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Mary', 'Seth', 'Bennett (-3:30)']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Brooke', 'Katherine']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Katherine', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Katherine (4:30-)', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Katherine', 'Seth']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['CLOSED']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Brooke', 'Katherine']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Katherine', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Katherine', 'Mattie']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['Jamie', 'Katherine', 'Seth']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <TableBodyCell>
                            <Listgroup items={['CLOSED']} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>
                </TableBody>
            </Table>
        </div>
    </div>

    <!-- Tutor Scheudle Mobile view -->

    <div class="flex justify-center pb-5 mt-0 lg:hidden">
        <div class="flex flex-col items-center py-5">
            <Tabs tabStyle="underline" >

                <!-- Monday -->
                <TabItem open defaultClass="p-4" activeClasses="border-b-2 border-[#b5a36a] gold-color" inactiveClasses="text-white">

                    <span slot="title"> MON </span>
                    
                    <Table shadow>
                        <TableBody tableBodyClass="divide-y">
            
                            <TableBodyRow>
                                <TableBodyCell>9:00 - 10:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Bennett']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Seth', 'Bennett']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Kevin', 'Bennett (-11:30)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Mary', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                    <TableBodyCell>
                                        <Listgroup items={['Mattie', 'Jamie', 'Seth']} let:item class="w-25 text-center shadow-md black-color">
                                            {item}
                                        </Listgroup>
                                    </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Mattie', 'Brooke', 'Mary']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Mary (-3:30)', 'Brooke', 'Jamie (3:30-)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Brooke', 'Katherine']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Brooke', 'Katherine']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                        </TableBody>
                    </Table>    
                </TabItem>

                <!-- Tuesday -->
                <TabItem defaultClass="p-4" activeClasses="border-b-2 border-[#b5a36a] gold-color" inactiveClasses="text-white">
                    <span slot="title">TUE</span>
                    <Table>
                        <TableBody tableBodyClass="divide-y">
            
                            <TableBodyRow>
                                <TableBodyCell>9:00 - 10:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Kevin']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                    
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Kevin']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                    
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Kevin', 'Katherine']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Kevin', 'Mary']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow> 
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Seth', 'Bennett']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Seth (-2:30)', 'Brooke (2:30-)', 'Bennett', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Bennett (-3:30)', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Katherine', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Katherine', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                        </TableBody>
                    </Table>
                </TabItem>


                <!-- Wednesday -->
                <TabItem defaultClass="p-4" activeClasses="border-b-2 border-[#b5a36a] gold-color" inactiveClasses="text-white">
                    <span slot="title">WED</span>
                    <Table shadow >
                        <TableBody tableBodyClass="divide-y">
            
                            <TableBodyRow>
                                <TableBodyCell>9:00 - 10:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Bennett']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Seth', 'Bennett']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Kevin', 'Bennett (-11:30)', 'Seth (-11:30)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Katherine', 'Mary', 'Dr. J']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Katherine', 'Dr. J']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Mary', 'Dr. J', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Mary (-3:30)', 'Jamie (3:30-)', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Katherine (4:30-)', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Katherine', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                        </TableBody>
                    </Table>
                </TabItem>


                <!-- Thursday -->
                <TabItem defaultClass="p-4" activeClasses="border-b-2 border-[#b5a36a] gold-color" inactiveClasses="text-white">
                    <span slot="title">THU</span>
                    
                    <Table shadow>
                        <TableBody tableBodyClass="divide-y" >
            
                            <TableBodyRow>
                                <TableBodyCell>9:00 - 10:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Kevin']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>

                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>9:00 - 10:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Kevin']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>

                            </TableBodyRow>
                            

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                    <TableBodyCell>
                                    <Listgroup items={['Kevin', 'Katherine', 'Bennett']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Kevin', 'Mary', 'Mattie (12:30-)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Seth', 'Bennett (1:30-)', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Seth', 'Brooke (2:30-)', 'Bennett']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Seth', 'Bennett (-3:30)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Katherine', 'Seth']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Katherine', 'Seth']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                        </TableBody>
                    </Table>
                
                </TabItem>


                <!-- Friday -->
                <TabItem defaultClass="p-4" activeClasses="border-b-2 border-[#b5a36a] gold-color" inactiveClasses="text-white">
                    <span slot="title">FRI</span>
                    <Table shadow  >
                        <TableBody tableBodyClass="divide-y">
            
                            <TableBodyRow>
                                <TableBodyCell>9:00 - 10:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Brooke', 'Mattie']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>

                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>9:00 - 10:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={['Seth', 'Mattie (-10:30)', 'Bennett (10:15-)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>

                            </TableBodyRow>
                            

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Jamie', 'Kevin', 'Bennett (-11:30)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Kevin', 'Katherine']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Dr. J', 'Seth', 'Bennett (1:30-)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Seth', 'Dr. J', 'Bennett']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['Mary', 'Seth', 'Bennett (-3:30)']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['CLOSED']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={['CLOSED']} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                        </TableBody>
                    </Table>
                </TabItem>

            </Tabs>
        </div>
    </div>

</main>

<style>
    h1{
        font-weight: bold;
        font-size: x-large;
        margin-top: 30px;
        text-align: center;
        color: var(--lu-gold);
    }
    h2{
        color: var(--lu-gold);
    }
    h3{
        color: white;
    }
</style>