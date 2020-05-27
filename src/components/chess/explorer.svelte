<script>
    import Card from '../card/card.svelte'

    export let opening
    import Board from './board.svelte'
    import CurrentMoveList from './moves-list.svelte'
    import MovesTable from './moves-table.svelte'
    let showMoves = false

    const movesArray = opening.pgn.split(/\d{1,2}\.\ /).slice(1).map(move => move.trim()).map(movePair => movePair.split(' ')).reduce((arr, move) => arr.concat(move), [])
    let moveIndex = 0
    import PrevIcon from '../icons/prev-icon.svelte'
    import NextIcon from '../icons/next-icon.svelte'
    import FirstIcon from '../icons/first-icon.svelte'
    import LastIcon from '../icons/last-icon.svelte'

    import Chess from 'chess.js'
    let game = new Chess()

    $: currentMoves = movesArray.slice(0, moveIndex)
    $: currentPosition = moveIndex >= 0 && '   ' + game.ascii().trim() + '  '

    const handleToggleShowMoves = () => showMoves = !showMoves

    const handleGoToStart = () => {
        moveIndex = 0
        game.reset()
    }
    const handlePreviousMove = () => {
        moveIndex = Math.max(0, moveIndex - 1)
        game.undo()
    }
    const handleNextMove = () => {
        game.move(movesArray[moveIndex])
        moveIndex = Math.min(moveIndex + 1, movesArray.length)
    }
    const handleGoToEnd = () => {
        movesArray.forEach(move => game.move(move))
        moveIndex = movesArray.length
    }
</script>

<Card>
    <div slot="header">
        <h2 class="title">{opening.name}</h2>
    </div>
    
    <div slot="body" class="explorer-interior" on:click={handleToggleShowMoves}>
        <div class="board-container">
            <Board position={currentPosition} />
        </div>
        <div class="moves-table-container">
            <MovesTable moves={movesArray} index={moveIndex} visible={showMoves} />
        </div>
    </div>

    <div slot="footer" class="actions">
        <button class="btn btn-xs btn-primary" disabled={ moveIndex === 0 } on:click={handleGoToStart}><FirstIcon fill="var(--color-dark)" size="16" /></button>
        <button class="btn btn-xs btn-primary" disabled={ moveIndex === 0 }  on:click={handlePreviousMove}><PrevIcon fill="var(--color-dark)" size="16" /></button>
        <button class="btn btn-xs btn-primary" disabled={ moveIndex === movesArray.length }  on:click={handleNextMove}><NextIcon fill="var(--color-dark)" size="16" /></button>
        <button class="btn btn-xs btn-primary" disabled={ moveIndex === movesArray.length }  on:click={handleGoToEnd}><LastIcon fill="var(--color-dark)" size="16" /></button>
    </div>
</Card>

<style>
    .title {
        text-align: center;
    }
    .explorer-interior {
        display: flex;
        flex-direction: row;
    }
    .explorer-interior .board-container {
        flex: 1;
        padding: 0.5rem;
    }
    .explorer-interior .moves-table-container {
        border-left: 1px solid var(--color-secondary);
    }
    .actions {
        background-color: var(--color-secondary);
        width: 100%;
        display: flex;
    }
    .actions button {
        padding: 0.5rem;
        flex: 1;
    }
</style>