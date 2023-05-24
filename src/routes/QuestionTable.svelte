<script>
    // Array to hold all question objects
    export let question_array = [];
    export let question_list = []; 
    let answer_list;

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
            })
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
</style>

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
            <button class="DeleteUser" on:click={() => deleteRow(row.questionID)}>X</button>
        </tr>
    {/each}
</table>