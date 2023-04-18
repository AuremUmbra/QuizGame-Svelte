<script>
    export let placeholder = input;
    export let value;
</script>
<style>
    .login {
        text-align:center;
    }
</style>


<input class="login" type="password" bind:value placeholder={placeholder}>