<script>
    import {score, round, time} from '../../store/store.js';

</script>

<header class="game-info comic-relief">
    <dl>
        <div class="score">
            <dt>Score</dt>
            <dd>{$score}</dd>
        </div>
        <div class="round">
            <dt>Round</dt>
            <dd>{$round}</dd>
        </div>
        <div class="time">
            <dt>Time</dt>
            <dd>{$time}</dd>
        </div>
    </dl>
</header>

<style lang="scss">
    .game-info {
        position: absolute;
        background: rgba(255, 255, 255, 0.5);
        left: 0;
        top: 0;
        width: 100%;

        dl {
            display: flex;
            justify-content: space-between;
            padding: 10px 30px;
            dt, dd {font-size: max(2vw, 24px);}

            .round {
                color: #fff;
                -webkit-text-stroke: 6px rgba(0, 0, 0, 1);
                paint-order: stroke fill;
            }

            .score, .time {
                color: #fff;
                text-shadow: 2px 2px #000;
            }
        }
    }
</style>