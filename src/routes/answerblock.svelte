<script>
    // Importing component
    import Answer_button from "./answerbutton.svelte";
    import Submitbutton from "./submitbutton.svelte";
    // Defining variable, giving default values and allowing values to be filled by other components
    export let answer = ['answer','answer','answer','answer'];
    export let answerID = [0,1,2,3];
    export let answeredID;
</script>

<style>
    .buttons {
    /*Puts buttons in the center of screen horizontally*/
    width:100%;
    display: flex;
    justify-content: center;
}
</style>

<!--Seperating buttons to make two different lines. 
    Passing on:click event on to next component
    Giving each button a different answer based on the answer array -->
    

    <Answer_button answer = {answer[0]} answerID = {answerID[0]} bind:answered = {answeredID} />
    <Answer_button answer = {answer[1]} answerID = {answerID[1]} bind:answered = {answeredID} />
    <Answer_button answer = {answer[2]} answerID = {answerID[2]} bind:answered = {answeredID} />
    <Answer_button answer = {answer[3]} answerID = {answerID[3]} bind:answered = {answeredID} />
<div class="buttons">
<Submitbutton on:click/>
</div>