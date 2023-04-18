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
    width: 25%;
    /*How fast the color changes*/
    margin: 80px;
    height: 85px;
    font-family: sans-serif;
    font-weight: bold;
    box-shadow: 1px 5px 11px black;
  }

  .button:hover {
    background-color:  rgb(255, 170, 0);
  }

  .button:active {
    background-color:  rgb(255, 155, 0);
  }
</style>

<!-- Creating a button with an on:click event that can be used by another component -->
<button class="button" on:click>Start</button>