<script>
    // Importing components
    import Question_page from "./question_page.svelte";
    import Homepage from "./homepage.svelte";
    import Exitquiz from "./exitquiz.svelte";
    import Scorepage from "./scorepage.svelte";
    

    // Defining variables
    let question_visibility = 0;
    let score_visibility = 0;
    let i = 0;
    let score = 0;
    let answeredID;
    let questionPromise;
    let answers = []; 
    let answerID = [];
    let question_id;
    let questionsID = [];
    let length = 10;
    let maxlength = 10;

    // Function to start quiz
    function handleStartClick() {
        question_visibility = 1;
        QuizLength();
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
        questionsID = [];
    }

    //Function to move to next question and end quiz at end
    function handleClickAnswer() {
        answers = [];
        answerID = [];
        
        //Call ScoreUpdate function when it is finished
        ScoreUpdate(question_id,answeredID)
        
        answeredID = 0;
        i += 1;

        //End quiz if finished
        if (i >= length) {
            score_visibility = 1
            i = 0;
            question_visibility=0;
        } else {
            
            handleQuestionClick();   
        }     
    }

    //Function to get questions and answers from API
    async function QuestionGet() {
        const res = await fetch("https://dtpkanganquestionapi.azurewebsites.net/Question");
        const data = await res.json();

        if (res.ok) {
            if (questionsID.includes(data.questionID)) {
                handleQuestionClick()
            } else {
                data.options.forEach((q) => {
                    answers = [...answers, q.answerText];
                    answerID = [...answerID,q.answerID];
                })
                questionsID = [...questionsID,data.questionID];
                console.log(questionsID)
            }
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

    async function QuizLength() {
        const res_length = await fetch('https://dtpkanganquestionapi.azurewebsites.net/Question/GetAll')
        const data_length = await res_length.json();
        if (res_length.ok) {
            if (data_length.length <= 10) {
                length = data_length.length
            } else {
                length = maxlength
            }
            
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
        <Question_page  on:click={handleClickAnswer} questions={question.questionText} answers={answers} answerID={answerID} bind:answeredID = {answeredID} i={i} length={length}/>
    {:catch error}
        <p style="color:red">{error.message}</p>
    {/await}
    <Exitquiz on:click={handleQuizExit}/>
{:else if (score_visibility===1)}
    <Scorepage score={score} questionslength={length}/>
    <Exitquiz on:click={handleQuizExit}/>
{:else}
    <Homepage on:click={handleStartClick}/>
{/if} 
