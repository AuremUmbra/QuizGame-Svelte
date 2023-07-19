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
let newQuestionID = 0;

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
        const res_list = await fetch('https://dotnetcore78277kangan.azurewebsites.net/getquiz');
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
                    {"questionID": q.questionID,
                    "questionDescription": q.questionDescription, 
                    "correctAnswer": answer_list[0].answerDescription, 
                    "incorrectAnswer1": answer_list[1].answerDescription, 
                    "incorrectAnswer2": answer_list[2].answerDescription, 
                    "incorrectAnswer3": answer_list[3].answerDescription}]
                 if (newQuestionID <= q.questionID) {
                     newQuestionID = q.questionID + 1
                }
            })
       
    }

    //Function to start Above function which gets question list from API
    function handleGetQuestionList() {
        questionListPromise = QuestionList();
    }

    async function GetUserList() {
        const res_user = await fetch('https://dotnetcore78277kangan.azurewebsites.net/users');
        const data_user = await res_user.json();

        user_list = [];
        if (res_user.ok) {
            data_user.users.forEach((u) => {
                user_list = [...user_list,{"name":u.name,"userID":u.userId}]
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
    <UserMBtn on:click={handleAdminHome} User_Manager="Home Page"/>
    <Adminhome title="User Manager"/>
    <Usermanager 
        user_list={user_list} 
        {userListPromise}
    />

{:else if createquiz_visibility}
    <UserMBtn on:click={handleAdminHome} User_Manager="Home Page"/>
    <UserMBtn on:click={handleUserM} User_Manager="User Manager"/>
    <Adminhome title="Create or modify your quiz"/>
    <Createquiz 
        {question_array} 
        {questionListPromise} 
        {question_list} 
        newID={newQuestionID}
    />


{:else}
    <UserMBtn on:click={handleUserM} User_Manager="User Manager"/>
    <Adminhome title="Admin Page"/>
    <CreatequizBtn on:click={handleCreateQuiz} createbtn="Create or Modify Quiz"/>
{/if}