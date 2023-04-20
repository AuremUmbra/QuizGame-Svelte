<script>
    import LoginPage from "./loginpage.svelte";
    import QuizStart from "./QuizStart.svelte";
    import Loginbutton from "./loginbutton.svelte";
    import Adminpage from "./Adminpage.svelte";
    let admin_visibility = 0;
    let quiz_visibility = 0;
    let username;
    let password;

    function handleLogIn() {
        if (username === 'Admin' && password === 'password') {
            username = "";
            password = "";
            admin_visibility = 1;
        }
        else if (password === "password") {
            username="";
            password="";
            quiz_visibility = 1;
            
        }
        else {
            alert("Incorrect username or password");
            username="";
            password="";
        }
        
    }

    function handleLogOut() {
        admin_visibility = 0
        quiz_visibility = 0
        question_visibility = 0
        username=""
        password=""
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

