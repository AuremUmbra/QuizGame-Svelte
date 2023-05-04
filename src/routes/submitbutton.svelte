<script>
    export let answeredID;
    function buttonDisabled(answeredID) {
        return answeredID == null;
    }
</script>

<style>
    .button {
        /*Button background color*/
        background-color:  #b88103;
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
        font-size: 20px;
        /*Size of the button*/
        /* width: 20%; */
        margin: 20px;
        height: 85px;
    }

    /* Color changes when hovered above */
    .button:hover {
        background-color: #d68e10;
    }

    
</style>

<!-- Button for submiting answer -->
<div class="buttons" disabled='{buttonDisabled(answeredID)}'><button class="button" on:click>Submit</button></div>