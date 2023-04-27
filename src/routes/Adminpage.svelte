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

// Function to go to the user manager page
function handleUserM() {
    usermanager_visibility = 1;
}

// Function to go to create/modify quiz page
function handleCreateQuiz() {
    createquiz_visibility = 1;
}

function handleAdminHome() {
    usermanager_visibility = 0;
    createquiz_visibility = 0;
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
    <Createquiz/>
    <div class="create">
        <CreatequizBtn on:click={handleAdminHome} createbtn="Home Page"/>
    </div>

{:else}
    <UserMBtn on:click={handleUserM} User_Manager="User Manager"/>
    <Adminhome title="Admin Page"/>
    <CreatequizBtn on:click={handleCreateQuiz} createbtn="Create or Modify Quiz"/>
{/if}