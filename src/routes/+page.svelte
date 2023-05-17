<script>
    import LoginPage from "./loginpage.svelte";
    import QuizStart from "./QuizStart.svelte";
    import Loginbutton from "./loginbutton.svelte";
    import Adminpage from "./Adminpage.svelte";
    let admin_visibility = 0;
    let quiz_visibility = 0;
    let username;
    let password;
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

    //temporary username and password
    let tempusername = 'Admin';
    let temppassword = 'password';

    async function handleLogIn() {
        //----This is Temporary - Remove When Proper Login Available
        if (username === tempusername && password === temppassword) {
            username = "";
            password = "";
            admin_visibility = 1;
        }
        else if (password === temppassword) {
            username="";
            password="";
            quiz_visibility = 1;
            
        }
        else {
            alert("Incorrect username or password");
            username="";
            password="";
        }
        //----End Temporary Login

        //----Start Future Login - UnComment When Done.

        // const login = await fetch("https://dtpkanganquestionapi.azurewebsites.net/CheckUser", {
        //     method: 'POST',
        //     body: JSON.stringify({
        //         username,
        //         password
        //     })
        // })
        // const json = await login.json()
        // if (json == true && username == tempusername) {
        //     username = "";
        //     password = "";
        //     admin_visibility = 1;
        // } else if (json == true) {
        //     username="";
        //     password="";
        //     quiz_visibility = 1;
        // } else {
        //     alert("Incorrect username or password");
        //     username="";
        //     password="";
        // }

        //----End Future Login
    }

    function handleLogOut() {
        admin_visibility = 0
        quiz_visibility = 0
        question_visibility = 0
        username=""
        password=""
        score_visibility = 0;
        i = 0;
        score = 0;
        answeredID = null;
        questionPromise = null;
        answers = []; 
        answerID = [];
        question_id = null;
        questionsID = [];
    }
</script>

<style>
    .logout {
        display:flex;
        justify-content:flex-end;
    }
</style>


{#if admin_visibility === 1}
    <div class="logout">
    <Loginbutton on:click={handleLogOut} login="Log Out"/>
    </div>
    <Adminpage/>
    
{:else if quiz_visibility === 1}
    <div class="logout">
    <Loginbutton on:click={handleLogOut} login="Log Out"/>
    </div>
    <QuizStart/>
    
{:else}
    <LoginPage on:click={handleLogIn} bind:username={username} bind:password={password} />
{/if}

