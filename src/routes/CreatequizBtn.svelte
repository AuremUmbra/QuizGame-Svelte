<script>
    export let createbtn;
    export let createBtnDisabled = false;
</script>

<style>
button {
        margin: 10px auto;
        display: block;
        padding: 5px;
        width: 200px;
        height: 75px;
        font-size: 1.1rem;
        border-radius: 0.5rem;
        border: solid;
        border-width: thin;
        background-color:  rgb(255, 187, 0);
    }

    button:hover {
        background-color:  rgb(255, 170, 0);
    }

    button:active {
        color: white;
        background-color:  rgb(255, 155, 0);
    }

    button:disabled {
        color: black;
        background-color: gray;
    }
</style>

<button disabled={createBtnDisabled} on:click>{createbtn}</button>