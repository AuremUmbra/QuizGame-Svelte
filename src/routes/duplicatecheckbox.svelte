<script>
    export let duplicate = false;
    export let duplicatetext;
    export let duplicatedisabled;
</script>

<div>
    <input id="duplicatecheckbox" disabled={duplicatedisabled} type=checkbox  bind:checked={duplicate} class="w-4 h-4 text-orange-500 border-gray-300 rounded focus:ring-orange-500 dark:focus:ring-orange-500 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600 disabled:text-grey-700 disabled:bg-gray-300 peer" >
    <label for="duplicatecheckbox" class="ml-2 text-m font-medium text-sky-500 peer-disabled:text-gray-300 peer-disabled:line-through" >{duplicatetext}</label>
</div>