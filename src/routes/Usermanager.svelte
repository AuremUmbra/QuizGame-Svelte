<script>
    import UserInput from "./UserInput.svelte";
    import UserTable from "./UserTable.svelte";

    //Define variables for the Admin's input
    let newName;
    let newFirstName;
    let newLastName;
    let newPassword;
    let newUser_json;
    let newUser;
    export let userListPromise;
    let update_visibility = false;

    //Array to store data objects
    export let user_list = [];

    //Function to add a new row of user information
    async function addUser() {
        newUser =  {
            "LoginID": "",
            "FirstName":"",
            "LastName":"",
            "Password":""
        }
        newUser.LoginID = newName;
        newUser.FirstName = newFirstName;
        newUser.LastName = newLastName;
        newUser.Password = newPassword

        newName = "";
        newFirstName = "";
        newLastName = "";
        newPassword = "";


        newUser_json = JSON.stringify(newUser)

        const res_nUser = await fetch(`https://best-quiz-game.azurewebsites.net/adduser?LoginID=${newUser.LoginID}&FirstName=${newUser.FirstName}&LastName=${newUser.LastName}&Password=${newUser.Password}`, {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: newUser_json    
        });

        const data_nUser = await res_nUser.json();

        userListPromise = GetUserList();
    };

    function handleAddUser() {
        userListPromise = addUser()
    }

    async function GetUserList() {
        const res_user = await fetch('https://best-quiz-game.azurewebsites.net/getallusers');
        const data_user = await res_user.json();
        
        user_list = [];
        if (res_user.ok) {
            data_user.forEach((u) => {
                user_list = [...user_list,{"username":u.LoginID,"firstName":u.FirstName,"lastName":u.LastName}]
            })
        } else {
            return error;
        }
    }

</script>

<style>
    h2 {
        text-align: center;
        font-size: 28px;
        font: sans-serif;
        font-weight: bold;
    }
</style>

<br>
{#if ! update_visibility}
    <UserInput 
        bind:newName = {newName} 
        bind:firstname = {newFirstName}
        bind:lastname = {newLastName}
        bind:password = {newPassword}
        on:click={(() => handleAddUser())} 
    />
{/if}

{#await userListPromise}
    <h2>Loading User List</h2>
{:then}
    <UserTable 
        user_list = {user_list} 
        bind:update_visibility = {update_visibility}
        bind:userListPromise = {userListPromise}
    />

{:catch error}
    <h2 style="color:red">{error.message}</h2>
{/await}


