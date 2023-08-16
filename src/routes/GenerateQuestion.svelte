<script>

    import CreatequizBtn from "./CreatequizBtn.svelte";

    export let generateQuestionQuantity;
    export let generateQuestionTopic;
    export let AINewQuestion;
    let previousN = 1;

    function createBtnDisabled(gQQ,gQT) {
        if (gQQ == null || gQQ == "") {
            return true;
        }
        if (gQT == null || gQT == "") {
            return true;
        }
        if (gQQ < 11 && gQQ > 0) {
            return false;
        }
        return true
    }

    function validator(node, value) {
    return {
      update(value) {
				generateQuestionQuantity = value === null || generateQuestionQuantity < node.min ? previousN : parseInt(value)
        previousN = generateQuestionQuantity
      }
    }
  }

</script>



<style>
    .generateQuizInput {
        text-align:center;
        border-radius: 8px;
        margin:0px;
        padding:0px 0px;
        width:220px;
        height:45px;
    }

    .generateQuiz {
        display: flex;
        justify-content: center;
    }

</style>


<div class="generateQuiz">
    <div>
        <input class="generateQuizInput" placeholder="Generate Question Topic" bind:value={generateQuestionTopic}/>
        <input class="generateQuizInput" placeholder="Generate Question Quantity" use:validator={generateQuestionQuantity} type='number' min=1 bind:value={generateQuestionQuantity}/>
        <CreatequizBtn createBtnDisabled = {createBtnDisabled(generateQuestionQuantity,generateQuestionTopic)} createbtn = {AINewQuestion} on:click/>
    </div>
</div>