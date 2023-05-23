<script>
    // define variables
    export let newQuestion;
    export let newcorrectAnswer;
    export let newincorrectAnswer1;
    export let newincorrectAnswer2;
    export let newincorrectAnswer3;
    // Array to hold all question objects
    export let question_array = [];
    export let question_list = []; 
    

    // existing arrays for questions, correct answers and incorrect answers.
    // let questions = ['In which suburb is the Digital Traineeship Programme being run?','Which Train Station is the closest to the Cremorne Campus of Kangan Institute?','What is the capital city of Australia?'];
    // let correct_answers = ['Cremorne','Richmond','Canberra'];
    // let incorrect_answers = ['South Yarra','Bendigo','Dandenong','Frankston','Cranburne','Berwick','Beaconsfield','Adelaide','Brisbane','Perth','Paris','Melbourne','Sydney','Newcastle'];

    // Array for the headers of each column for the table holding the questions and answers
    let columns = ["Question", "Correct Answer", "Incorrect Answer 1", "Incorrect Answer 2", "Incorrect Answer 3"];

    //function to delete a question, answer and incorrect answers
    function deleteRow(rowToBeDeleted) {
        question_array = question_array.filter(row => row != rowToBeDeleted)
    };


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
    input {
        padding: 15px 32px;
    }
    /* .NewUserInput {
        margin-left: auto;
        margin-right: auto;
        text-align: center;
    } */
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

    /* #column-right {
        border: none;
    } */

    .QuestionInput {
        text-align: center;
        width: 245px;
        margin-right: auto;
        margin-left: 30px;
    }

    .InputBox {
        padding: 8px;
    }

    /* Aligning the add question and answers button to the left of the divider holding the inputs */
    .AddBtnDiv {
        padding: 8px;
        text-align: left;
    }
    
    p {
        text-align: left;
    }
</style>

<!-- Input for a new question, new answer and new incorrect answers -->
<div class="QuestionInput">
    <div class="InputBox">
        <p>Question:</p>
        <input class="question" bind:value={newQuestion}>
    </div>

    <div class="InputBox">
        <p>Correct Answer:</p><input class="correct_answer" bind:value={newcorrectAnswer}>
    </div>

    <div class="InputBox">
        <p>Incorrect Answer 1:</p><input class="incorrect_answer" bind:value={newincorrectAnswer1}>
    </div>

    <div class="InputBox">
        <p>Incorrect Answer 2:</p><input class="incorrect_answer" bind:value={newincorrectAnswer2}>
    </div>

    <div class="InputBox">
        <p>Incorrect Answer 3:</p><input class="incorrect_answer" bind:value={newincorrectAnswer3}>
    </div>

    <!-- A button to add a new row consisting of a question, the correct answer and 3 incorrect answers -->
    <div class="AddBtnDiv"><button class="Add" on:click>Add</button></div>
</div>

<!-- A table to hold all the questions, answers and incorrect answers -->
<table>
    <!-- The column headers -->
    <tr>
        {#each columns as column}
            <th>{column}</th>
        {/each}
    </tr>
    <!-- The table data -->
    {#each question_list as row}
        <tr>
            <td>{row.questionDescription}</td>
            <td>{row.correctAnswer}</td>
            <td>{row.incorrectAnswer1}</td>
            <td>{row.incorrectAnswer2}</td>
            <td>{row.incorrectAnswer3}</td>
            <!-- button to delete user from table -->
            <button class="DeleteUser" on:click={() => deleteRow(row)}>X</button>
        </tr>
    {/each}
</table>