<h1>Table Tennis Rating Calculator</h1>

<section class="ratings-section">
    <label>
        Player 1 Rating:
        <input type="number" bind:value={playerRatingA} min="0" />
    </label>
    
    <label>
        Player 2 Rating:
        <input type="number" bind:value={playerRatingB} min="0" />
    </label>
</section>

<p>Winner:</p>
<label>
    <input
        type="radio"
        name="winner"
        value={'A'}
        bind:group={winner}
    />
    Player 1
</label>

<label>
    <input
        type="radio"
        name="winner"
        value={'B'}
        bind:group={winner}
    />
    Player 2
</label>

{#if playerRatingA && playerRatingB && winner}
    <p>Point spread: {getPointSpread(playerRatingA, playerRatingB)}</p>
    <p>Player 1 new rating: {getNewRatings(playerRatingA, playerRatingB, winner).newPlayerRatingA}</p>
    <p>Player 2 new rating: {getNewRatings(playerRatingA, playerRatingB, winner).newPlayerRatingB}</p>
{/if}

<script lang="ts">
	let playerRatingA: number;
	let playerRatingB: number;
    let winner: string;

    function getPointSpread(playerRatingA: number, playerRatingB: number): number {
        return Math.abs(playerRatingA - playerRatingB);
    }

    function getRatingChange(playerRatingA: number, playerRatingB: number, expectedResult: boolean): number {
        const pointSpread = getPointSpread(playerRatingA, playerRatingB);

        if (between(pointSpread, 0, 12)) {
            return 8;
        } else if (between(pointSpread, 13, 37)) {
            return expectedResult ? 7 : 10;
        } else if (between(pointSpread, 38, 62)) {
            return expectedResult ? 6 : 13;
        } else if (between(pointSpread, 63, 87)) {
            return expectedResult ? 5 : 16;
        } else if (between(pointSpread, 88, 112)) {
            return expectedResult ? 4 : 20;
        } else if (between(pointSpread, 113, 137)) {
            return expectedResult ? 3 : 25;
        } else if (between(pointSpread, 138, 162)) {
            return expectedResult ? 2 : 30;
        } else if (between(pointSpread, 163, 187)) {
            return expectedResult ? 2 : 35;
        } else if (between(pointSpread, 188, 212)) {
            return expectedResult ? 1 : 40;
        } else if (between(pointSpread, 213, 237)) {
            return expectedResult ? 1 : 45;
        } else {
            return expectedResult ? 0 : 50;
        }
    }

    function getNewRatings(playerRatingA: number, playerRatingB: number, winner: string) {
        const expectedResult: boolean = ((playerRatingA > playerRatingB) && winner === 'A') || ((playerRatingA < playerRatingB) && winner === 'B');
        console.log(playerRatingA, playerRatingB, winner, expectedResult);

        const ratingChange = getRatingChange(playerRatingA, playerRatingB, expectedResult);

        if (winner === 'A') {
            return {
                newPlayerRatingA: playerRatingA + ratingChange,
                newPlayerRatingB: playerRatingB - ratingChange
            };
        } else {
            return {
                newPlayerRatingA: playerRatingA - ratingChange,
                newPlayerRatingB: playerRatingB + ratingChange
            };
        }
    }

    function between(a: number, b: number, c: number): boolean {
        return Math.min(b, c) <= a && a <= Math.max(b, c);
    }
</script>

<style lang="scss">
    .ratings-section {
        display: flex;
        flex-direction: column;

        > label {
            margin-bottom: 1em;
        }
    }
</style>
