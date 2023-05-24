<script>
    // Allows Button to display different text from other components
    export let login;
</script>

<style>
    .login {
        /*Button background color*/
        background-color: black;
        /*Button text color*/
        color: white;
        /*Centering text inside button*/
        text-align: center;
        /*Button rounding*/
        border-radius: 8px;
        /*Button border*/
        border: 0px solid black;
        text-decoration:none;
        display: inline-block;
        /*Text size*/
        font-size: 14px;
        margin: 0px;
        /*Button space around text*/
        padding: 0px 0px;
        width: 120px;
        height: 45px;
    }
</style>

<!-- Create button that has a click event and text that is chosen by the importing component  -->
<button class="login button" on:click>{login}</button>