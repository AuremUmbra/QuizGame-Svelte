<script>
    //Define variables for the Admin's input
    export let newName;
    export let title = "New User"
    // export let newEmail;

    function NewUserDisabled(Name) {
        if (Name == null || Name == "") {
            return true
        } 
        return false;
    }
</script>

<style>
    input {
        padding: 15px 32px;
    }
    .NewUserInput {
        margin-left: auto;
        margin-right: auto;
        text-align: center;
    }
    .Add {
        /*Button background color*/
        background-color: #b88103;
        /*Button text color*/
        color: white;
        /*Button space around text*/
        padding: 15px 32px;
        /*Centering text inside button*/
        text-align: center;
        /*Button rounding*/
        border-radius: 8px;
        /*Button border*/
        border: none;
        text-decoration:none;
        display: inline-block;
        /*Text size*/
        font-size: 14px;
    }
    .Add:hover {
        background-color: #d68e10;
    }

    .Add:disabled {
        background-color: grey;

    }

    h2 {
        text-align:center;
        font-size: 2em;
        font:sans-serif;
    }
</style>

<h2>{title}</h2>
<!-- New user input -->
<div class="NewUserInput">
    <input placeholder="Name" type="text" bind:value={newName} /> 
    <!-- <input placeholder="Email" type="text" bind:value={newEmail} />  -->

<!-- Button to add new user row to table -->
    <button disabled={NewUserDisabled(newName)} class="Add" on:click>Add</button>
</div>

