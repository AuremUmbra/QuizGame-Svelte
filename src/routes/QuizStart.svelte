<script>
    // Importing components
    import Question_page from "./question_page.svelte";
    import Homepage from "./homepage.svelte";
    import Exitquiz from "./exitquiz.svelte";
    import Scorepage from "./scorepage.svelte";
    import UserMBtn from "./UserMBtn.svelte";
    import UserProfile from "./UserProfile.svelte";


    // Defining variables
    export let question_visibility = false;
    export let score_visibility = false;
    let profile_visibility = false;
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
    let userProfilePromise;
    let user;
    export let username;
    let history;
    let history_json;

    // Function to start quiz
    function handleStartClick() {
        question_visibility = 1;
        QuizLength();
        handleQuestionClick();
        duplicatedisabled = true;
    }

    // Function to leave quiz early
    function handleQuizExit() {
        score_visibility = false;
        question_visibility = false;
        i = 0;
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
        AddHistory(username,question_id,answeredID)
        ScoreUpdate(question_id,answeredID)
        Duplicatecheck(duplicate);

        duplicate = false;
        duplicatedisabled = false;
        answeredID = 0;
        i += 1;

        //End quiz if finished
        if (i >= length) {
            score_visibility = true
            i = 0;
            question_visibility = false;
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

    async function AddHistory(login_id,question_id,answeredID) {
        history = {
            "login_id":"",
            "question_id":0,
            "option_name":""
        };

        history.login_id = login_id;
        history.question_id = question_id;
        history.option_name = answeredID;

        history_json = JSON.stringify(history);

        const res_history = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/addhistory`, {
            method:'POST',
            headers: {'Content-Type': 'application/json'},
            body: history_json
        });

        const data_history = await res_history.json();
    }
    //Function to check answers and update score
    async function ScoreUpdate(question_id,answeredID) {

        // Need to update this later to get actual result.
        const res_check = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/CheckAnswer?question_id=${question_id}&option_name=${answeredID}`) // -- Output will be Boolian Value
        const data_check = await res_check.json();

        // Ahn's API -> `https://dtpkanganquestionapi.azurewebsites.net/CheckAnswer/${question_id}/${answeredID}`
        // Team's API -> https://dotnetcore78277kangan.azurewebsites.net/CheckAnswer

        if (res_check.ok) {
            if (data_check == true) {
            score += 1
            // } else if (data_check == 'True' ) {
            //     score += 1
            // } else if (data_check == 'true') {
            //     score += 1
            }
        } else {
            throw new Error(data_check);
        }
    }  

    async function QuizLength() {
        //Need to change when API Endpoint exitsts/know what it is. 
        const res_length = await fetch('https://dotnetcore78277kangan.azurewebsites.net/AllQuestions')
        const data_length = await res_length.json();
        // Ahn's API -> 'https://dtpkanganquestionapi.azurewebsites.net/Question/GetAll'
        // Team's API -> 'https://dotnetcore78277kangan.azurewebsites.net/AllQuestions'

        if (res_length.ok) {
            if (data_length.questions.length <= maxlength) {
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

    function handleUserProfile() {
        if (profile_visibility) {
            profile_visibility = false
        } else if (profile_visibility == false) {
            profile_visibility = true
            userProfilePromise = GetUser()
        }
    }

    async function GetUser() {
        const res_user = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/getuser/${username}`);
        const data_user = await res_user.json();

        user = null;
        if (res_user.ok) {
            user = [...user,{"name":data_user.login_id,"firstname":data_user.firstname,"lastname":data_user.lastname}]
        } else {
            return error;
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
{#if question_visibility}
    {#await questionPromise}
        <h2>Loading Question</h2>
    {:then question}
        <Question_page  on:click={handleClickAnswer} questions={question.questionDescription} answers={answers} answerID={answerID} bind:answeredID = {answeredID} bind:duplicate={duplicate} {duplicatedisabled} i={i} length={length}/>
    {:catch error}
        <h2 style="color:red">{error.message}</h2>
    {/await}
    <Exitquiz on:click={handleQuizExit}/>
{:else if score_visibility}
    <Scorepage score={score} questionslength={length}/>
    <Exitquiz on:click={handleQuizExit}/>
{:else if profile_visibility}
    <UserMBtn on:click={handleUserProfile()} User_Manager="Home Page"/>
    <UserProfile {userProfilePromise} {user}/>
{:else}
    <UserMBtn on:click={handleUserProfile()} User_Manager="User Profile" />
    <Homepage on:click={handleStartClick}/>
{/if} 
