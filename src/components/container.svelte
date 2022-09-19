<main class="container">
    <slot />
</main>

<style>
    .container {
        height: calc(100vh - 48px);
        overflow-y: scroll;
        padding: 16px 8px 0;
    }
</style>
