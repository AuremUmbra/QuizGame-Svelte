<script>
</script>

<style>
    .button {
    /*Button background color*/
    background-color: rgb(255, 187, 0);
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
    /*Width of the button*/
    width: 100%;
    /*How fast the color changes*/
    margin: 80px;
    height: 85px;
    font-family: sans-serif;
    font-weight: bold;
    box-shadow: 1px 5px 11px black;
  }

  /* Color changes when hovered above */
  .button:hover {
    background-color:  rgb(255, 170, 0);
  }

  /* Color changes when actived */
  .button:active {
    background-color:  rgb(255, 155, 0);
  }

  .startbutton { 
    /*Puts buttons in the center of screen horizontally*/
    width: 100%;
    display:flex;
    justify-self:center;
  } 
</style>

<!-- Creating a button with an on:click event that can be used by another component -->
<div class="startbutton" ><button class="button" on:click>Start</button></div>