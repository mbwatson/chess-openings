<script>
    export let moves
    export let index
    const pairMoves = arr => arr.reduce((result, value, i, array) => {
        if (i % 2 === 0) result.push({ white: array[i], black: array[i + 1]});
        return result
    }, [])
    $: movesArray = moves.length % 2 === 0
        ? pairMoves(moves)
        : pairMoves(moves.slice(0, -1)).concat({white: moves[moves.length - 1]})
    $: console.log(moves)
    $: console.table(movesArray)
</script>

<table class="moves-table">
    {#if movesArray.length}
        <tbody>
            {#each movesArray as turn, i (i)}
                <tr>
                    <th>{i+1}</th>
                    <td class:complete={2 * i + 1 <= index}>{turn.white || ''}</td>
                    <td class:complete={2 * i + 1 <= index - 1}>{turn.black || ''}</td>
                </tr>
            {/each}
        </tbody>
    {/if}
</table>

<style>
    .moves-table {
        width: 100px;
        background-color: var(--color-white);
        margin: 0 auto;
        border-collapse: collapse;
    }
    .moves-table tr {
        border-bottom: 1px solid var(--color-secondary);
    }
    .moves-table th {
        font-weight: bold;
        background-color: var(--color-secondary-light);
        color: var(--color-secondary);
        border-right: 1px solid var(--color-secondary);
    }
    .moves-table td, .moves-table th {
        padding: 0.25rem 0.5rem;
    }
    .moves-table td {
        transition: color 250ms;
        color: var(--color-grey);
    }
    .moves-table td.complete {
        color: var(--color-secondary);
    }
</style>
