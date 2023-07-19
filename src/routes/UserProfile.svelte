<script>
    import UserInput from "./UserInput.svelte";

    //Define array for the table headers
    let columns = ["Name", "User ID", "First Name", "Last Name"/* , "Date Created", "Last Updated "*/];

    let user;
    let UpdateUserName;
    let UpdateUserFirstName;
    let UpdateUserLastName;
    let UpdateUserPassword;

    async function GetUser() {
        const res_user = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/getuser/${username}`);
        const data_user = await res_user.json();

        user = null;
        if (res_user.ok) {
            user_list = [...user_list,{"name":data_user.login_id,"firstname":data_user.firstname,"lastname":data_user.lastname}]
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

        const res_update = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/updateuser/${UpdateUserID}`, {
            method: 'PUT',
            headers: {'Content-Type': 'application/json'},
            body: uUpdate_json
        })

        GetUser()
        update_visibility = false;
        uUpdate = null;
        uUpdate_json = null;
        UpdateUserName = null;
        UpdateUserFirstName = null;
        UpdateUserLastName = null;
        UpdateUserPassword = null;
    }


</script>

<style>
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



<table>
    <!-- The column headers -->
    <tr>
        {#each columns as column}
            <th>{column}</th>
        {/each}
    </tr>
    <!-- The table data -->
    {#each user as row}
        <tr>
            <td>{row.name}</td>
            <td>{row.userID}</td>
            <td>{row.firstname}</td>
            <td>{row.lastname}</td>
<!--             <td>{row.datecreated}</td>
            <td>{row.lastupdated}</td> -->
            <!-- button to update user from table -->
            <button class="UpdateUser" on:click={() => updateUserStart(row)}>U</button>
        </tr>
    {/each}
</table>