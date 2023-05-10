<script>
    export let duplicate = false;
    export let duplicatetext;
</script>

<label>
    <input type=checkbox bind:checked={duplicate}>{duplicatetext}
</label>