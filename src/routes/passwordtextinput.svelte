<script>
    export let placeholder = input;
    export let value;
</script>
<style>
    .login {
        text-align:center;
        border-radius: 8px;
        margin:0px;
        padding:0px 0px;
        width:220px;
        height:45px;
    }
</style>


<input class="login" type="password" bind:value placeholder={placeholder}>