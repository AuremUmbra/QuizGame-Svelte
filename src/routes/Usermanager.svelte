<script>
    import UserInput from "./UserInput.svelte";
    import UserTable from "./UserTable.svelte";

    //Define variables for the Admin's input
    let newName;
    export let newUserID;
    let newUser_json;
    let newUser;
    export let userListPromise;
    let update_visibility = false;

    //Array to store data objects
    export let user_list = [];

    //Function to add a new row of user information
    async function addUser() {
        newUser =  {
            "name": "",
            "userId": 0
        }
        newUser.name = newName;
        newUser.userId = newUserID
        
        newName = null;

        newUser_json = JSON.stringify(newUser)

        const res_nUser = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/newuser`, {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: newUser_json    
        });

        const data_nUser = await res_nUser.json();

        user_list = [];
        newUserID = 0;
        if (res_nUser.ok) {
            data_nUser.users.forEach((u) => {
                user_list = [...user_list,{"name":u.name,"userID":u.userId}]
                if (newUserID <= u.userId) {
                    newUserID = (u.userId + 1)
                }
            })
        } else {
            return error;
        }
    };

</script>

<style>
    h2 {
        text-align: center;
    }
</style>

<br>
{#if ! update_visibility}
    <UserInput 
        bind:newName = {newName} 
        on:click={(() => addUser())} 
    />
{/if}

{#await userListPromise}
    <h2>Loading User List</h2>
{:then}
    <UserTable 
        user_list = {user_list} 
        {newUserID}
        bind:update_visibility = {update_visibility}
    />

{:catch error}
    <p style="color:red">{error.message}</p>
{/await}


