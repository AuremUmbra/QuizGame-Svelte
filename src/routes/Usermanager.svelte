<script>
    import UserInput from "./UserInput.svelte";
    import UserTable from "./UserTable.svelte";

    //Define variables for the Admin's input
    let newName;
    let newEmail;
    export let userListPromise;

    

    //Array to store data objects
    export let data = [
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

</script>

<style>
    h2 {
        text-align: center;
    }
</style>

<br>
<UserInput 
    bind:newName = {newName} 
    bind:newEmail = {newEmail} 
    on:click={(() => addRow())} 
/>

{#await userListPromise}
    <h2>Loading User List</h2>
{:then}
    <UserTable 
        data = {data} 
    />

{:catch error}
    <p style="color:red">{error.message}</p>
{/await}


