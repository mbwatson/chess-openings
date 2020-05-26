<script>
    export let moves
    export let index
    const pairMoves = arr => arr.reduce((result, value, i, array) => {
        if (i % 2 === 0) result.push(`${array[i]} ${array[i + 1]}`);
        return result
    }, [])
    $: turns = index % 2 === 0
        ? pairMoves(moves.slice(0, index))
        : pairMoves(moves.slice(0, index - 1)).concat(moves[index - 1])
</script>

<div class="move-list">
    {#if turns.length}
        {#each turns as turn, i}
            <span class="turn">{i+1}. {turn}</span>
        {/each}
    {/if}
</div>

<style>
    .move-list {
        background-color: inherit;
        padding: 0.25rem;
        min-height: 2.1rem;
        color: var(--color-secondary);
        display: flex;
        align-items: center;
    }
    .move-list .turn {
        margin-right: 0.25rem;
    }
</style>
