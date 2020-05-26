<script></script>

<div class="card-grid">
    <slot></slot>
</div>

<style>
    .card-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
        grid-gap: 2rem;
    }
</style>