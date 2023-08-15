<script>
    //Define variables for the Admin's input
    export let newName;
    export let firstname;
    export let lastname;
    export let password;
    export let usernameChangeDisabled = true;

    export let title = "New User";
    export let effect = "Add";
    // export let newEmail;

    function NewUserDisabled(Name,FName,LName,Password) {
        if (Name == null || Name == "") {
            return true;
        } 
        if (FName == null || FName == "") {
            return true;
        }
        if (LName == null || LName == "") {
            return true;
        }
        if (Password == null || Password == "") {
            return true;
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

    input:disabled {
        background-color: lightgray;
    }
</style>

<h2>{title}</h2>
<!-- New user input -->
<div class="NewUserInput">
    <input placeholder="User Name" disabled={usernameChangeDisabled} type="text" bind:value={newName} /> 
    <input placeholder="First Name" type="text" bind:value={firstname} />
    <input placeholder="Last Name" type="text" bind:value={lastname} />
    <input placeholder="Password" type="text" bind:value={password} />
    <!-- <input placeholder="Email" type="text" bind:value={newEmail} />  -->

<!-- Button to add new user row to table -->
    <button disabled={NewUserDisabled(newName,firstname,lastname,password)} class="Add" on:click>{effect}</button>
</div>

