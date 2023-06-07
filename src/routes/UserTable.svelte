<script>
    //Define array for the table headers
    let columns = ["Name", "User ID", "Date Created", "Last Updated"];

    //Array to store data objects
    export let data = [];

    //Function to delete a row of user information from the table (not the database)


    async function deleteRow(uID) {
        const res = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/deletequestion/${uID}`, {
            method: 'DELETE',
        })


        GetUserList()
    };

    async function GetUserList() {
        const res_user = await fetch('https://dotnetcore78277kangan.azurewebsites.net/allusers');
        const data_user = await res_user.json();

        user_list = [];
        if (res_user.ok) {
            data_user.users.forEach((u) => {
                user_list = [...user_list,{"name":u.name,"userID":u.userId}]
            })
        } else {
            return error;
        }
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
</style>


<!-- The table holding each user's information, including the Admin's information -->
<table>
    <!-- The column headers -->
    <tr>
        {#each columns as column}
            <th>{column}</th>
        {/each}
    </tr>
    <!-- The table data -->
    {#each data as row}
        <tr>
            <td>{row.name}</td>
            <td>{row.userID}</td>
            <td>{row.datecreated}</td>
            <td>{row.lastupdated}</td>
            <!-- button to delete user from table -->
            <button class="DeleteUser" on:click={() => deleteRow(row)}>X</button>
        </tr>
    {/each}
</table>