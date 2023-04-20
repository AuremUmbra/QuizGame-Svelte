<script>
    // Importing components
    import Question_page from "./question_page.svelte";
    import Homepage from "./homepage.svelte";
    import Exitquiz from "./exitquiz.svelte";
    import Scorepage from "./scorepage.svelte";
    

    // Defining variables
    export let question_visibility = 0
    let score_visibility = 0
    let i = 0 
    let questions = ['In which suburb is the Digital Traineeship Programme being run?','Which Train Station is the closest to the Cremorne Campus of Kangan Institute?','What is the capital city of Australia?']
    let correct_answers = ['Cremorne','Richmond','Canberra']
    let incorrect_answers = ['South Yarra','Bendigo','Dandenong','Frankston','Cranburne','Berwick','Beaconsfield','Adelaide','Brisbane','Perth','Paris','Melbourne','Sydney','Newcastle']
    let answerID = [0,1,2,3];
    let score = 0;
    let answeredID;
    
    //Creating answers for this question array
    $: answers =[
        correct_answers[i],
        incorrect_answers[i*3],
        incorrect_answers[(i*3+1)],
        incorrect_answers[(i*3+2)]
    ]
    //Define shuffled answers array
    let answers2;

    //Function to shuffle answers array
    function shuffle(a) {
    var y, x, z;
    for (z = a.length - 1; z > 0; z--) {
        y = Math.floor(Math.random() * (z + 1));
        x = a[z];
        a[z] = a[y];
        a[y] = x;
    }
    return a;
    }

    // Function to start quiz
    function handleStartClick() {
        question_visibility = 1
        answers2 = shuffle(answers)
    }

    // Function to leave quiz early
    function handleQuizExit() {
        score_visibility=0
        question_visibility = 0
        i=0
        score=0
        answeredID=0
    }

    //Function to move to next question and end quiz at end
    function handleClickAnswer() {
        
        
        //If statement to check for score and add score if correct
        if (answers2[answeredID] === correct_answers[i]) {
            score += 1;
        };
        //Change variables
        answeredID=0
        i += 1;
        answers =[
            correct_answers[i],
            incorrect_answers[i*3],
            incorrect_answers[(i*3+1)],
            incorrect_answers[(i*3+2)]
        ];
        //Shuffle new answers array
        answers2 = shuffle(answers);

        //End quiz if finished
        if (i >= questions.length) {
            // alert("Quiz Finished. You Scored "{score}"/"{questions.length})
            score_visibility = 1
            i = 0;
            question_visibility=0;
        };
        
    }
</script>

<style>
    /* Background color */
    body {
        background-color:  #298fbf;
    }
</style>

<!-- Shows Question page or ScorePage or Homepage -->
{#if (question_visibility===1)}
    <Question_page  on:click={handleClickAnswer} questions={questions} answers={answers2} answerID={answerID} bind:answeredID = {answeredID} i={i}/>
    <Exitquiz on:click={handleQuizExit}/>
{:else if (score_visibility===1)}
    <Scorepage score={score} questionslength={questions.length}/>
    <Exitquiz on:click={handleQuizExit}/>
{:else}
    <Homepage on:click={handleStartClick}/>
{/if} 
