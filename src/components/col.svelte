<script>
    export let span;
    $: width = (span / 12) * 100 + "%";
</script>

<div class="col" style="width: {width};">
    <slot />
</div>

<style>
    .col {
        padding: 0 8px;
    }
</style>
