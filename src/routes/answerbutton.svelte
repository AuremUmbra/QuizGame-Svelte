<script>   

    // Defining variable, giving default value and allowing value to be filled by other components
    export let answer = 'answer';
    export let answerID;
    export let answered;

  

</script>

<style>
    /* .button {
    /*Button background color*/
    /* background-color: #fbb900;
    /*Button text color*/
    /* color: black; */
    /*Button space around text*/
    /* padding: 15px 32px; */
    /*Centering text inside button*/
    /* text-align: center; */
    /*Button rounding*/
    /* border-radius: 8px; */
    /*Button border*/
    /* border: none; */
    /* text-decoration:none; */
    /* display: inline-block; */
    /*Text size*/
    /* font-size: 20px;
    font-weight: bold; */
    /*Size of the button*/
    /* width: 90%;
    margin: 20px;
    height: 85px; */
  /* } */

  /* button:hover {
    background-color: rgb(255, 193, 69);
  } */
  /* .buttons { */
    /*Puts buttons in the center of screen horizontally*/
    /* width: 100%;
    display:flex;
    justify-self:center;
  } */

  /* .AnswerSelect {
    display: inline-flex;
    justify-items: center;
    justify-self: space-between;
    width:100%;
    padding:1.25rem;
    color:black;
    background-color:  #298fbf;
    border:none;
    border-radius: 8px;
    cursor:pointer;
    
  }
  .AnswerSelect:hover {
    color:rgb(77, 77, 77);
  }
  .AnswerSelect:peer-checked {
    color:#298fbf;
    background-color:rgb(255, 187, 0)
  } */ 
</style>

<!-- Creating a button with a on:click event that will be used by a different component and a text that will be defined by another component with the default being 'answer' -->
<!-- <div class="buttons"><button id={answerID} class="button" on:click={() => handleScore({answerID})}>{answer}</button></div> -->

<!-- List item -->
<li>
  <!-- Radio button input. ID matches label. Class to make radio button part hidden. -->
  <input type="radio" id={answerID} name="hosting" value={answerID} bind:group={answered} class="hidden peer" required>
  <!-- Label for radio button. Styled in class part using flowbite. -->
  <label for={answerID} class="inline-flex items-center justify-between w-full p-5 text-black bg-sky-500 border-none rounded-lg cursor-pointer peer-checked:text-blue-600 peer-checked:bg-orange-500 hover:text-gray-600 hover:bg-sky-300">                           
      <div class="block">
          <div class="w-full text-lg font-semibold">{answer}</div>
      </div>
      <svg aria-hidden="true" class="w-6 h-6 ml-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
  </label>
</li>

<!-- class for label - "inline-flex items-center justify-between w-full p-5 text-gray-500 bg-white border border-gray-200 rounded-lg cursor-pointer dark:hover:text-gray-300 dark:border-gray-700 dark:peer-checked:text-blue-500 peer-checked:border-blue-600 peer-checked:text-blue-600 hover:text-gray-600 hover:bg-gray-100 dark:text-gray-400 dark:bg-gray-800 dark:hover:bg-gray-700" -->
<!--                      display   justify-items justify-self    width padding color    background-color  border-color  border-radius curser         text color on hover                                      -->