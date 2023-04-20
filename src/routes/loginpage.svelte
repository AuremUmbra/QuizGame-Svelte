<script>
    // Importing components
    import Loginbutton from "./loginbutton.svelte";
    import Logintextinput from "./logintextinput.svelte";
    import Passwordtextinput from "./passwordtextinput.svelte";
    import Title from "./title.svelte";
    import Logos from "./Logos.svelte";
    // Defining and exporting variables to be defined in importing components
    export let username;
    export let password;

    // Moved to different component

    // export let admin_visibility = 0;
    // export let quiz_visibility = 0;
    // function handleLogIn() {
    //     if (username === 'Admin' && password === 'password') {
    //         admin_visibility = 1
    //     }
    //     else if (password === "password") {
    //         quiz_visibility = 1
    //     }
    //     else {
    //         alert("Incorrect username or password")
    //     }
        
    // }
</script>

<style>
    /* Centering and margining login area */
    .login {
        display: flex;
        justify-content: center;
        margin-top: 80px;
    }
    /* Giving background color for login page */
    body {
        background-color:  #298fbf;
    }
</style>

<!-- Getting Title and Logos -->
<Title/>
<Logos/>
<body>
    <!-- Creating form for password, username and login button -->
    <form class = "login">
        <Logintextinput placeholder="Username" bind:value={username}/>
        <Passwordtextinput placeholder="Password" bind:value={password}/>
        <Loginbutton on:click login="Log In"/><!--={handleLogIn}-->
    </form>
</body>
