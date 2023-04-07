<script lang="ts">
import { onMount } from 'svelte'
 const alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
 let isLoaded = false
 let board : string[][] = []
 let boardWidth : number = 15
 let boardHeight : number = 15
 let foundWords : string[] = []
 let userGuess : string = ''
 export let wordsToFind : string[] = [
    'MONK',
    'PUBLISHER',
    'CONSTANT',
    'MAJORITY',
    'SCHEME'
 ]
 let mouseDown = false
 let letterLocations : number[][] = []
export let score : number = 0
export const totalWords : number = wordsToFind.length

onMount(() => {
    setTimeout(() => {
        isLoaded = true
    }, 750)
})

const generateLetterLocations = (word: string, wordDirection: number): number[] => {
  const wordLength: number = word.length;
  let wordStartX: number, wordStartY: number;
  
  do {
    wordStartX = Math.floor(Math.random() * (boardWidth - wordLength));
    wordStartY = Math.floor(Math.random() * (boardHeight - wordLength));
  } while (letterLocations.some(([x, y]) => x === wordStartX && y === wordStartY));
  
  //check if there is any overlap with other words
  for (let i = 0; i < wordLength; i++) {
    if (wordDirection === 0) {
      if (letterLocations.some(([x, y]) => x === wordStartX + i && y === wordStartY)) {
        return generateLetterLocations(word, wordDirection);
      }
    } else {
      if (letterLocations.some(([x, y]) => x === wordStartX && y === wordStartY + i)) {
        return generateLetterLocations(word, wordDirection);
      }
    }
  }
  
  const location: number[] = [wordStartX, wordStartY];
  letterLocations.push(location);
  return location;
};


for (let i=0; i<boardWidth; i++) {
    board.push([])
    for (let j=0; j<boardHeight; j++) {
        board[i].push(alphabet[Math.floor(Math.random() * alphabet.length)])
    }
}
for(let i=0; i<wordsToFind.length; i++){
    let word = wordsToFind[i]
    let wordDirection = Math.floor(Math.random() * 2)

    let wordStartX = generateLetterLocations(word, wordDirection)[0]
    let wordStartY = generateLetterLocations(word, wordDirection)[1]

    for(let i=0; i<word.length; i++){
        if(wordDirection === 0) {
            letterLocations = [...letterLocations, [wordStartX + i, wordStartY]]
        } else {
            letterLocations = [...letterLocations, [wordStartX, wordStartY + i]]
        }
    }

    


    for (let j=0; j<word.length; j++) {
        if (wordDirection === 0) {
            board[wordStartX + j][wordStartY] = word[j]
        } else {
            board[wordStartX][wordStartY + j] = word[j]
        }
    }
}

const handleClick = (target : any) => {
    mouseDown ? mouseDown = false : mouseDown = true

    if(!mouseDown) {

        if(wordsToFind.includes(userGuess)) {
            foundWords = [...foundWords, userGuess]
            userGuess = ''
            score += 1
            let elements = document.querySelectorAll('.bg-sky-300')
        elements.forEach(element => {
            element.classList.remove('bg-sky-300')
            element.classList.add('bg-green-300')
        })
        } else {
            let elements = document.querySelectorAll('.bg-sky-300')
        elements.forEach(element => {
            element.classList.remove('bg-sky-300')
            element.classList.add('bg-slate-400')
            
        })
            userGuess = ''
        }
        return
    }
    let letter = target.target.dataset.letter
    target.target.parentElement.classList.add('bg-sky-300')
    target.target.parentElement.classList.remove('bg-slate-400')

    userGuess = userGuess + letter

}

const handleEnter = (target : any) => {
   
    if (mouseDown) {
        let letter = target.target.dataset.letter
        userGuess = userGuess + letter
        target.target.parentElement.classList.add('bg-sky-300')
        target.target.parentElement.classList.remove('bg-slate-400')

        
    }
}
</script>

{#if isLoaded}

<div class='mx-auto my-12 w-10/12 h-10/12 bg-slate-300 p-1/2 flex flex-row gap-32'>

    <div class='select-none'>
        {#each board as row}
        <div class='flex flex-row'>
            {#each row as letter}
                <div 
                    on:mousedown={handleClick} 
                    class='w-10 h-10 bg-slate-400 text-center text-2xl hover:cursor-pointer' 
                    id='letter' 
                    data-letter="{letter}">

                        <p 
                        on:mouseenter={handleEnter}
                        on:mouseup={handleClick}
                        data-letter="{letter}"
                        >{letter}</p>

                    </div>
            {/each}
        </div>
    {/each}
    </div>
   <div>

    <div class='flex flex-col gap-2 text-xl'>
        <h1 class='text-2xl underline'>Words to find</h1>
    {#each wordsToFind as word}
        <p class='{foundWords.includes(word) ? 'text-green-500 line-through' : ''}'>{word}</p>
    {/each}

    </div>

   </div>

    </div>

{:else}
    <div class='flex flex-col items-center justify-center h-screen'>
        <h1 class='text-4xl'>Loading...</h1>
    </div>
{/if}
