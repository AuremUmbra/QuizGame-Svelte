<script>
    import Loginbutton from "./loginbutton.svelte";
    import Logintextinput from "./logintextinput.svelte";
    import Passwordtextinput from "./passwordtextinput.svelte";
    import Title from "./title.svelte";
    import Logos from "./Logos.svelte"
    export let username;
    export let password;
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
    .login {
        display: flex;
        justify-content: center;
        margin-top: 80px;
    }

    body {
        background-color:  #298fbf;
    }
</style>

<Title/>
<Logos/>
<body>
    <form class = "login">
        <Logintextinput placeholder="Username" bind:value={username}/>
        <Passwordtextinput placeholder="Password" bind:value={password}/>
        <Loginbutton on:click login="Log In"/><!--={handleLogIn}-->
    </form>
</body>
