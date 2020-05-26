<script>
    export let moves
    const pairMoves = arr => arr.reduce((result, value, i, array) => {
        if (i % 2 === 0) result.push(array.slice(i, i + 2));
        return result
    }, [])
    $: movesArray = moves.length % 2 === 0
        ? pairMoves(moves)
        : pairMoves(moves.slice(0, -1)).concat(moves.slice(-1))
</script>

<table class="move-table">
    {#if movesArray.length}
        <tbody>
            {#each movesArray as turn, i (i)}
                <tr>
                    <th>{i+1}</th>
                    <td>{turn[0]}</td>
                    <td>{turn[1]}</td>
                </tr>
            {/each}
        </tbody>
    {/if}
</table>

<style>
    .move-table {
        background-color: var(--color-white);
        margin: 0 auto;
        border-collapse: collapse;
    }
    .move-table td, .move-table th {
        padding: 0.25rem 0.5rem;
    }
    .move-table th {
        font-weight: bold;
        background-color: var(--color-secondary-light);
        color: var(--color-secondary);
        border-right: 1px solid var(--color-secondary);
    }
    .move-table tr {
        border-bottom: 1px solid var(--color-secondary);
    }
</style>
