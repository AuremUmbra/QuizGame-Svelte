<script>
    // Array to hold all question objects
    export let question_array = [];
    export let question_list = []; 
    export let questionListPromise;
    let answer_list;
    let question_description;
    let question_id;
    let answer1;
    let answer2;
    let answer3;
    let answer4;
    let update_visibility = false;
    let duplicate_visibility = false;
    // let qUpdate;
    // let qUpdate_json;
    let UpdateBtn = "Update";
    let answerTitle;
    let showDuplicateButton = "Show Duplicate Questions"; 
    let hideDuplicateButton = "Return";

    import QuestionInput from "./QuestionInput.svelte";
    import CreatequizBtn from "./CreatequizBtn.svelte";

    // Array for the headers of each column for the table holding the questions and answers
    let columns = ["Question ID","Question", "A", "B", "C", "D"];

    //function to delete a question, answer and incorrect answers
    /* async function deleteRow(qID) {
        const res = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/deletequestion/${qID}`, {
            method: 'DELETE',
        })


        questionListPromise = QuestionList()
    }; */

    // Function to get Question List from API
    async function QuestionList() {
        const res_list = await fetch('https://best-quiz-game.azurewebsites.net/GetAllQuiz');
        const data_list = await res_list.json();

        question_array = [];
        if (res_list.ok) {
            data_list.forEach((q) => {
                question_array = [...question_array,{"questionID": q.QuestionID, "questionDescription": q.QuestionText, "questionAnswers": q.Options}]
            })
        } else {
            return error;
        }

        question_list = [];
            question_array.forEach((q) => {
                answer_list = [
                    {"answerDescription": q.questionAnswers.A},
                    {"answerDescription": q.questionAnswers.B},
                    {"answerDescription": q.questionAnswers.C},
                    {"answerDescription": q.questionAnswers.D}
                ]
                question_list = [
                    ...question_list,
                    {"questionID": q.questionID,
                    "questionDescription": q.questionDescription, 
                    "answer1": answer_list[0].answerDescription, 
                    "answer2": answer_list[1].answerDescription, 
                    "answer3": answer_list[2].answerDescription, 
                    "answer4": answer_list[3].answerDescription}
                ]
        })
    }

    async function updateQuestionStart(Q) {
        question_id = Q.questionID;
        question_description = Q.questionDescription;
        answer1 = Q.answer1;
        answer2 = Q.answer2;
        answer3 = Q.answer3;
        answer4 = Q.answer4;
        
        
        const res_answer = await fetch(`https://best-quiz-game.azurewebsites.net/GetAnswer?QuestionID=${Q.questionID}`)
        const data_answer = await res_answer.json();

        answerTitle = data_answer.AnswerName;
        update_visibility = true;
    }

    async function handleUpdateQuestionEnd() {
        /* qUpdate = {
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

        questionListPromise = QuestionList()
        
        qUpdate = null;
        qUpdate_json = null;
        question_description = null;
        question_id = null;
        correct_answer = null;
        incorrect_answer1 = null;
        incorrect_answer2 = null;
        incorrect_answer3 = null; */
        console.log("Done nothing for now. API needs to change first.")
        update_visibility = false;
        
    }

    function cancelUpdate() {
        update_visibility = false;
    }


    async function duplicateQuestionList() {
        const res_list = await fetch('https://best-quiz-game.azurewebsites.net/GetDuplicateQuestions');
        const data_list = await res_list.json();

        question_array = [];
        if (res_list.ok) {
            data_list.forEach((q) => {
                question_array = [...question_array,{"questionID": q.QuestionID, "questionDescription": q.QuestionText, "questionAnswers": q.Options}]
            })
        } else {
            return error;
        }

        question_list = [];
            question_array.forEach((q) => {
                answer_list = [
                    {"answerDescription": q.questionAnswers.A},
                    {"answerDescription": q.questionAnswers.B},
                    {"answerDescription": q.questionAnswers.C},
                    {"answerDescription": q.questionAnswers.D}
                ]
                question_list = [
                    ...question_list,
                    {"questionID": q.questionID,
                    "questionDescription": q.questionDescription, 
                    "answer1": answer_list[0].answerDescription, 
                    "answer2": answer_list[1].answerDescription, 
                    "answer3": answer_list[2].answerDescription, 
                    "answer4": answer_list[3].answerDescription}
                ]
        })
    }

    function handleShowDuplicateButton() {
        duplicate_visibility = true;
        questionListPromise = duplicateQuestionList();
    }

    function handleHideDuplicateButton() {
        duplicate_visibility = false;
        questionListPromise = QuestionList();
    }
</script>


<style>
    /* .DeleteUser {
        background-color: black;
        color: white;
        padding: 10px 25px;
        margin-top: 5px;
        text-align: center;
         border-radius: 8px;
        border: none;
        text-decoration:none;
        display: inline-block;
        font-size: 14px;
    } */
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

    .CancelUpdate {
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
    .CancelUpdate:hover {
        background-color: #d68e10;
    }

    .CancelUpdate:disabled {
        background-color: grey;

    }

    .CancelUpdateDiv {
        display:flex;
        justify-content: center;
    }

</style>

{#if update_visibility}
    <QuestionInput 
        bind:Question = {question_description} 
        bind:CorrectAnswer = {answer1} 
        bind:IncorrectAnswer1 = {answer2}
        bind:IncorrectAnswer2 = {answer3}
        bind:IncorrectAnswer3 = {answer4}
        on:click = {() => handleUpdateQuestionEnd()}
        effect = {UpdateBtn}
        {answerTitle}
    />
    <div class="CancelUpdateDiv">
        <button class="CancelUpdate" on:click={(() => cancelUpdate())}>Cancel</button>
    </div>
{/if}

{#if duplicate_visibility}
    <CreatequizBtn 
        createbtn={hideDuplicateButton}
        on:click={(() => handleHideDuplicateButton())}
    />
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
                <td>{row.questionID}</td>
                <td>{row.questionDescription}</td>
                <td>{row.answer1}</td>
                <td>{row.answer2}</td>
                <td>{row.answer3}</td>
                <td>{row.answer4}</td>
            </tr>
        {/each}
    </table>

{/if}

{#if ! update_visibility && ! duplicate_visibility}
    <CreatequizBtn 
        createbtn={showDuplicateButton}
        on:click={(() => handleShowDuplicateButton())}
    />
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
                <td>{row.questionID}</td>
                <td>{row.questionDescription}</td>
                <td>{row.answer1}</td>
                <td>{row.answer2}</td>
                <td>{row.answer3}</td>
                <td>{row.answer4}</td>
                <!-- button to delete user from table -->
                <button class="UpdateUser" on:click={() => updateQuestionStart(row)}>U</button>
                <!-- <button class="DeleteUser" on:click={() => deleteRow(row.questionID)}>X</button> -->
            </tr>
        {/each}
    </table>
{/if}