<script>
// Importing components
import Adminhome from "./Adminhome.svelte";
import Usermanager from "./Usermanager.svelte";
import Createquiz from "./Createquiz.svelte";
import UserMBtn from "./UserMBtn.svelte";
import CreatequizBtn from "./CreatequizBtn.svelte";

// Defining variables
let usermanager_visibility = false;
let createquiz_visibility = false;
let question_array = [];
let questionListPromise;
let userListPromise;
let question_list = [];
let answer_list = [];
let user_list = [];

// Function to go to the user manager page
function handleUserM() {
    handleGetUserList()
    usermanager_visibility = true;
}

// Function to go to create/modify quiz page
function handleCreateQuiz() {
    handleGetQuestionList();
    createquiz_visibility = true;
}

// Function to return to home 
function handleAdminHome() {
    usermanager_visibility = false;
    createquiz_visibility = false;
}

    // Function to get Question List from API
    async function QuestionList() {
        const res_list = await fetch('https://best-quiz-game.azurewebsites.net/GetAllQuiz');
        const data_list = await res_list.json();

        question_array = [];
        if (res_list.ok) {
            data_list.forEach((q) => {
                question_array = [...question_array,{"questionID": q.QuestionID, "questionDescription": q.QuestionText, "questionAnswers": q.Options}]
            })
        } else {
            return error;
        }

        question_list = [];
            question_array.forEach((q) => {
                answer_list = [
                    {"answerDescription": q.questionAnswers.A},
                    {"answerDescription": q.questionAnswers.B},
                    {"answerDescription": q.questionAnswers.C},
                    {"answerDescription": q.questionAnswers.D}
                ]
                question_list = [
                    ...question_list,
                    {"questionID": q.questionID,
                    "questionDescription": q.questionDescription, 
                    "answer1": answer_list[0].answerDescription, 
                    "answer2": answer_list[1].answerDescription, 
                    "answer3": answer_list[2].answerDescription, 
                    "answer4": answer_list[3].answerDescription}
                ]
        })
    }

    //Function to start Above function which gets question list from API
    function handleGetQuestionList() {
        questionListPromise = QuestionList();
    }

    async function GetUserList() {
        const res_user = await fetch('https://best-quiz-game.azurewebsites.net/getallusers');
        const data_user = await res_user.json();
        
        user_list = [];
        if (res_user.ok) {
            data_user.forEach((u) => {
                user_list = [...user_list,{"username":u.LoginID,"firstName":u.FirstName,"lastName":u.LastName}]
            })
        } else {
            return error;
        }
    }

    function handleGetUserList() {
        userListPromise = GetUserList()
    }
</script>

<style>

</style>


<!-- Show user manager page, create/modify quiz page or admin home page -->
{#if usermanager_visibility}
    <UserMBtn on:click={(() => handleAdminHome())} User_Manager="Home Page"/>
    <Adminhome title="User Manager"/>
    <Usermanager 
        user_list={user_list} 
        {userListPromise}
    />

{:else if createquiz_visibility}
    <UserMBtn on:click={(() => handleAdminHome())} User_Manager="Home Page"/>
    <UserMBtn on:click={(() => handleUserM())} User_Manager="User Manager"/>
    <Adminhome title="Create or modify your quiz"/>
    <Createquiz 
        {question_array} 
        {questionListPromise} 
        {question_list} 
    />


{:else}
    <UserMBtn on:click={(() => handleUserM())} User_Manager="User Manager"/>
    <Adminhome title="Admin Page"/>
    <CreatequizBtn on:click={(() => handleCreateQuiz())} createbtn="Create or Modify Quiz"/>
{/if}