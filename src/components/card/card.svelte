<script>
</script>

<article class="card">
    <div class="card-header">
        <slot name="header"></slot>
    </div>
    <div class="card-body">
        <slot name="body"></slot>
    </div>
    <div class="card-footer">
        <slot name="footer"></slot>
    </div>
</article>

<style>
    .card {
        border: 1px solid var(--color-secondary);
        border-radius: 3px;
        overflow: hidden;
        transition: filter 250ms;
        filter: drop-shadow(0 0 0.25rem rgba(0, 0, 0, 0.15));
    }
    .card:hover {
        filter: drop-shadow(0 0 0.5rem rgba(0, 0, 0, 0.2));
    }
    .card-header {
        background-color: var(--color-secondary);
        color: var(--color-light);
        padding: 0.5rem;
    }
    .card-body {
        flex: 1;
        /*padding: 1rem;*/
        background-color: #fff;
        border-bottom-width: 1px;
        border-bottom-style: solid;
        border-bottom-color: var(--color-secondary);
    }
    .card-footer {
        padding: 0;
        background-color: #fff;
    }
</style>