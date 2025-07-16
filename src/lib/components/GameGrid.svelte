<script>
  import soundSrcClick from "../../assets/audio/click.mp3";
  import matchedSound from "../../assets/audio/match.mp3";
  import {score, round, time} from "../../store/store";
  import Modal from "./Modal.svelte";

  let clickSound = new Audio(soundSrcClick);
  let matchSound = new Audio(matchedSound);

     // 카드 데이터
  const card_data = [
    {
      id: 0,
      name: 'html',
      imgUrl: 'images/html.png',
    },
    {
      id: 1,
      name: 'css',
      imgUrl: 'images/css.png',
    },
    {
      id: 2,
      name: 'js',
      imgUrl: 'images/js.png',
    },
    {
      id: 3,
      name: 'react',
      imgUrl: 'images/react.png',
    },
    {
      id: 4,
      name: 'vue',
      imgUrl: 'images/vue.png',
    },
    {
      id: 5,
      name: 'svelte',
      imgUrl: 'images/svelte.png',
    },
    {
      id: 6,
      name: 'sass',
      imgUrl: 'images/sass.png',
    },
    {
      id: 7,
      name: 'github',
      imgUrl: 'images/github.png',
    },
    {
      id: 8,
      name: 'quest',
      imgUrl: 'images/quest.png',
    },
  ]

  // 카드 목록
  // id:카드번호, flipped: 뒤집혀진 상태, matched: 매칭된 상태
  let cards = [
    { id: 0, flipped: false, matched: false },
    { id: 0, flipped: false, matched: false },
    { id: 1, flipped: false, matched: false },
    { id: 1, flipped: false, matched: false },
    { id: 2, flipped: false, matched: false },
    { id: 2, flipped: false, matched: false },
    { id: 3, flipped: false, matched: false },
    { id: 3, flipped: false, matched: false },
    { id: 4, flipped: false, matched: false },
    { id: 4, flipped: false, matched: false },
    { id: 5, flipped: false, matched: false },
    { id: 5, flipped: false, matched: false },
    { id: 6, flipped: false, matched: false },
    { id: 6, flipped: false, matched: false },
    { id: 7, flipped: false, matched: false },
    { id: 7, flipped: false, matched: false },
  ]

  function shuffle() {
    isGameClear = false;
    isGameOver = false;
    cards = cards.sort(() => 0.5 - Math.random())
    cards.forEach((card) => {
      card.flipped = false,
      card.matched = false
    })
    startTimer();
  }

  function filpCard(i) {
    if(cards[i].flipped === false) {
      cards[i].flipped = true
    }

    setTimeout(() => {
      cards[i].flipped = false
    }, 1000)

    checkMatch(i)
  }

  let pairArr = [null, null];

  let isGameClear = false;

  function checkMatch(i) {
    if(pairArr[1] !== i) {
      pairArr.push(i);
      pairArr.shift();
    }
    
    if(cards[pairArr[0]]?.id === cards[pairArr[1]].id && cards[pairArr[0]].flipped) {
      cards[pairArr[0]].matched = true;
      cards[pairArr[1]].matched = true;
      $score += 100;
      matchSound.currentTime = 0;
      matchSound.pause();
      matchSound.play();
    }

    isGameClear = cards.every(card => card.matched === true);
  }

  let isGameOver = false;
  let timer;

  function startTimer() {
    timer = setInterval(() => {
      $time -= 1;
      if($time === 0) {
        isGameOver = true;
        clearInterval(timer);
      }
      if(isGameClear === true) {
        clearInterval(timer);
      }
    }, 1000-($round * 100))
  }

  shuffle();

</script>

<h1>Game Gird</h1>
<ul class="game-grid">
    {#each cards as card, i}
        <li class={card.flipped || card.matched === true ? 'card' : 'card hidden'}>
          <button on:click={() => {filpCard(i); clickSound.currentTime=0; clickSound.pause(); clickSound.play();}}>
            <img src={card_data[card.id].imgUrl} alt="">
          </button>
        </li>
    {/each}
</ul>

<!-- <button on:click={shuffle}>shuffle</button> -->

{#if isGameClear}  
  <Modal 
  shuffle={shuffle}
  modalTitle="Game Clear!"
  scoreTitle="Bonus score"
  btn1Text="Next"
   />
{/if}

 {#if isGameOver}
  <Modal 
  shuffle={shuffle}
  modalTitle="Game Over"
  scoreTitle="Final score"
  btn1Text="Replay"
  />
 {/if}

<style lang="scss">
    .game-grid {
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-template-rows: repeat(4, 1fr);
        list-style-type: none;
        gap: 2%;
        padding: 20px;

        button {
          background: transparent;
          border: none;
        }

        .card {
            position: relative;
            background: #fff;
            aspect-ratio: 1/1;
            padding: 20%;
            border-radius: 18%;
            img {
                width: 100%;
                object-fit: contain;
                display: block;
                aspect-ratio: 1/1;
            }
        }
    }

    .game-grid .card.hidden {
      background: rgba(0, 0, 0, 0.5);
      img {opacity: 0;}
      background-image: url('images/quest.png');
      background-repeat: no-repeat;
      background-size: 30%;
      background-position: center;
    }
</style>