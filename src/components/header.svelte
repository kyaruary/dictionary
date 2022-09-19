<script>
    export let onDelete;
    function handleShare() {
        const query = `?words=${localStorage.getItem("words") ?? "[]"}`;
        window.navigator.clipboard.writeText(window.location.href + query).then(() => {
            alert("已复制你的单词本到粘贴板");
        });
    }
</script>

<div class="header">
    <svg class="delete icon" on:click={onDelete} viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" width="48" height="48"
        ><path d="M360 184h-8c4.4 0 8-3.6 8-8v8h304v-8c0 4.4 3.6 8 8 8h-8v72h72v-80c0-35.3-28.7-64-64-64H352c-35.3 0-64 28.7-64 64v80h72v-72z" /><path
            d="M864 256H160c-17.7 0-32 14.3-32 32v32c0 4.4 3.6 8 8 8h60.4l24.7 523c1.6 34.1 29.8 61 63.9 61h454c34.2 0 62.3-26.8 63.9-61l24.7-523H888c4.4 0 8-3.6 8-8v-32c0-17.7-14.3-32-32-32zM731.3 840H292.7l-24.2-512h487l-24.2 512z"
        /></svg
    >
    <svg on:click={handleShare} class="share icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" width="48" height="48"
        ><path
            d="M752 664c-28.5 0-54.8 10-75.4 26.7L469.4 540.8c1.7-9.3 2.6-19 2.6-28.8s-0.9-19.4-2.6-28.8l207.2-149.9C697.2 350 723.5 360 752 360c66.2 0 120-53.8 120-120s-53.8-120-120-120-120 53.8-120 120c0 11.6 1.6 22.7 4.7 33.3L439.9 415.8C410.7 377.1 364.3 352 312 352c-88.4 0-160 71.6-160 160s71.6 160 160 160c52.3 0 98.7-25.1 127.9-63.8l196.8 142.5c-3.1 10.6-4.7 21.8-4.7 33.3 0 66.2 53.8 120 120 120s120-53.8 120-120-53.8-120-120-120z m0-476c28.7 0 52 23.3 52 52s-23.3 52-52 52-52-23.3-52-52 23.3-52 52-52zM312 600c-48.5 0-88-39.5-88-88s39.5-88 88-88 88 39.5 88 88-39.5 88-88 88z m440 236c-28.7 0-52-23.3-52-52s23.3-52 52-52 52 23.3 52 52-23.3 52-52 52z"
        /></svg
    >
</div>

<style>
    .header {
        height: 48px;
        background-color: #fff;
        display: flex;
        align-items: center;
        justify-content: end;
        box-sizing: border-box;
        padding: 0 16px;
    }
    .icon {
        width: 24px;
        height: 24px;
    }
    .share path {
        fill: tomato;
    }
    .delete {
        margin-right: 16px;
    }
    .delete path {
        fill: #dfdfdf;
    }
</style>
