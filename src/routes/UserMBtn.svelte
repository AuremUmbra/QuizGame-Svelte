<script>
    export let User_Manager;
</script>

<style>
    .UserMButton{
        /*Puts button at top right of screen underneath logout button*/
        display:flex;
        justify-content:flex-end;
    }

    .UmBtn {
        /* Same styling as logout/in button */
         /*Button background color*/
         background-color: black;
        /*Button text color*/
        color: white;
        /*Centering text inside button*/
        text-align: center;
        /*Button rounding*/
        border-radius: 8px;
        /*Button border*/
        border: 2px solid black;
        text-decoration:none;
        display: inline-block;
        /*Text size*/
        font-size: 14px;
        margin: 10px;
        /*Button space around text*/
        padding: 1px 1px;
        width: 120px;
        height: 55px;
    }
</style>

<div class=UserMButton>
    <button class=UmBtn on:click>{User_Manager}</button>
</div>