<script>
    // Defining variables and allowing them to be filled by other components
    export let questions;
    export let i = 0;
    export let answeredID;
    export let answers;
    export let answerID;
    export let length;
    export let duplicate;
    
    //Importing component 
    import Answerblock from "./answerblock.svelte";

    

</script>

<style>
    /*Question style*/
h2 {
    text-align: center;
    font-size: 28px;
    font: sans-serif;
    font-weight: bold;
}
/* Answer Block Styling */

.ansblock {
    background-color: white;
    padding: 35px;
    margin-top: 55px;
    box-shadow: 5px 5px 18px black;
}
/* Background color */

body {
        background-color:  #298fbf;
    }

</style>
<body>
<!-- Question number and number of total questions -->
        <h2>Question {i+1}/{length}</h2>
        <!-- Asking the question -->
        <h2>{questions}</h2>

    
    <!-- Using component,
        passing on on:click event to another component
        Giving values for the different buttons to show
     -->
    <div class="ansblock">
    <Answerblock answer = {answers} answerID = {answerID} bind:answeredID = {answeredID} bind:duplicate on:click/>
    </div>
</body>