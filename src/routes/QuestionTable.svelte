<script>
    // Array to hold all question objects
    export let question_array = [];
    export let question_list = []; 
    let answer_list;
    let question_description;
    let question_id;
    let correct_answer;
    let incorrect_answer1;
    let incorrect_answer2;
    let incorrect_answer3;
    let update_visibility = false;
    let qUpdate;
    let qUpdate_json;
    export let newID;
    let UpdateBtn = "Update";

    import QuestionInput from "./QuestionInput.svelte";

    // Array for the headers of each column for the table holding the questions and answers
    let columns = ["Question", "Correct Answer", "Incorrect Answer 1", "Incorrect Answer 2", "Incorrect Answer 3"];

    //function to delete a question, answer and incorrect answers
    async function deleteRow(qID) {
        const res = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/deletequestion/${qID}`, {
            method: 'DELETE',
        })


        QuestionList()
    };

    // Function to get Question List from API
    async function QuestionList() {
        const res_list = await fetch('https://dotnetcore78277kangan.azurewebsites.net/AllQuestions');
        const data_list = await res_list.json();

        question_array = [];
        if (res_list.ok) {
            data_list.questions.forEach((q) => {
                question_array = [...question_array,{"questionID": q.questionID, "questionDescription": q.questionDescription, "questionAnswers": q.questionAnswers}]
            })
        } else {
            return error;
        }

        question_list = [];
            question_array.forEach((q) => {
                answer_list = [];
                q.questionAnswers.forEach((a) => {
                    answer_list = [...answer_list,{"answerDescription": a.answerDescription, "isCorrect": a.isCorrect}]
                })
                answer_list.sort((a,b) => {if (a.isCorrect && b.isCorrect) return 0;
                    if (a.isCorrect) return -1;
                    if (b.isCorrect) return 1;
                })
                question_list = [...question_list,
                    {"questionID": q.questionID,
                    "questionDescription": q.questionDescription, 
                    "correctAnswer": answer_list[0].answerDescription, 
                    "incorrectAnswer1": answer_list[1].answerDescription, 
                    "incorrectAnswer2": answer_list[2].answerDescription, 
                    "incorrectAnswer3": answer_list[3].answerDescription}]
                if (newID < q.questionID) {
                    newID = q.questionID + 1
                }
            })
    }

    function updateQuestionStart(Q) {
        question_id = Q.questionID;
        question_description = Q.questionDescription;
        correct_answer = Q.correctAnswer;
        incorrect_answer1 = Q.incorrectAnswer1;
        incorrect_answer2 = Q.incorrectAnswer2;
        incorrect_answer3 = Q.incorrectAnswer3;
        update_visibility = true;
    }

    async function handleUpdateQuestionEnd() {
        qUpdate = {
            "questionDescription":"", "questionAnswers": [
            {"answerDescription":"", "isCorrect": true}, 
            {"answerDescription":"", "isCorrect": false}, 
            {"answerDescription":"", "isCorrect": false}, 
            {"answerDescription":"", "isCorrect": false}]};
        qUpdate.questionDescription = question_description;
        qUpdate.questionAnswers[0].answerDescription = correct_answer;
        qUpdate.questionAnswers[1].answerDescription = incorrect_answer1;
        qUpdate.questionAnswers[2].answerDescription = incorrect_answer2;
        qUpdate.questionAnswers[3].answerDescription = incorrect_answer3;

        qUpdate_json = JSON.stringify(qUpdate)

        const res_update = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/updatequestion/${question_id}`, {
            method: 'PUT',
            headers: {'Content-Type': 'application/json'},
            body: qUpdate_json
        })

        QuestionList()
        update_visibility = false;
        qUpdate = null;
        qUpdate_json = null;
        question_description = null;
        question_id = null;
        correct_answer = null;
        incorrect_answer1 = null;
        incorrect_answer2 = null;
        incorrect_answer3 = null;
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


{#if update_visibility}
    <QuestionInput 
        bind:Question = {question_description} 
        bind:CorrectAnswer = {correct_answer} 
        bind:IncorrectAnswer1 = {incorrect_answer1}
        bind:IncorrectAnswer2 = {incorrect_answer2}
        bind:IncorrectAnswer3 = {incorrect_answer3}
        on:click = {() => handleUpdateQuestionEnd()}
        effect = {UpdateBtn}
    />
{/if}

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
            <button class="UpdateUser" on:click={() => updateQuestionStart(row)}>U</button>
            <button class="DeleteUser" on:click={() => deleteRow(row.questionID)}>X</button>
        </tr>
    {/each}
</table>