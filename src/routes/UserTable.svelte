<script>
    import UserInput from "./UserInput.svelte";
    
    //Define array for the table headers
    let columns = ["Name", "User ID", "First Name", "Last Name"/* , "Date Created", "Last Updated "*/];

    //Array to store data objects
    export let user_list = [];
    export let userListPromise;

    let UpdateUserName;
    let UpdateUserFirstName;
    let UpdateUserLastName;
    let UpdateUserPassword;

    let userStatusButton = A;

    export let update_visibility = false;

    let uUpdate;
    let uUpdate_json;
    let title = "Update User";
    let UpdateBtn = "Update";
    
    //Function to delete a row of user information from the table
    async function deleteUser(uID) {
        const res = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/deleteuser/${uID}`, {
            method: 'DELETE',
        })


        userListPromise = GetUserList()
    };

    async function GetUserList() {
        const res_user = await fetch('https://dotnetcore78277kangan.azurewebsites.net/allusers');
        const data_user = await res_user.json();

        user_list = [];
        if (res_user.ok) {
            data_user.users.forEach((u) => {
                user_list = [...user_list,{"name":u.login_id,"firstname":u.firstname,"lastname":u.lastname}]
            })
        } else {
            return error;
        }
    }

    function updateUserStart(u) {
        UpdateUserName = u.name
        UpdateUserFirstName = u.firstname
        UpdateUserLastName = u.lastname
        update_visibility = true;
    }

    async function UpdateUserEnd() {
        uUpdate = {
            "login_id":"",
            "firstname":"",
            "lastname":"",
            "password":""
        }
        uUpdate.login_id = UpdateUserName;
        uUpdate.firstname = UpdateUserFirstName;
        uUpdate.lastname = UpdateUserLastName;
        uUpdate.password = UpdateUserPassword

        uUpdate_json = JSON.stringify(uUpdate)

        const res_update = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/updateuser/${UpdateUserName}`, {
            method: 'PUT',
            headers: {'Content-Type': 'application/json'},
            body: uUpdate_json
        })

        userListPromise = GetUserList()
        update_visibility = false;
        uUpdate = null;
        uUpdate_json = null;
        UpdateUserName = null;
        UpdateUserFirstName = null;
        UpdateUserLastName = null;
        UpdateUserPassword = null;
    }

    async function toggleUserStatus(u) {
        
        const res_userStatus = await fetch(`https://dtpkanganquestionapi.azurewebsites.net/activateuserstatus?login_id=${u.name}`,{
            method: 'PUT',
            headers: {'Content-Type': 'application/json'}
        })
        userListPromise = GetUserList()
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
</style>



{#if update_visibility}
    <UserInput
        bind:newName = {UpdateUserName}
        bind:firstname = {UpdateUserFirstName}
        bind:lastname = {UpdateUserLastName}
        {title}
        effect = {UpdateBtn}
        usernameChangeDisabled = {true}
        on:click={(() => UpdateUserEnd())}
    />
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
            <td>{row.name}</td>
            <td>{row.userID}</td>
            <td>{row.firstname}</td>
            <td>{row.lastname}</td>
<!--             <td>{row.datecreated}</td>
            <td>{row.lastupdated}</td> -->
            <!-- button to delete user from table -->
            <button class="UpdateUser" on:click={() => updateUserStart(row)}>U</button>
            <button class="ActivateUser" on:click={() => toggleUserStatus(row.userID)}>{userStatusButton}</button>
            <button class="DeleteUser" on:click={() => deleteUser(row.userID)}>X</button>
        </tr>
    {/each}
</table>