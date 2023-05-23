<script>
    import QuestionInput from "./QuestionInput.svelte";
    export let question_array = [];
    export let questionListPromise;
    export let question_list = [];
    export let newQuestion;
    export let newcorrectAnswer;
    export let newincorrectAnswer1;
    export let newincorrectAnswer2;
    export let newincorrectAnswer3;
</script>

<style>

</style>

{#await questionListPromise}
    <h2>Loading Question List</h2>
{:then}
    <QuestionInput 
        {question_array} 
        {question_list} 
        on:click 
        bind:newQuestion={newQuestion} 
        bind:newcorrectAnswer={newcorrectAnswer} 
        bind:newincorrectAnswer1={newincorrectAnswer1} 
        bind:newincorrectAnswer2={newincorrectAnswer2} 
        bind:newincorrectAnswer3={newincorrectAnswer3}
    />
{:catch error}
    <p style="color:red">{error.message}</p>
{/await}