<script>
    import {score, round, page, initTime, initScore} from '../../store/store.js';
    export let shuffle = function(){};
    export let modalTitle, scoreTitle, btn1Text = "";

    function setHiScore() {
        let hiScroe = 0;
        hiScroe = localStorage.getItem("hiScore") || 0;
        console.log(hiScroe);

        if($score > hiScroe) {
            localStorage.setItem("hiScore", $score);
        }

        localStorage.setItem("lastScore", $score);
    }
</script>

<div class="modal">
    <div class="modal-container">
        <h1>{modalTitle}</h1>
        <p class="score-title">{scoreTitle}</p>
        <p class="score">{$score}</p>
        <button
         class="btn next"
          on:click={() => {
            setHiScore();
            if(btn1Text === 'Next') {
                shuffle();
                $round += 1;
                initTime();
            } else {
                shuffle();
                initScore();
            }
          }}
          >
          {btn1Text}
        </button>
        <button
         class="btn"
         on:click={() => {
            setHiScore();
            $page = 'title';
            initScore();
         }}
         >
         Home
        </button>
    </div>
</div>

<style lang="scss">
    .modal {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        display: flex;
        align-items: center;
        justify-content: center;
        z-index: 100;
        display: grid;
        place-items: center;

        .modal-container {
            background: #fff;
            width: 400px;
            padding: 20px;
            border-radius: 30px;
            border: 10px solid #000;
            
            h1 {margin-bottom: 5px;}

            .score-title {
                font-size: 20px;
                color: #666;
            }

            .score {
                font-size: 36px;
                color: #f63030;
                margin-bottom: 20px;
            }
        }

        .btn {
            font-size: 24px;

            &.next {color:#f63030}
        }
    }
</style>