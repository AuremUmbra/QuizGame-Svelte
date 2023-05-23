<script>
// Importing components
import Adminhome from "./Adminhome.svelte";
import Usermanager from "./Usermanager.svelte";
import Createquiz from "./Createquiz.svelte";
import UserMBtn from "./UserMBtn.svelte";
import CreatequizBtn from "./CreatequizBtn.svelte";

// Defining variables
let usermanager_visibility = 0;
let createquiz_visibility = 0;
let question_array = [];
let questionListPromise;
let question_list = [];
let answer_list = [];
let newQuestion;
let newcorrectAnswer;
let newincorrectAnswer1;
let newincorrectAnswer2;
let newincorrectAnswer3;
let qNew;
let qNew_json

// Function to go to the user manager page
function handleUserM() {
    usermanager_visibility = 1;
}

// Function to go to create/modify quiz page
function handleCreateQuiz() {
    handleGetQuestionList();
    createquiz_visibility = 1;
}

// Function to return to home 
function handleAdminHome() {
    usermanager_visibility = 0;
    createquiz_visibility = 0;
}

// Function to get Question List from API
async function QuestionList() {
        const res_list = await fetch('https://dotnetcore78277kangan.azurewebsites.net/AllQuestions');
        const data_list = await res_list.json();

        question_array = [];
        if (res_list.ok) {
            data_list.questions.forEach((q) => {
                question_array = [...question_array,{"questionID": q.questionID, "questionDescription": q.questionDescription, "questionAnswers": q.questionAnswers}]
            })
        } else {
            return error;
        }

        question_list = [];
            question_array.forEach((q) => {
                answer_list = [];
                q.questionAnswers.forEach((a) => {
                    answer_list = [...answer_list,{"answerDescription": a.answerDescription, "isCorrect": a.isCorrect}]
                })
                answer_list.sort((a,b) => {if (a.isCorrect && b.isCorrect) return 0;
                    if (a.isCorrect) return -1;
                    if (b.isCorrect) return 1;
                })
                question_list = [...question_list,
                    {"questionDescription": q.questionDescription, 
                    "correctAnswer": answer_list[0].answerDescription, 
                    "incorrectAnswer1": answer_list[1].answerDescription, 
                    "incorrectAnswer2": answer_list[2].answerDescription, 
                    "incorrectAnswer3": answer_list[3].answerDescription}]
            })
            console.log(question_list)
    }

    //Function to start Above function which gets question list from API
    function handleGetQuestionList() {
        questionListPromise = QuestionList();
    }

    async function addNewQuestion() {
        qNew = {
            "questionDescription":"", "questionAnswers": [
            {"answerDescription":"", "isCorrect": true}, 
            {"answerDescription":"", "isCorrect": false}, 
            {"answerDescription":"", "isCorrect": false}, 
            {"answerDescription":"", "isCorrect": false}]};
        qNew.questionDescription = newQuestion;
        qNew.questionAnswers[0].answerDescription = newcorrectAnswer;
        qNew.questionAnswers[1].answerDescription = newincorrectAnswer1;
        qNew.questionAnswers[2].answerDescription = newincorrectAnswer2;
        qNew.questionAnswers[3].answerDescription = newincorrectAnswer3;
        
        console.log(qNew)
        
        qNew_json = JSON.stringify(qNew)
        console.log(qNew_json)

        const res = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/newquestion`, {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: qNew_json
        })

        handleGetQuestionList()
    }
</script>

<style>
    .create{
        margin-top: 50px;
        margin-left: 565px;
    }
</style>


<!-- Show user manager page, create/modify quiz page or admin home page -->
{#if (usermanager_visibility===1)}
    <UserMBtn on:click={handleAdminHome} User_Manager="Home Page"/>
    <Adminhome title="User Manager"/>
    <Usermanager/>

{:else if (createquiz_visibility===1)}
    <UserMBtn on:click={handleUserM} User_Manager="User Manager"/>
    <Adminhome title="Create or modify your quiz"/>
    <Createquiz 
        {question_array} 
        {questionListPromise} 
        {question_list} 
        on:click={addNewQuestion} 
        bind:newQuestion={newQuestion} 
        bind:newcorrectAnswer={newcorrectAnswer} 
        bind:newincorrectAnswer1={newincorrectAnswer1} 
        bind:newincorrectAnswer2={newincorrectAnswer2} 
        bind:newincorrectAnswer3={newincorrectAnswer3}
    />
    <div class="create">
        <CreatequizBtn on:click={handleAdminHome} createbtn="Home Page"/>
    </div>

{:else}
    <UserMBtn on:click={handleUserM} User_Manager="User Manager"/>
    <Adminhome title="Admin Page"/>
    <CreatequizBtn on:click={handleCreateQuiz} createbtn="Create or Modify Quiz"/>
{/if}