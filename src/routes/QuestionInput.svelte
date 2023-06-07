<script>
    // define variables
    export let Question;
    export let CorrectAnswer;
    export let IncorrectAnswer1;
    export let IncorrectAnswer2;
    export let IncorrectAnswer3;
    

    // existing arrays for questions, correct answers and incorrect answers.
    // let questions = ['In which suburb is the Digital Traineeship Programme being run?','Which Train Station is the closest to the Cremorne Campus of Kangan Institute?','What is the capital city of Australia?'];
    // let correct_answers = ['Cremorne','Richmond','Canberra'];
    // let incorrect_answers = ['South Yarra','Bendigo','Dandenong','Frankston','Cranburne','Berwick','Beaconsfield','Adelaide','Brisbane','Perth','Paris','Melbourne','Sydney','Newcastle'];

    function NewQuestionDisabled(Q,CA,IA1,IA2,IA3) {
        if (Q == null || Q == "") {
            return true
        }
        if (CA == null || CA == "") {
            return true
        } 
        if (IA1 == null || IA1 == "") {
            return true
        } 
        if (IA2 == null || IA2 == "") {
            return true
        } 
        if (IA3 == null || IA3 == "") {
            return true
        } 
        return false
    }
</script>

<style>
        input {
        padding: 15px 32px;
        width: 80%; 
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
        font-size: 1.1rem;
        width:25%;
    }
    .Add:hover {
        background-color: #d68e10;
    }

    .Add:disabled {
        background-color: grey;
    }
     .QuestionInput {
        text-align: center;
        width: 245px;
        margin-right: auto;
        margin-left: 30px;
        display:contents;
        justify-content: center;
    }

    .InputBox {
        padding: 8px;
        
    }

    /* Aligning the add question and answers button to the left of the divider holding the inputs */
    .AddBtnDiv {
        padding: 8px;
        text-align: inherit;
        
    }
    
    p {
        text-align: inherit;
        font-size: 1.2em;
    }
</style>

<!-- Input for a new question, new answer and new incorrect answers -->
<div class="QuestionInput">
    <div class="InputBox">
        <p>Question:</p>
        <input class="question" bind:value={Question}>
    </div>

    <div class="InputBox">
        <p>Correct Answer:</p><input class="correct_answer" bind:value={CorrectAnswer}>
    </div>

    <div class="InputBox">
        <p>Incorrect Answer 1:</p><input class="incorrect_answer" bind:value={IncorrectAnswer1}>
    </div>

    <div class="InputBox">
        <p>Incorrect Answer 2:</p><input class="incorrect_answer" bind:value={IncorrectAnswer2}>
    </div>

    <div class="InputBox">
        <p>Incorrect Answer 3:</p><input class="incorrect_answer" bind:value={IncorrectAnswer3}>
    </div>

    <!-- A button to add a new row consisting of a question, the correct answer and 3 incorrect answers -->
    <div class="AddBtnDiv"><button disabled={NewQuestionDisabled(Question,CorrectAnswer,IncorrectAnswer1,IncorrectAnswer2,IncorrectAnswer3)} class="Add" on:click>Add</button></div>
</div>

