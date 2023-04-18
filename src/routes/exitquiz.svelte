<script>

</script>

<style>
    .exitbutton {
        /*Button background color*/
        background-color: #b88103;
        /*Button text color*/
        color: white;
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
        font-size: 14px;
    }
    .exitbutton:hover {
        background-color: #d68e10;
    }
    .exit{
        /*Puts button at bottom of screen*/
        position:absolute;
        bottom:30px;
        right:30px;
        
    }
</style>

<div class="exit">
<button class="exitbutton" on:click>Exit</button>
</div>