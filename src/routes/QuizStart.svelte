<script>
    // Importing components
    import Question_page from "./question_page.svelte";
    import Homepage from "./homepage.svelte";
    import Exitquiz from "./exitquiz.svelte";
    import Scorepage from "./scorepage.svelte";
    

    // Defining variables
    export let question_visibility = 0;
    export let score_visibility = 0;
    export let i = 0;
    export let score = 0;
    export let answeredID;
    export let questionPromise;
    export let answers = []; 
    export let answerID = [];
    export let question_id;
    export let questionsID = [];
    let length = 10;
    let maxlength = 10;
    let duplicate;
    let duplicatedisabled = true;

    // Function to start quiz
    function handleStartClick() {
        question_visibility = 1;
        QuizLength();
        handleQuestionClick();
        duplicatedisabled = true;
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
        duplicate = false;
    }

    //Function to move to next question and end quiz at end
    function handleClickAnswer() {
        answers = [];
        answerID = [];
        
        //Call ScoreUpdate function when it is finished
        ScoreUpdate(question_id,answeredID)
        Duplicatecheck(duplicate);

        duplicate = false;
        duplicatedisabled = false;
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
        const res = await fetch("https://dotnetcore78277kangan.azurewebsites.net/question", {
            //mode: 'no-cors'
        });
        const data = await res.json();

        // Ahn's API -> "https://dtpkanganquestionapi.azurewebsites.net/Question"
        // Team's API -> https://dotnetcore78277kangan.azurewebsites.net/Question

        if (res.ok) {
            if (questionsID.includes(data.questionID)) {
                handleQuestionClick()
            } else {
                data.questionAnswers.forEach((q) => { //questionAnswers for Team / options for Ahn's API
                    answers = [...answers, q.answerDescription]; //Text for Ahn's API, Description for Team's API
                    answerID = [...answerID,q.answerID];
                })
                questionsID = [...questionsID,data.questionID];
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
        // Need to update this later to get actual result.
        const res_check = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/CheckAnswer`) // -- Output will be Boolian Value
        const data_check = await res_check.text();

        // Ahn's API -> `https://dtpkanganquestionapi.azurewebsites.net/CheckAnswer/${question_id}/${answeredID}`
        // Team's API -> https://dotnetcore78277kangan.azurewebsites.net/CheckAnswer

        if (res_check.ok) {
            if (data_check == true) {
            score += 1
            } else if (data_check == 'True' ) {
                score += 1
            } else if (data_check == 'true') {
                score += 1
            }
        } else {
            throw new Error(data_check);
        }
    }  

    async function QuizLength() {
        //Need to change when API Endpoint exitsts/know what it is. 
        const res_length = await fetch('https://dotnetcore78277kangan.azurewebsites.net/AllQuestions')
        const data_length = await res_length.json();
        console.log(data_length)
        console.log(data_length.questions.length)
        // Ahn's API -> 'https://dtpkanganquestionapi.azurewebsites.net/Question/GetAll'
        // Team's API -> 

        if (res_length.ok) {
            if (data_length.questions.length <= 10) {
                length = data_length.questions.length
            } else {
                length = maxlength
            }
            
        }
    }

    function Duplicatecheck(duplicate) {
        if (duplicate == true) {
            //Do things
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
        <Question_page  on:click={handleClickAnswer} questions={question.questionDescription} answers={answers} answerID={answerID} bind:answeredID = {answeredID} bind:duplicate={duplicate} {duplicatedisabled} i={i} length={length}/>
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
