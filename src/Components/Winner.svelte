<div class='z-50 absolute inset-0 flex items-center justify-center bg-black bg-opacity-75'>
    <div>
        <div class='bg-white p-4 rounded-lg'>
            <h1 class='text-2xl font-bold'>You Win!</h1>
            <p class='text-lg'>You found all the words!</p>
        </div>
        <div class='flex flex-row gap-4 mt-4'>
            <button class='bg-green-300 p-2 rounded-lg hover:bg-green-400' on:click={() => {window.location.reload()}}>Play Again</button>
            <button class='bg-red-300 p-2 rounded-lg hover:bg-red-400' on:click={() => {window.location.href = '/'}}>Home</button>
        </div>
    </div>
</div>