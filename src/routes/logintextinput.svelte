<script>
    //Defining and exporting variables
    export let placeholder = input;
    export let value;
</script>
<style>
    /* Aligning text to center of input */
    .login {
        text-align:center;
        border-radius: 8px;
        margin:0px;
        padding:0px 0px;
        width:220px;
        height:45px;
    }
</style>

<!-- Creating text input login for username  -->
<input class="login" type="text" bind:value placeholder={placeholder}>