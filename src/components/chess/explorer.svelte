<script>
    import Card from '../card/card.svelte'

    export let opening
    const movesArray = opening.pgn.split(/\d{1,2}\.\ /).slice(1).map(move => move.trim()).map(movePair => movePair.split(' ')).reduce((arr, move) => arr.concat(move), [])
    let moveIndex = 0
    import PrevIcon from '../icons/prev-icon.svelte'
    import NextIcon from '../icons/next-icon.svelte'
    import FirstIcon from '../icons/first-icon.svelte'
    import LastIcon from '../icons/last-icon.svelte'
    import MoveList from './move-list.svelte'

    import Chess from 'chess.js'
    let game = new Chess()

    $: currentMoves = movesArray.slice(0, moveIndex)
    $: currentPosition = moveIndex >= 0 && '   ' + game.ascii().trim() + '  '

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
        <h3 class="subtitle">{opening.pgn}</h3>
    </div>
    
    <div slot="body">
        <pre class="ascii-board">{currentPosition}</pre>

        <br>

    </div>

    <div slot="actions" class="actions">
        <button class="btn btn-xs btn-primary" on:click={handleGoToStart}><FirstIcon fill="var(--color-dark)" size="16" /></button>
        <button class="btn btn-xs btn-primary" on:click={handlePreviousMove}><PrevIcon fill="var(--color-dark)" size="16" /></button>
        <button class="btn btn-xs btn-primary" on:click={handleNextMove}><NextIcon fill="var(--color-dark)" size="16" /></button>
        <button class="btn btn-xs btn-primary" on:click={handleGoToEnd}><LastIcon fill="var(--color-dark)" size="16" /></button>
    </div>

    <div slot="footer">
        <MoveList moves={movesArray} index={moveIndex} />
    </div>
</Card>

<style>
    .title, .subtitle {
        text-align: center;
    }
    .ascii-board {
        text-align: center;
        color: var(--color-secondary);
    }
    .actions {
        background-color: var(--color-secondary);
        border-width: 1px 0;
        border-style: solid;
        border-color: var(--color-secondary);
        width: 100%;
        display: flex;
    }
    .actions button {
        padding: 0.5rem;
        flex: 1;
    }
</style>