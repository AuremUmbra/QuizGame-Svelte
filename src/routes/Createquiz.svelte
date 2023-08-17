<script>
    // import CreatequizBtn from "./CreatequizBtn.svelte";
    // import QuestionInput from "./QuestionInput.svelte";
    import QuestionTable from "./QuestionTable.svelte";
    import GenerateQuestion from "./GenerateQuestion.svelte";

    export let question_array = [];
    export let questionListPromise;
    export let question_list = [];
    /* let newQuestion;
    let newcorrectAnswer;
    let newincorrectAnswer1;
    let newincorrectAnswer2;
    let newincorrectAnswer3;
    let qNew;
    let qNew_json; */
    let answer_list = [];
    let showNewQuestion = false;
    let createbtn = "Make New Question";
    let AINewQuestion = "Get New Question From AI";
    let verificationAI;
    let generateQuestionTopic;
    let generateQuestionQuantity;


    // async function AddNewQuestion() {
    //     qNew = {
    //         "questionID":"",
    //         "questionDescription":"", "questionAnswers": [
    //             {"answerDescription":"", "isCorrect": true, "answerID":""}, 
    //             {"answerDescription":"", "isCorrect": false, "answerID":""}, 
    //             {"answerDescription":"", "isCorrect": false, "answerID":""}, 
    //             {"answerDescription":"", "isCorrect": false, "answerID":""}
    //         ]
    //     };
    //     qNew.questionDescription = newQuestion;
    //     qNew.questionAnswers[0].answerDescription = newcorrectAnswer;
    //     qNew.questionAnswers[1].answerDescription = newincorrectAnswer1;
    //     qNew.questionAnswers[2].answerDescription = newincorrectAnswer2;
    //     qNew.questionAnswers[3].answerDescription = newincorrectAnswer3;

      
    //     qNew_json = JSON.stringify(qNew)

    //     const res_nQuestion = await fetch(`https://dotnetcore78277kangan.azurewebsites.net/Question/Add`, {
    //         method: 'POST',
    //         headers: {'Content-Type': 'application/json'},
    //         body: qNew_json
    //     });

    //     const data_nQuestion = await res_nQuestion.json();

    //     question_array = [];
    //     if (res_nQuestion.ok) {
    //         data_nQuestion.questions.forEach((q) => {
    //             question_array = [...question_array,{"questionID": q.questionID, "questionDescription": q.questionText, "questionAnswers": q.questionAnswers}]
    //         })
    //     } else {
    //         return error;
    //     }

    //     question_list = [];
    //         question_array.forEach((q) => {
    //             answer_list = [];
    //             q.questionAnswers.forEach((a) => {
    //                 answer_list = [...answer_list,{"answerDescription": a.answerText, /* "isCorrect": a.isCorrect */}]
    //             })
    //             /* answer_list.sort((a,b) => {if (a.isCorrect && b.isCorrect) return 0;
    //                 if (a.isCorrect) return -1;
    //                 if (b.isCorrect) return 1;
    //             }) */
    //             question_list = [...question_list,
    //                 {"questionID": q.questionID,
    //                 "questionDescription": q.questionDescription, 
    //                 "correctAnswer": answer_list[0].answerDescription, 
    //                 "incorrectAnswer1": answer_list[1].answerDescription, 
    //                 "incorrectAnswer2": answer_list[2].answerDescription, 
    //                 "incorrectAnswer3": answer_list[3].answerDescription}]
                
    //         })

    //     newQuestion = null;
    //     newcorrectAnswer  = null;
    //     newincorrectAnswer1 = null;
    //     newincorrectAnswer2 = null;
    //     newincorrectAnswer3 = null;
    //     showNewQuestion = false;
    //     createbtn = "Make New Question"
    // }

    function MakeNewQuestion() {
        if (showNewQuestion == false) {
            createbtn = "Exit";
            showNewQuestion = true;
        } else {
            showNewQuestion = false;
            QuestionList()
            createbtn = "Make New Question";
        }
    }
    
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

    async function GenerateNewAIQuestion () {
        const res_NewAIQuestion = await fetch(`https://best-quiz-game.azurewebsites.net/generatequiz?topic=${generateQuestionTopic}&qty=${generateQuestionQuantity}`)
        const data_NewAIQuestion = await res_NewAIQuestion.json();

        questionListPromise = QuestionList();

    }

    // function handleAddNewQuestion() {
    //     questionListPromise = AddNewQuestion();
    // }
</script>

<style>
   h2 {
        text-align: center;
        font-size: 28px;
        font: sans-serif;
        font-weight: bold;
    }
</style>

<br>
<!-- <CreatequizBtn {createbtn} on:click = {() => MakeNewQuestion()}/> -->
<GenerateQuestion 
    bind:generateQuestionQuantity = {generateQuestionQuantity}
    bind:generateQuestionTopic = {generateQuestionTopic}
    {AINewQuestion}
    on:click = {(() => GenerateNewAIQuestion())}
/>

<!-- {#if showNewQuestion == true } 
    <QuestionInput 
        on:click={() => handleAddNewQuestion()}
        bind:Question={newQuestion} 
        bind:CorrectAnswer={newcorrectAnswer} 
        bind:IncorrectAnswer1={newincorrectAnswer1} 
        bind:IncorrectAnswer2={newincorrectAnswer2} 
        bind:IncorrectAnswer3={newincorrectAnswer3}
    />

{:else} -->
    {#await questionListPromise}
        <h2>Loading Question List</h2>
    {:then}
        <QuestionTable 
            {question_list} 
            {question_array}
            {questionListPromise}
        />
    
    {:catch error}
        <h2 style="color:red">{error.message}</h2>
    {/await}
<!-- {/if} -->