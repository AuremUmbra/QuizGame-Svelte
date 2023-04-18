<script>
    // Importing components
    import Startbutton from "./startbutton.svelte";
    import Title from "./title.svelte";
    import Logos from "./Logos.svelte";
    // Defining Title variable
</script>

<style>
    .buttons {
    /*Puts buttons in the center of screen horizontally*/
    display: flex;
    justify-content: center;
}


</style>


    <!-- Creating title -->
    <Logos />
    <Title/>
    <!-- Using imported component and passing on:click event on to another component -->
    <div class=buttons>
        <Startbutton on:click/>
    </div>
