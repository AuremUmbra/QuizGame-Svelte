<script>
    //Define variables for the Admin's input
    let newName;
    let newEmail;

    //Define array for the table headers
    let columns = ["Name", "Email", "Date Created", "Last Updated"];

    //Array to store data objects
    let data = [
        {name: "Admin", email:"admin@gmail.com", datecreated: new Date(), lastupdated: new Date()},
        {name: "User", email:"user@gmail.com", datecreated: new Date(), lastupdated: new Date()}
    ];

    //Function to add a new row of user information
    function addRow() {
        let newUser =  {name: "", email:"", datecreated:"", lastupdated:""}
        newUser.name = newName;
        newUser.email = newEmail;
        newUser.datecreated = new Date();
        newUser.lastupdated = new Date();
        data = [...data, newUser];
    };

    //Function to delete a row of user information from the table (not the database)
    function deleteRow(rowToBeDeleted) {
        data = data.filter(row => row != rowToBeDeleted)
    };
</script>

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
            <td>{row.email}</td>
            <td>{row.datecreated}</td>
            <td>{row.lastupdated}</td>
            <!-- button to delete user from table -->
            <button class="DeleteUser" on:click={() => deleteRow(row)}>X</button>
        </tr>
    {/each}
</table>



<!-- New user input -->
<div class="NewUserInput">
    <input placeholder="Name" type="text" bind:value={newName} /> 
    <input placeholder="Email" type="text" bind:value={newEmail} /> 

<!-- Button to add new user row to table -->
    <button class="Add" on:click={addRow}>Add</button>
</div>

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

    #column-right {
        border: none;
    }
</style>