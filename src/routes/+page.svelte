<script>
    import LoginPage from "./loginpage.svelte";
    import QuizStart from "./QuizStart.svelte";
    import Loginbutton from "./loginbutton.svelte";
    import Adminpage from "./Adminpage.svelte";
    let admin_visibility = false;
    let quiz_visibility = false;
    let username;
    let password;
    let question_visibility = false;
    let score_visibility = false;
    let i = 0;
    let score = 0;
    let answeredID;
    let questionPromise;
    let loginPromise;
    let answers = []; 
    let answerID = [1,2,3,4];
    let question_id;
    let questionsID = [];
    let passwordTest;
    let passwordTest_json;

    function handleLogIn() {
        loginPromise = LogIn()
    }

    async function LogIn() {

        passwordTest = {
            "Login_Id":"",
            "Password":""
        }
       
        passwordTest.Login_Id = username;
        passwordTest.Password = password;


        passwordTest_json = JSON.stringify(passwordTest)

        const login = await fetch(`https://best-quiz-game.azurewebsites.net/userlogin?Login_Id=${passwordTest.Login_Id}&Password=${passwordTest.Password}`       /*Old Version "https://dtpkanganquestionapi.azurewebsites.net/userlogin" */, {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: passwordTest_json
            })
        
            console.log(login)

        
        passwordTest = null;
        passwordTest_json = null;
        password = null;

        if (login.ok) {
            const login_json = await login.json()
            console.log(login_json)
            if (login_json == 'Login Successful.' && username == 'admin') {
                admin_visibility = true;
            } else if (login_json == "Login Successful.") {
                quiz_visibility = true;
            } else {
                alert("Incorrect username or password");
            }
        } else {
            return error;          
        }

        
        
    }

    function handleLogOut() {
        admin_visibility = false
        quiz_visibility = false
        question_visibility = false
        username=""
        password=""
        score_visibility = false;
        i = 0;
        score = 0;
        answeredID = null;
        questionPromise = null;
        answers = []; 
        question_id = null;
        questionsID = [];
    }
</script>

<style>
    .logout {
        display:flex;
        justify-content:flex-end;
    }
    h2 {
        text-align: center;
        font-size: 28px;
        font: sans-serif;
        font-weight: bold;
    }
</style>

{#await loginPromise}
    <h2>Logging In</h2>
{:then}
    {#if admin_visibility}
        <div class="logout">
        <Loginbutton on:click={handleLogOut} login="Log Out"/>
        </div>
        <Adminpage/>
        
    {:else if quiz_visibility}
        <div class="logout">
        <Loginbutton on:click={handleLogOut} login="Log Out"/>
        </div>
        <QuizStart
            {question_visibility}
            {score_visibility}
            {i}
            {score}
            {answeredID}
            {questionPromise}
            {answers}
            {answerID}
            {question_id}
            {questionsID}
            {username}
        />
        
    {:else}
        <LoginPage on:click={handleLogIn} bind:username={username} bind:password={password} />
    {/if}
{:catch error}
    <h2 style="color:red">{error.message}</h2>
{/await}
