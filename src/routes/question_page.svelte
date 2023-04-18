<script>
    // Defining variables and allowing them to be filled by other components
    export let questions = ['In which suburb is the Digital Traineeship Programme being run?','Which Train Station is the closest to the Cremorne Campus of Kangan Institute?','What is the capital city of Australia?'];
    export let correct_answers = ['Cremorne','Richmond','Canberra'];
    export let incorrect_answers = ['South Yarra','Bendigo','Dandenong','Frankston','Cranburne','Berwick','Beaconsfield','Adelaide','Brisbane','Perth','Paris','Melbourne','Sydney','Newcastle'];
    export let i = 0
    
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

.ansblock {
    background-color: white;
    padding: 35px;
    margin-top: 55px;
    box-shadow: 5px 5px 18px black;
}

</style>

<!-- Question number and number of total questions -->
        <h2>Question {i+1}/{questions.length}</h2>
        <!-- Asking the question -->
        <h2>{questions[i]}</h2>

    
    <!-- Using component,
        passing on on:click event to another component
        Giving values for the different buttons to show
     -->
<div class="ansblock">
    <Answerblock on:click answer={ [
        correct_answers[i],
        incorrect_answers[i*3],
        incorrect_answers[(i*3+1)],
        incorrect_answers[(i*3+2)]
    ]}/> 
</div>