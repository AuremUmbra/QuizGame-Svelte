<script>
    // Importing components
    import Question_page from "./question_page.svelte";
    import Homepage from "./homepage.svelte";
    import Exitquiz from "./exitquiz.svelte";
    import Scorepage from "./scorepage.svelte";
    

    // Defining variables
    let question_visibility = 0
    let score_visibility = 0
    let i = 0 
    let score = 0;
    let answeredID;
    let questionPromise;
    let answers = []; 
    let answerID = [];
    let questions = [1,2,3];

    // Function to start quiz
    function handleStartClick() {
        question_visibility = 1;
        handleQuestionClick();
        // answers2 = shuffle(answers)
    }

    // Function to leave quiz early
    function handleQuizExit() {
        score_visibility=0;
        question_visibility = 0;
        i=0;
        score=0;
        answers = [];
        answerID = [];
        answeredID = 0;
    }

    //Function to move to next question and end quiz at end
    function handleClickAnswer() {
        answers = [];
        answerID = [];
        
        //If statement to check for score and add score if correct
        // if (answers2[answeredID] === correct_answers[i]) {
        //     score += 1;
        // };
        //Change variables

        //Remove Later if get whole question list at beginning 
        handleQuestionClick();


        answeredID = 0;

        i += 1;

        //End quiz if finished
        if (i >= questions.length) {
            score_visibility = 1
            i = 0;
            question_visibility=0;
        };
        
    }

    //Function to get questions and answers from API
    async function QuestionGet() {
        const res = await fetch("https://dtpkanganwebapi.azurewebsites.net/Question");
        const data = await res.json();

        if (res.ok) {
            data.options.forEach((q) => {
                answers = [...answers, q.answerText];
                answerID = [...answerID,q.answerID];
            })
            console.log(data)
            return data;
        } else {
            throw new Error(data);
        }
    }


    //Function to call QuestionGet function when needed
    function handleQuestionClick() {
        questionPromise = QuestionGet();
        //console.log(questionPromise)
        
    }

</script>

<style>
    /* Background color -- Currently Unused*/
    /* body {
        background-color:  #298fbf;
    } */

    
    h2 {
        text-align: center;
        font-size: 28px;
        font: sans-serif;
        font-weight: bold;
    }
</style>

<!-- Shows Question page or ScorePage or Homepage -->
{#if (question_visibility===1)}
    {#await questionPromise}
        <h2>Loading Question</h2>
    {:then question}
        <Question_page  on:click={handleClickAnswer} questions={question.questionText} answers={answers} answerID={answerID} bind:answeredID = {answeredID} i={i}/>
    {:catch error}
        <p style="color:red">{error.message}</p>
    {/await}
    <Exitquiz on:click={handleQuizExit}/>
{:else if (score_visibility===1)}
    <Scorepage score={score} questionslength={questions.length}/>
    <Exitquiz on:click={handleQuizExit}/>
{:else}
    <Homepage on:click={handleStartClick}/>
{/if} 
