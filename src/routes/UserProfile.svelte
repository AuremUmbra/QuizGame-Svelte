<script>
    import UserInput from "./UserInput.svelte";

    //Define array for the table headers
    let columns = ["Username", "First Name", "Last Name"/* , "Date Created", "Last Updated "*/];

    export let username;
    export let user;
    export let userProfilePromise;
    let UpdateUserName;
    let UpdateUserFirstName;
    let UpdateUserLastName;
    let UpdateUserPassword;
    let update_visibility = false;
    let title = "Update Your Profile";
    let UpdateBtn = "Update";
    let uUpdate;
    let uUpdate_json;

    async function GetUser() {
        const res_user = await fetch(`https://best-quiz-game.azurewebsites.net/getuser?loginID=${username}`);
        const data_user = await res_user.json();

        user = null;
        if (res_user.ok) {
            user = [{"username":data_user.LoginID,"FirstName":data_user.FirstName,"LastName":data_user.LastName}]
        } else {
            return error;
        }
    }

    function updateUserStart(u) {
        UpdateUserName = username
        UpdateUserFirstName = u.FirstName
        UpdateUserLastName = u.LastName
        update_visibility = true;
    }


    async function UpdateUserEnd() {
        uUpdate = {
            "LoginID":"",
            "FirstName":"",
            "LastName":"",
            "Password":""
        }
        uUpdate.LoginID = username;
        uUpdate.FirstName = UpdateUserFirstName;
        uUpdate.LastName = UpdateUserLastName;
        uUpdate.Password = UpdateUserPassword

        uUpdate_json = JSON.stringify(uUpdate)

        const res_update = await fetch(`https://best-quiz-game.azurewebsites.net/updateuser?loginid=${username}&FirstName=${UpdateUserFirstName}&LastName=${UpdateUserLastName}&Password=${UpdateUserPassword}`, {
            method: 'PUT',
            headers: {'Content-Type': 'application/json'},
            body: uUpdate_json
        })

        userProfilePromise = await GetUser()
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
    h2 {
        text-align: center;
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

</style>

{#await userProfilePromise}
    <h2>Loading User</h2>
{:then}
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
                <td>{row.username}</td>
                <td>{row.FirstName}</td>
                <td>{row.LastName}</td>
    <!--             <td>{row.datecreated}</td>
                <td>{row.lastupdated}</td> -->
                <!-- button to update user from table -->
                <button class="UpdateUser" on:click={() => updateUserStart(row)}>U</button>
            </tr>
        {/each}
    </table>
{:catch error}
    <h2 style="color:red">{error.message}</h2>]
{/await}