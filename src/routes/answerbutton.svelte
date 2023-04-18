<script>
    // Defining variable, giving default value and allowing value to be filled by other components
    export let answer = 'answer';
</script>

<style>
    .button {
    /*Button background color*/
    background-color: #fbb900;
    /*Button text color*/
    color: black;
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
    font-size: 20px;
    font-weight: bold;
    /*Size of the button*/
    width: 90%;
    margin: 20px;
    height: 85px;
  }

  button:hover {
    background-color: rgb(255, 193, 69);
  }
  .buttons {
    /*Puts buttons in the center of screen horizontally*/
    width: 100%;
    display:flex;
    justify-self:center;
  }
</style>

<!-- Creating a button with a on:click event that will be used by a different component and a text that will be defined by another component with the default being 'answer' -->
<div class="buttons"><button class="button" on:click>{answer}</button></div>