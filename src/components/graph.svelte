<script>
    import { onMount } from "svelte";
    export let total = 0;
    export let shown = 0;

    /**
     * @type { HTMLCanvasElement}
     */
    let canvas;

    /**
     * @type { CanvasRenderingContext2D }
     */
    let ctx;

    $: {
        if (ctx) {
            draw(total, shown);
        }
    }

    onMount(() => {
        canvas.height = 88;
        canvas.width = 88;
        ctx = canvas.getContext("2d");
        ctx.translate(44, 44);
        draw(total, shown);
    });

    function draw(total, shown) {
        ctx.clearRect(0, 0, 88, 88);

        ctx.beginPath();
        ctx.fillStyle = "tomato";
        ctx.arc(0, 0, 44, 0, 2 * Math.PI);
        ctx.lineTo(0, 0);
        ctx.fill();
        ctx.closePath();

        const percentage = total === 0 ? 0 : shown / total;

        ctx.beginPath();
        ctx.fillStyle = "sandybrown";
        ctx.arc(0, 0, 44, 0, percentage * 2 * Math.PI);
        ctx.lineTo(0, 0);
        ctx.fill();
        ctx.closePath();
    }
</script>

<div class="container">
    <canvas bind:this={canvas} />
</div>

<style>
    .container {
        display: flex;
        width: 100%;
        height: 100%;
        align-items: center;
        justify-content: center;
    }
</style>
