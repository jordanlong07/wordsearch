<script lang=ts>
    import Board from '../../Components/Board.svelte';
    import Winner from '../../Components/Winner.svelte';
    export let score : number = 0
    export let totalWords : number = 0
</script>
<div  class='h-full w-full'>
    <div class='text-center mt-3'>
        Score = {score}/{totalWords}
    </div>
    <Board
        bind:score
        bind:totalWords
    />
    {#if score === totalWords && totalWords !== 0}
        <Winner />
    {/if}
</div>

