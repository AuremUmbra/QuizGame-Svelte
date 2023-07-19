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
    let answers = []; 
    let answerID = [];
    let question_id;
    let questionsID = [];
    let passwordTest;
    let passwordTest_json;

    async function handleLogIn() {

        passwordTest = {
            "login_id":"",
            "password":""
        }
       
        passwordTest.login_id = username;
        passwordTest.password = password;


        passwordTest_json = JSON.stringify(passwordTest)

        const login = await fetch("https://dtpkanganquestionapi.azurewebsites.net/userlogin", {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: passwordTest_json
            })
        


        
        
        if (login.ok) {
            const login_json = await login.json()
            if (login_json) {
                quiz_visibility = 1;
            } else if (login_json && username == "Admin") {
                admin_visibility = 1;
            } else {
                alert("Incorrect username or password");
            }
        } else {
            passwordTest = null;
            passwordTest_json = null;
            username = null;
            password = null;
            alert(error);
        }

        passwordTest = null;
        passwordTest_json = null;
        username = null;
        password = null;
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
    />
    
{:else}
    <LoginPage on:click={handleLogIn} bind:username={username} bind:password={password} />
{/if}

