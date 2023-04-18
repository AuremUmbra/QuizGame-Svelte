<script>
    // Importing components
    import Question_page from "./question_page.svelte";
    import Homepage from "./homepage.svelte";
    import Exitquiz from "./exitquiz.svelte";

    // Defining variables
    export let question_visibility = 0
    let i = 0 
    let questions = ['In which suburb is the Digital Traineeship Programme being run?','Which Train Station is the closest to the Cremorne Campus of Kangan Institute?','What is the capital city of Australia?']
    let correct_answers = ['Cremorne','Richmond','Canberra']
    let incorrect_answers = ['South Yarra','Bendigo','Dandenong','Frankston','Cranburne','Berwick','Beaconsfield','Adelaide','Brisbane','Perth','Paris','Melbourne','Sydney','Newcastle']

    // import questions from "./question_page.svelte";

    // Function to start quiz
    function handleStartClick() {
        question_visibility = 1
    }

    // Function to leave quiz early
    function handleQuizExit() {
        question_visibility = 0
        i=0
    }

    //Function to move to next question and end quiz at end
    function handleClickAnswer() {
        i += 1
        if (i >= questions.length) {
            alert("Quiz Finished")
            i = 0
            question_visibility=0
        }
        
    }
</script>

<style>
    /* Background color */
    :global(body) {
        background-color:  #298fbf;
    }
</style>

<!-- Shows Question page or Homepage -->
{#if (question_visibility===1)}
    <Question_page on:click={handleClickAnswer} questions={questions} correct_answers={correct_answers} incorrect_answers={incorrect_answers} i={i}/>
    <Exitquiz on:click={handleQuizExit}/>
{:else}
    <Homepage on:click={handleStartClick}/>
{/if} 
