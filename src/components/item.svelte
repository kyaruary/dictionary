<script>
    export let text;
    export let onDelete;

    function handleDelte() {
        const yes = window.confirm(`确定删除${text}吗`);
        if (yes) {
            onDelete?.(text);
        }
    }
</script>

<div class="item">
    <span class="text">{text}</span>
    <svg on:click={handleDelte} class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" width="48" height="48"
        ><path
            d="M512 471.744l207.424-207.36a28.416 28.416 0 1 1 40.256 40.192L552.256 512l207.36 207.424a28.416 28.416 0 1 1-40.192 40.256L512 552.256l-207.424 207.36a28.416 28.416 0 1 1-40.256-40.192L471.744 512l-207.36-207.424a28.416 28.416 0 0 1 40.192-40.256L512 471.744z"
        /></svg
    >
</div>

<style>
    .item {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 6px 0;
        position: relative;
        padding-left: 16px;
    }
    .item::before {
        display: block;
        content: "";
        position: absolute;
        left: 0;
        bottom: 6px;
        top: 6px;
        width: 4px;
        /* background-color: blueviolet; */
        background-color: #dfdfdf;
    }
    /* .item::after {
        display: block;
        content: "";
        position: absolute;
        left: 0;
        right: 0;
        bottom: 0;
        height: 1px;
        background-color: #dfdfdf;
    } */

    .icon {
        width: 24px;
        height: 24px;
    }
    .icon path {
        fill: tomato;
    }
    .text {
        font-size: 18px;
    }
</style>
