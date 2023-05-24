<script>
    export let placeholder = input;
    export let value;
</script>
<style>
    .login {
        text-align:center;
        border-radius: 8px;
    }
</style>


<input class="login" type="password" bind:value placeholder={placeholder}>