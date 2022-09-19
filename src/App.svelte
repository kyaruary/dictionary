<script>
    import { onMount } from "svelte";
    import Alphabet from "./components/alphabet.svelte";
    import Card from "./components/card.svelte";
    import Col from "./components/col.svelte";
    import Container from "./components/container.svelte";
    import Graph from "./components/graph.svelte";
    import Header from "./components/header.svelte";
    import Item from "./components/item.svelte";
    import Row from "./components/row.svelte";
    import { randomWords } from "./lib/random-words";
    import qs from "qs";

    let words = JSON.parse(localStorage.getItem("words") || "[]");

    let shown = JSON.parse(localStorage.getItem("shown") || "{}");

    let selected = [];

    let value = "";

    $: total = words.length;
    $: shown_total = Object.keys(shown).filter((key) => shown[key] === true).length;
    $: reset_total = total - shown_total;

    onMount(() => {
        const query = qs.parse(location.search.split("?")[1]);
        if (!query.words) return;
        try {
            const _words = JSON.parse(query.words);
            const yes = window.confirm("是否覆盖本地的单词本");
            if (yes) {
                words = _words;
                selected = [];
                shown = {};
                localStorage.setItem("shown", "{}");
                localStorage.setItem("words", JSON.stringify(_words));
            }
            location.href = location.origin + location.pathname;
        } catch (e) {
            console.log(e);
        }
    });

    function add() {
        if (value && !words.includes(value)) {
            words = words.concat(value);
            localStorage.setItem("words", JSON.stringify(words));
            value = "";
        }
    }

    function remove(word) {
        words = words.filter((item) => item !== word);
        localStorage.setItem("words", JSON.stringify(words));
        if (shown[word]) {
            shown = Object.assign(shown, { word: undefined });
            delete shown[word];
            localStorage.setItem("shown", JSON.stringify(shown));
        }
    }

    function deleteAll() {
        const yes = window.confirm("确定删除所有的单词吗？");
        if (yes) {
            words = [];
            shown = {};
            localStorage.setItem("words", "[]");
            localStorage.setItem("shown", "{}");
        }
    }

    function random() {
        const exactly = words.length >= 3 ? 3 : words.length;
        selected = randomWords({ words, exactly });
        selected.forEach((key) => {
            shown[key] = true;
        });
        localStorage.setItem("shown", JSON.stringify(shown));
    }
</script>

<Header onDelete={deleteAll} />
<Container>
    <Card>
        <Row>
            <Col span={4}>
                <Graph {total} shown={shown_total} />
            </Col>
            <Col span={8}>
                <div class="overview">
                    <div style="font-weight: 500;">Overview</div>
                    <div style="font-size: 12px;font-weight: 700;display:flex;align-items:center;">
                        未出现 <span style="color:blueviolet ;">{reset_total}</span><span class="divider" />已出现
                        <span style="color: sandybrown;">{shown_total}</span><span class="divider" />总计 <span style="color: tomato;">{total}</span>
                    </div>
                    <div>
                        <button class="button" on:click={random}>随机三个单词</button>
                    </div>
                </div>
            </Col>
        </Row>
    </Card>
    <Row>
        <Col span={4}>
            <Card>
                <Alphabet word={selected[0]} />
            </Card>
        </Col>
        <Col span={4}>
            <Card><Alphabet word={selected[1]} /></Card>
        </Col>
        <Col span={4}>
            <Card><Alphabet word={selected[2]} /></Card>
        </Col>
    </Row>
    <Card>
        <Row>
            <Col span={8}><input type="text" class="input" placeholder="Words" bind:value /></Col>
            <Col span={4}><button class="button" on:click={add}>Add Words</button></Col>
        </Row>
    </Card>
    <Card>
        {#if words.length === 0}
            <div class="word-icon">
                <svg viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" width="48" height="48"
                    ><path
                        d="M769.6 121.63C754.21 52.15 692.2 0 618.18 0H408.79c-74.05 0-136.05 52.15-151.45 121.63C154.88 139.11 76.55 228.32 76.55 335.7v470.92c0 119.85 97.52 217.37 217.37 217.37h439.11c119.87 0 217.4-97.52 217.4-217.37V335.7c-0.01-107.38-78.34-196.59-180.83-214.07zM408.79 70.77h209.38c33.25 0 61.8 19.51 75.56 47.53H333.22c13.75-28.02 42.31-47.53 75.57-47.53z m293.14 118.31c-3.4 43.47-39.45 77.92-83.76 77.92H408.79c-44.33 0-80.38-34.45-83.78-77.92h376.92z m177.72 617.55c0 80.84-65.78 146.6-146.62 146.6H293.92c-80.84 0-146.6-65.75-146.6-146.6V335.7c0-67.06 45.3-123.62 106.87-141.01 6.23 79.92 73.11 143.08 154.6 143.08h209.38c81.47 0 148.35-63.15 154.58-143.08 61.58 17.38 106.89 73.94 106.89 141.01v470.93z"
                    /><path
                        d="M623.54 635.41c-5.45 35.03-11.37 70.98-16.82 107.36h-2.27c-7.72-36.37-15.02-72.33-22.74-107.36l-43.67-174.23h-46.4l-43.25 174.23c-7.72 35.49-15.44 71.45-22.74 107.36h-1.8L406.1 635.41l-31.38-174.23h-54.58l68.68 334.83h63.68l44.13-186.06c5.45-26.39 10.91-51.39 15.48-76.9h2.27c4.53 25.51 9.57 50.51 15.48 76.9l45.02 186.06h65.07l65.95-334.83h-50.51l-31.85 174.23z"
                    /></svg
                >
                Empty Word List.
            </div>
        {:else}
            {#each words as word}
                <Item text={word} onDelete={remove} />
            {/each}
        {/if}
    </Card>
</Container>

<style>
    .button {
        outline: none;
        border: none;
        background-color: tomato;
        color: #fff;
        border-radius: 8px;
        padding: 0 16px;
        height: 32px;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        font-weight: 700;
        transition: background-color 0.6s ease-out;
    }
    .button:active {
        background-color: rgba(255, 99, 71, 0.8);
        outline: 2px solid tomato;
    }

    .input {
        outline: none;
        border: none;
        border-radius: 0px;
        height: 100%;
        width: 100%;
        padding-left: 8px;
        box-sizing: border-box;
    }
    .overview > * {
        margin-bottom: 8px;
    }
    .divider {
        height: 12px;
        margin: 0 6px;
        background-color: #dfdfdf;
        width: 1px;
        display: inline-block;
    }
    .word-icon {
        display: flex;
        flex-flow: column nowrap;
        align-items: center;
        justify-content: center;
        height: 100%;
        color: #dfdfdf;
    }
    .word-icon svg {
        width: 68px;
        height: 68px;
        margin-bottom: 8px;
    }
    .word-icon svg path {
        fill: rgba(255, 99, 71, 0.3);
    }
</style>
