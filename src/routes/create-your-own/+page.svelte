<script lang="ts">
    import Board from '../../Components/Board.svelte';
    import Winner from '../../Components/Winner.svelte';
    export let score : number = 0
    export let totalWords : number = 0

    let areOptionsPicked : boolean = false
    let userOptions : string[] = []
    let option : string = ''

    const handleNewWord = () => {
        userOptions = [...userOptions, option.toLocaleUpperCase()]
        option = ''

    }
</script>


{#if !areOptionsPicked}
    <div class='flex flex-col gap-3 place-content-center w-full bg-slate-300 py-6 align-middle'>

       

            <div class='flex flex-row gap-3 place-self-center place-items-center text-center w-fit ml-12'>
                <p>Enter your words here:</p>
                <input class='border border-1 rounded-sm' type='text' bind:value={option}
                on:keypress={((e) => e.key === 'Enter' ? handleNewWord() : null)}

                />
                <button 
                    on:click={() => handleNewWord()} 
                    on:keypress={((e) => e.key === 'Enter' ? handleNewWord() : null)}
                    
                    >
                    
                    Add
                </button>
                
            </div>
        

            <div class='text-center'>
                {#if userOptions.length != 0}<p class='text-2xl underline'>Your words:</p>{/if}
                {#each userOptions as word}
                    <p class='text-lg'>{word}</p>
                    
                {/each}
            </div>
        </div>
        <div class='w-full mx-auto text-center mt-2'>
        <button class='border border-1 w-fit px-2 py-1 rounded mx-auto text-center' on:click={() => areOptionsPicked = true}>Submit</button>
        </div>



{:else}
    <Board
        bind:score
        bind:totalWords
        wordsToFind={userOptions}
    />
{/if}

{#if score === totalWords && totalWords !== 0}
    <Winner />
{/if}
