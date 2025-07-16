<script>
    import { onDestroy } from "svelte";
    import GamePlay from "./GamePlay.svelte";
    import GameScore from "./GameScore.svelte";
    import {page} from "../../store/store.js";
    import bgm from "../../assets/audio/bgm.mp3";
    export let title;

    let _BGM = new Audio(bgm);
    
    function playBGM() {
        _BGM.loop = true;
        _BGM.play();
    }

    $: {
        if($page !== 'play') {
            _BGM.pause();
            _BGM.currentTime = 0;
        }
    }

    onDestroy(() => {
        _BGM.pause();
        _BGM.currentTime = 0;
        _BGM.remove();
    })
</script>

<main>
    {#if $page === "title"}
        <section class="comic-relief">
            <h1>{title}</h1>
            <div class="btn-group">
                <button class="btn" on:click={() => {$page = "play", playBGM();}}>Start</button>
                <button class="btn" on:click={() => $page = "score"}>Score</button>
                
            </div>
        </section>
        {:else if $page === 'play'}
        <GamePlay />
        {:else if $page === 'score'}
        <GameScore />
    {/if}
</main>

<style lang="scss">
    h1 {
        font-size: 16vw;
        color: #ff3e00;
        -webkit-text-stroke: 10px rgba(0,0,0, 0.5);
        paint-order: stroke fill;
    }

    .btn-group {
        display: flex;
        flex-direction: column;
        margin-top: 40px;
        button {
            background: transparent;
            border: none;
            butotn 
            &:hover {text-decoration: underline;}
        }
    }
</style>