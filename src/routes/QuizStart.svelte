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
    let question_id

    // Function to start quiz
    function handleStartClick() {
        question_visibility = 1;
        handleQuestionClick();
        
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
        
        //Call ScoreUpdate function when it is finished
        ScoreUpdate(question_id,answeredID)

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
        const res = await fetch("https://dtpkanganquestionapi.azurewebsites.net/Question");
        const data = await res.json();

        if (res.ok) {
            data.options.forEach((q) => {
                answers = [...answers, q.answerText];
                answerID = [...answerID,q.answerID];
            })
            question_id = data.questionID;
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

    //Function to check answers and update score
    async function ScoreUpdate(question_id,answeredID) {

        const res_check = await fetch(`https://dtpkanganquestionapi.azurewebsites.net/CheckAnswer/${question_id}/${answeredID}`); // -- Output will be Boolian Value
        const data_check = await res_check.json();
        if (res_check.ok) {
            if (data_check === true) {
            score += 1
            }
        } else {
            throw new Error(data_check);
        }

        
    }  
</script>

<style>
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
