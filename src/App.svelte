<script>
    import openings from './data/openings.json'
    import Header from './components/layout/header.svelte'
    import Main from './components/layout/main.svelte'
    import Footer from './components/layout/footer.svelte'
    import Explorer from './components/chess/explorer.svelte'
    import CardGrid from './components/card/card-grid.svelte'
    let query = ''
    $: filteredOpenings = openings.filter(({ name }) => name.toLowerCase().includes(query.toLowerCase()))
</script>

<Header></Header>
<Main>
    <input type="text" bind:value={query} placeholder="Filter openings">
    <CardGrid>
    {#each filteredOpenings.sort((o, p) => o.name > p.name) as opening (opening.id)}
        <Explorer opening={opening} />
    {:else}
        <div class="notice">
            Sorry&mdash;your query does not match any openings.
        </div>
    {/each}
    </CardGrid>
</Main>
<Footer>
    &copy; {(new Date()).getFullYear()}
</Footer>

<style>
    input {
        margin: 0 0 2rem 0;
    }
    .notice {
        text-align: center;
    }
</style>
