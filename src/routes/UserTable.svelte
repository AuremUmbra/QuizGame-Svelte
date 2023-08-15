<script>
    import UserInput from "./UserInput.svelte";
    
    //Define array for the table headers
    let columns = ["Username", "First Name", "Last Name"/* , "Date Created", "Last Updated "*/];

    //Array to store data objects
    export let user_list = [];
    export let userListPromise;

    let UpdateUserName;
    let UpdateUserFirstName;
    let UpdateUserLastName;
    let UpdateUserPassword;

    let userStatusButton = "A";

    export let update_visibility = false;

    let uUpdate;
    let uUpdate_json;
    let title = "Update User";
    let UpdateBtn = "Update";
    
    //Function to delete a row of user information from the table
    /* async function deleteUser(uID) {
        const res = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/deleteuser/${uID}`, {
            method: 'DELETE',
        })


        userListPromise = GetUserList()
    }; */

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

    function updateUserStart(u) {
        UpdateUserName = u.username
        UpdateUserFirstName = u.firstName
        UpdateUserLastName = u.lastName
        update_visibility = true;
    }

    async function UpdateUserEnd() {
        uUpdate = {
            "LoginID":"",
            "FirstName":"",
            "LastName":"",
            "Password":""
        }
        uUpdate.LoginID = UpdateUserName;
        uUpdate.FirstName = UpdateUserFirstName;
        uUpdate.LastName = UpdateUserLastName;
        uUpdate.Password = UpdateUserPassword

        uUpdate_json = JSON.stringify(uUpdate)

        const res_update = await fetch(`https://best-quiz-game.azurewebsites.net/updateuser?loginid=${UpdateUserName}&FirstName=${UpdateUserFirstName}&LastName=${UpdateUserLastName}&Password=${UpdateUserPassword}`, {
            method: 'PUT',
            headers: {'Content-Type': 'application/json'},
            body: uUpdate_json
        })

        userListPromise = await GetUserList()
        update_visibility = false;
        uUpdate = null;
        uUpdate_json = null;
        UpdateUserName = null;
        UpdateUserFirstName = null;
        UpdateUserLastName = null;
        UpdateUserPassword = null;
    }

    async function activateUser(u) {
        const res_userStatus = await fetch(`https://best-quiz-game.azurewebsites.net/activateuserstatus?login_id=${u}`,{
            method: 'PUT',
            headers: {'Content-Type': 'application/json'}
        })
        userListPromise = GetUserList()
        alert("User Activated")
    }


    async function deactivateUser(u) {
        const res_userStatus = await fetch(`https://best-quiz-game.azurewebsites.net/deactivateuserstatus?login_id=${u}`,{
            method: 'PUT',
            headers: {'Content-Type': 'application/json'}
        })
        userListPromise = GetUserList()
        alert("User Deactivated")
    }

    function cancelUpdate() {
        update_visibility = false;
    }
</script>

<style>
    .DeleteUser {
        /*Button background color*/
        background-color: black;
        /*Button text color*/
        color: white;
        /*Button space around text*/
        padding: 10px 25px;
        margin-top: 5px;
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
   
    tr td {
        background: #eee;
        vertical-align: middle;

    }

    table, th, td {
        border: 2px solid;
        border-collapse: collapse;
        margin: auto;
    }

    th, td {
        height: 50px;
    }

    table {
        padding: 15px;
        margin-top: 60px;
        margin-bottom: 30px;
        max-width: 1500px;
    }

    .UpdateUser {
        /*Button background color*/
        background-color: green;
        /*Button text color*/
        color: black;
        /*Button space around text*/
        padding: 10px 25px;
        margin-top: 5px;
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

    .ActivateUser {
        /*Button background color*/
        background-color: yellow;
        /*Button text color*/
        color: black;
        /*Button space around text*/
        padding: 10px 25px;
        margin-top: 5px;
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

    .CancelUpdate {
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
    .CancelUpdate:hover {
        background-color: #d68e10;
    }

    .CancelUpdate:disabled {
        background-color: grey;

    }

    .CancelUpdateDiv {
        display:flex;
        justify-content: center;
    }
</style>



{#if update_visibility}
    
        <UserInput
            bind:newName = {UpdateUserName}
            bind:firstname = {UpdateUserFirstName}
            bind:lastname = {UpdateUserLastName}
            bind:password = {UpdateUserPassword}
            {title}
            effect = {UpdateBtn}
            usernameChangeDisabled = {true}
            on:click={(() => UpdateUserEnd())}
        />
    <div class="CancelUpdateDiv">
        <button class="CancelUpdate" on:click={(() => cancelUpdate())}>Cancel</button>
    </div>
{/if}


<!-- The table holding each user's information, including the Admin's information -->
<table>
    <!-- The column headers -->
    <tr>
        {#each columns as column}
            <th>{column}</th>
        {/each}
    </tr>
    <!-- The table data -->
    {#each user_list as row}
        <tr>
            <td>{row.username}</td>
            <td>{row.firstName}</td>
            <td>{row.lastName}</td>
<!--             <td>{row.datecreated}</td>
            <td>{row.lastupdated}</td> -->
            <!-- button to delete user from table -->
            <button class="UpdateUser" on:click={() => updateUserStart(row)}>U</button>
            <button class="ActivateUser" on:click={(() => activateUser(row.username))}>{userStatusButton}</button>
            <button class="DeleteUser" on:click={(() => deactivateUser(row.username))}>D</button>
        </tr>
    {/each}
</table>