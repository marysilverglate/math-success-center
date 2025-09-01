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
        // console.log("Currtime: " + currTime, "\nCurrDay: " + currDay);

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
                // findTutorAfterHours();
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
                    if (searchResult !== -1){ //this tutor can teach this subject

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


        if(message){
            // alert('has message: ' + message);
            let liEl = document.createElement('li');
            liEl.style.padding = "7px";
            liEl.style.fontSize = "16px";
            liEl.className = "lu-gold-color";
            liEl.innerText = message;
            resultField.appendChild(liEl);
            return;
        }

        
        // base case: nothing to print
        if (result.length <= 0 || !result){
            let liEl = document.createElement('li');
            liEl.style.padding = "5px";
            liEl.innerText = "Please select subject(s) above";
            resultField.appendChild(liEl);
            return;
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



    const andrew = {
        // Tutor name
        t_name : "Andrew", 
        // Tutor color
        t_color : 'black',
        // Tutor subject(s)
        t_subject : ["Calculus 1", "Calculus 2", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours :  [
                    /*M*/ [
                            [16, 17], [17, 18]
                          ],

                    /*T*/ [
                            [11, 12] , [12, 13], [13, 14] , [14, 15]
                          ],

                    /*W*/ [
                            [9,10], [14, 15], [15, 16], [16, 17]
                          ],

                    /*R*/ [
                            [11, 12] , [12, 13], [13, 14] , [14, 15]
                          ],

                    /*F*/ [
                            
                          ]
                  ]
    };

    const bailey = {
        // Tutor name
        t_name : "Bailey", 
        // Tutor color
        t_color : 'black',
        // Tutor subject(s)
        t_subject : ["Calculus 1", "Calculus 2", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [9, 10], [10, 11], [11,12], [12,12.5]
                          ],
                    /*T*/ 
                          [
                            [9.5,10], [10,11], [11,12], [12,12.5] [15, 16], [16,17], [17,18]
                          ],
                    /*W*/ 
                          [
                            [10, 11], [11,12], [12,12.5]
                          ],
                    /*R*/ 
                          [
                            [9.5,10], [10,11], [11,12], [12,12.5]
                          ], 
                    /*F*/ 
                          [
                            
                          ] 
                   ]
    };

    const mattie = {
        // Tutor name
        t_name : "Mattie", 
        // Tutor Color
        t_color : 'red',
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 1", "Calculus 2", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [9,10], [10,11] , [13,14] , [14,15] , [15,16] , [16,17] , [17,18]
                          ],
                    /*T*/ 
                          [
                            [16,17], [17, 18]
                          ],
                    /*W*/ 
                          [
                            [9,10], [10,11], [15,16], [16,17], [17, 18]
                          ],
                    /*R*/ 
                          [
                            
                          ], 
                    /*F*/ 
                          [
                            [10,11]
                          ] 
                   ]
    };

    const nestor = {
        // Tutor name
        t_name : "Néstor", 
        // Tutor Color
        t_color : 'black',
        // Tutor subject(s)
        t_subject : ["Calculus 1", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [12,13], [13,14], [14,15]
                          ],
                    /*T*/ 
                          [
                            [12.5,13], [13,14]
                          ],
                    /*W*/ 
                          [
                            [12,13], [13,14], [17,18]
                          ],
                    /*R*/ 
                          [
                            [12.5,13], [13,14] , [14,15] , [15,16], [17,18]
                          ], 
                    /*F*/ 
                          [
                            [14,15]
                          ] 
                   ]
    };

    const seth = {
        // Tutor name
        t_name : "Seth", 
        // Tutor color
        t_color : "purple",
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 2", "Calculus 1", "Physics", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [11, 12], [15,16], [16,16.5]
                          ],
                    /*T*/ 
                          [
                            [10.5,11], [11,12], [12,12.5]
                          ],
                    /*W*/ 
                          [
                            [10,11], [11,12] , [13,14], [15,16] , [16,16.5]
                          ],
                    /*R*/ 
                          [
                            [10.5,11], [11,12] , [14.5,15], [15,16], [16,17]
                          ], 
                    /*F*/ 
                          [
                            [13,14], [15,16]
                          ] 
                   ]
    };

    const alyssa = {
        // Tutor name
        t_name : "Alyssa", 
        // Tutor color
        t_color : "purple",
        // Tutor subject(s)
        t_subject : ["Computer Science", "Calculus 2", "Calculus 1", "Physics", "College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [11,12] , [12,13]
                          ],
                    /*T*/ 
                          [
                            [12.5,13], [13,14], [14,15], [15,16]
                          ],
                    /*W*/ 
                          [
                            [11,12] , [12,13] , [14,15]
                          ],
                    /*R*/ 
                          [
                            [12,13], [13,14] , [14,14.5], [17,18]
                          ], 
                    /*F*/ 
                          [
                            [11,12] , [12,13], [14,15]
                          ] 
                   ]
    };

    const drJ = {
        // Tutor name
        t_name : "Dr. J (Jeevanjee)", 
        // Tutor color
        t_color : "red",
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
                            
                          ],
                    /*R*/ 
                          [
                          ], 
                    /*F*/ 
                          [
                            [9,10], [10,11]
                          ] 
                   ]
    };

    const katherine = {
        // Tutor name
        t_name : "Katherine", 
        // Tutor color
        t_color : "black",
        // Tutor subject(s)
        t_subject : ["College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [16.5, 17], [17, 18]
                          ],
                    /*T*/ 
                          [
                            [16, 17], [17, 18]
                          ],
                    /*W*/ 
                          [
                            [16.5, 17], [17, 18]
                          ],
                    /*R*/ 
                          [
                            [16, 17], [17, 18]
                          ], 
                    /*F*/ 
                          [
                            [9,10], [10,11] , [11,12] , [12,13], [13,14]
                          ] 
                   ]
    };

    const mary = {
        // Tutor name
        t_name : "Mary", 
        // Tutor color
        t_color : "black",
        // Tutor subject(s)
        t_subject : ["College Algebra", "Intermediate Algebra", "Mathematical Structures for Teachers I", "Mathematical Structures for Teachers II", "Precalculus", "Basic Statistics", "Statistics for Natural Science", "Survey of Calculus", "Calculus 1", "Calculus 2"],
        // Tutor hours
        t_hours : [
                    /*M*/ [ 
                            [12.5,13], [13,14], [14,15]
                          ],
                    /*T*/ 
                          [
                            [9,10], [10,10.5]
                          ],
                    /*W*/ 
                          [
                            [12.5,13], [13,14], [14,15]
                          ],
                    /*R*/ 
                          [
                            [9,10], [10,10.5]
                          ], 
                    /*F*/ 
                          [

                          ] 
                   ]
    };



    // create list to hold tutor infomation
    const tutorList = [ 
                     nestor, 
                     bailey, 
                     mattie, 
                     drJ, 
                     andrew, 
                     alyssa, 
                     seth, 
                     katherine,
                     mary
                    ];
        

    //For full tutor schedule. Holds which tutors work at each time slot
    const days = ["MON", "TUE", "WED", "THUR", "FRI"];
    const timeSlots = [
                        [9,10], [10,11], [11,12],
                        [12,13], [13,14], [14,15],
                        [15,16], [16,17], [17,18]
                        ];
    
    const schedule = {};

    days.forEach((day, dayIndex) => {
    schedule[day] = {};
    timeSlots.forEach(([start, end]) => {
    schedule[day][`${start}_${end}`] = tutorList
      .filter(tutor => {
        const hours = tutor.t_hours?.[dayIndex];
        return Array.isArray(hours) && hours.some(([s, e]) => s < end && e > start);
      })
      .map(tutor => tutor.t_name);
    });
    });

/*   const MON_9_10 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 9 && slot[1] === 10));
    const MON_10_11 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 10 && slot[1] === 11));
    const MON_11_12 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 11 && slot[1] === 12));
    const MON_12_1 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 12 && slot[1] === 13));
    const MON_1_2 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 13 && slot[1] === 14));
    const MON_2_3 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 14 && slot[1] === 15));
    const MON_3_4 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 15 && slot[1] === 16));
    const MON_4_5 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 16 && slot[1] === 17));
    const MON_5_6 = tutorList.filter(tutor => tutor.t_hours[0]?.some(slot => slot[0] === 17 && slot[1] === 18));

    const TUE_9_10 = ['Mary', 'Bailey (9:30-)']
    const TUE_10_11 = ['Bailey', 'Seth (10:30-)', 'Mary (-10:30)']
    const TUE_11_12 = ['Andrew', 'Bailey', 'Seth']
    const TUE_12_1 = ['Andrew', 'Bailey (-12:30)', 'Seth (-12:30)', 'Alyssa (12:30-)', 'Néstor (12:30-)']
    const TUE_1_2 = ['Alyssa', 'Andrew', 'Néstor']
    const TUE_2_3 = ['Alyssa', 'Andrew']
    const TUE_3_4 = ['Alyssa', 'Bailey']
    const TUE_4_5 = ['Bailey', 'Katherine', 'Mattie']
    const TUE_5_6 = ['Bailey', 'Katherine', 'Mattie']

    const WED_9_10 = ['Andrew', 'Mattie']
    const WED_10_11 = ['Bailey', 'Mattie', 'Seth']
    const WED_11_12 = ['Alyssa', 'Bailey', 'Seth']
    const WED_12_1 = ['Alyssa', 'Néstor', 'Bailey (-12:30)', 'Mary (12:30-)']
    const WED_1_2 = ['Mary', 'Néstor', 'Seth']
    const WED_2_3 = ['Alyssa', 'Andrew', 'Mary']
    const WED_3_4 = ['Andrew', 'Mattie', 'Seth']
    const WED_4_5 = ['Andrew', 'Mattie', 'Seth (-4:30)', 'Katherine (4:30-)']
    const WED_5_6 = ['Katherine', 'Mattie', 'Néstor']

    const THUR_9_10 = ['Mary', 'Bailey (9:30-)']
    const THUR_10_11 = ['Bailey', 'Mary (-10:30)', 'Seth (10:30-)']
    const THUR_11_12 = ['Andrew', 'Bailey', 'Seth']
    const THUR_12_1 = ['Alyssa', 'Andrew', 'Bailey (-12:30)', 'Néstor (12:30-)']
    const THUR_1_2 = ['Alyssa', 'Andrew', 'Néstor']
    const THUR_2_3 = ['Andrew', 'Néstor','Alyssa (-2:30)', 'Seth (2:30-)']
    const THUR_3_4 = ['Néstor', 'Seth']
    const THUR_4_5 = ['Katherine', 'Seth']
    const THUR_5_6 = ['Alyssa', 'Katherine', 'Néstor']


    const FRI_9_10 = ['Katherine', 'Dr. J']
    const FRI_10_11 = ['Katherine', 'Dr. J', 'Mattie']
    const FRI_11_12 = ['Alyssa' , 'Katherine']
    const FRI_12_1 = ['Alyssa' , 'Katherine']
    const FRI_1_2 = ['Katherine', 'Seth']
    const FRI_2_3 = ['Alyssa', 'Néstor']
    const FRI_3_4 = ['Seth']
    const FRI_4_5 = ['CLOSED']
    const FRI_5_6 = ['CLOSED']
*/
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
                <Checkbox class="p-2" value="Alyssa" on:change={(e) => handleSelectionChange(e, "tutor")}>Alyssa</Checkbox>
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
    <h2 class="text-4xl font-bold mt-10 text-center"> Tutor-Specific Schedule </h2> 

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
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={schedule["MON"]["9_10"]} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                <span style="color: {item.t_color}; font-weight: bold;">
                                    {item.t_name}
                                  </span>
                            </Listgroup>
                        </TableBodyCell>

                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={schedule["TUE"]["9_10"]} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                <span style="color: {item.t_color}; font-weight: bold;">
                                    {item.t_name}
                                  </span>
                            </Listgroup>
                        </TableBodyCell>

                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={schedule["WED"]["9_10"]} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                <span style="color: {item.t_color}; font-weight: bold;">
                                    {item.t_name}
                                  </span>
                            </Listgroup>
                        </TableBodyCell>

                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={schedule["THUR"]["9_10"]} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                <span style="color: {item.t_color}; font-weight: bold;">
                                    {item.t_name}
                                  </span>
                            </Listgroup>
                        </TableBodyCell>

                        <!-- FRIDAY -->
                        <TableBodyCell>
                            <Listgroup items={schedule["FRI"]["9_10"]} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                <span style="color: {item.t_color}; font-weight: bold;">
                                    {item.t_name}
                                  </span>
                            </Listgroup>
                        </TableBodyCell>

                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={MON_10_11} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={TUE_10_11} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={WED_10_11} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={THUR_10_11} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- FRI -->
                        <TableBodyCell>
                            <Listgroup items={FRI_10_11} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>
                    
                    <TableBodyRow>
                        <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={MON_11_12} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={TUE_11_12} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={WED_11_12} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={THUR_11_12} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- FRI -->
                        <TableBodyCell>
                            <Listgroup items={FRI_11_12} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={MON_12_1} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={TUE_12_1} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={WED_12_1} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={THUR_12_1} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- FRI -->
                        <TableBodyCell>
                            <Listgroup items={FRI_12_1} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={MON_1_2} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={TUE_1_2} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={WED_1_2} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={THUR_1_2} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- FRI -->
                        <TableBodyCell>
                            <Listgroup items={FRI_1_2} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={MON_2_3} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={TUE_2_3} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={WED_2_3} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={THUR_2_3} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- FRI -->
                        <TableBodyCell>
                            <Listgroup items={FRI_2_3} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={MON_3_4} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={TUE_3_4} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={WED_3_4} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={THUR_3_4} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- FRI -->
                        <TableBodyCell>
                            <Listgroup items={FRI_3_4} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={MON_4_5} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={TUE_4_5} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={WED_4_5} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={THUR_4_5} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- FRI -->
                        <TableBodyCell>
                            <Listgroup items={FRI_4_5} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                    </TableBodyRow>

                    <TableBodyRow>
                        <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                        <!-- MON -->
                        <TableBodyCell>
                            <Listgroup items={MON_5_6} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- TUE -->
                        <TableBodyCell>
                            <Listgroup items={TUE_5_6} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- WED -->
                        <TableBodyCell>
                            <Listgroup items={WED_5_6} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- THU -->
                        <TableBodyCell>
                            <Listgroup items={THUR_5_6} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
                                {item}
                            </Listgroup>
                        </TableBodyCell>
                        <!-- FRI -->
                        <TableBodyCell>
                            <Listgroup items={FRI_5_6} let:item class="md:w-fit mx-auto text-center shadow-md black-color">
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
                                    <Listgroup items={MON_9_10} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={MON_10_11} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={MON_11_12} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={MON_12_1} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                    <TableBodyCell>
                                        <Listgroup items={MON_1_2} let:item class="w-25 text-center shadow-md black-color">
                                            {item}
                                        </Listgroup>
                                    </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={MON_2_3} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={MON_3_4} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={MON_4_5} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={MON_5_6} let:item class="w-25 text-center shadow-md black-color">
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
                                    <Listgroup items={TUE_9_10} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                    
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={TUE_10_11} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                    
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={TUE_11_12} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={TUE_12_1} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow> 
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={TUE_1_2} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={TUE_2_3} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={TUE_3_4} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={TUE_4_5} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={TUE_5_6} let:item class="w-25 text-center shadow-md black-color">
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
                                    <Listgroup items={WED_9_10} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={WED_10_11} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={WED_11_12} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={WED_12_1} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={WED_1_2} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={WED_2_3} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={WED_3_4} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={WED_4_5} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={WED_5_6} let:item class="w-25 text-center shadow-md black-color">
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
                                    <Listgroup items={THUR_9_10} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>

                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={THUR_10_11} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>

                            </TableBodyRow>
                            

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                    <TableBodyCell>
                                    <Listgroup items={THUR_11_12} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={THUR_12_1} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={THUR_1_2} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={THUR_2_3} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={THUR_3_4} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={THUR_4_5} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={THUR_5_6} let:item class="w-25 text-center shadow-md black-color">
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
                                    <Listgroup items={FRI_9_10} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>

                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>10:00 - 11:00 A.M.</TableBodyCell>
            
                                <TableBodyCell>
                                    <Listgroup items={FRI_10_11} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>

                            </TableBodyRow>
                            

                            <TableBodyRow>
                                <TableBodyCell>11:00 - 12:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={FRI_11_12} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>12:00 - 1:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={FRI_12_1} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>1:00 - 2:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={FRI_1_2} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>2:00 - 3:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={FRI_2_3} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>3:00 - 4:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={FRI_3_4} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>4:00 - 5:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={FRI_4_5} let:item class="w-25 text-center shadow-md black-color">
                                        {item}
                                    </Listgroup>
                                </TableBodyCell>
                            </TableBodyRow>

                            <TableBodyRow>
                                <TableBodyCell>5:00 - 6:00 P.M.</TableBodyCell>
                                <TableBodyCell>
                                    <Listgroup items={FRI_5_6} let:item class="w-25 text-center shadow-md black-color">
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