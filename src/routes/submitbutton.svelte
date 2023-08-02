<script>
    export let answeredID;
    export let answerID;
    function buttonDisabled(answeredID,answerID) {

        if (answeredID == answerID[0] || answeredID == answerID[1] || answeredID == answerID[2] || answeredID == answerID[3]) {
            return false;
        } else {
            return true;
        }
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

    .button:disabled {
        background-color: grey;
    }

    
</style>

<!-- Button for submiting answer -->
<div class="buttons" >
    <button class="button" disabled={buttonDisabled(answeredID,answerID)} on:click>Submit</button>
</div>