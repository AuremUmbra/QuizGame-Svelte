<script>
    //Defining and exporting variables
    export let placeholder = input;
    export let value;
</script>
<style>
    /* Aligning text to center of input */
    .login {
        text-align:center;
    }
</style>

<!-- Creating text input login for username  -->
<input class="login" type="text" bind:value placeholder={placeholder}>