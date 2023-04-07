<script lang=ts>
    const navItems = [
        { name: 'Home', link: '/' },
        { name: 'Wordsearch', link: '/wordsearch'},
        { name: 'Create Your Own!', link: '/create-your-own'}
    ];
</script>
<div class='bg-stone-300 w-full h-fit p-3 flex flex-row gap-10 place-content-center drop-shadow-sm'>
    {#each navItems as nav}
        
            <a href="{nav.link}" class='hover:brightness-50 hover:drop-shadow-sm' > <p>{nav.name}</p> </a>
        
    {/each}

</div>