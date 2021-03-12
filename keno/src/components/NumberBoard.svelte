<script>
    import App from '../App.svelte';
    import Header from './Header.svelte';
    import NumberItem from './NumberItem.svelte';
    import Button from './Button.svelte';

    let selection = [];
    let message;
    let stake = "";

    const numberRange = new Array(80);
    numberRange.fill(1);

    let numbers = numberRange;

    function pickRandomNumber() {
        const min = 1;
        const max = 80;
        return min + Math.random() * (max - min);
    };

    function generateLuckyPickNumbers() {
        const picked = [];
        while(picked.length < 5) {
            const randomNumber = Math.round(pickRandomNumber());
            if(!picked.includes(`${randomNumber}`)) {
                picked.push(`${randomNumber}`);
            }
        }
        return picked;
    };

    function handleLuckyPickClick() {
        selection = generateLuckyPickNumbers();
        message = null;
    }

    function handleClearBoardClick() {
        selection = [];
        stake = "";
        message = null;
    }

    function handlePlaceBetClick() {
        if(selection.length > 0 && stake) {
            selection = [];
            stake = "";
            message = "You are a winner!!!";
        } else if(selection.length === 0) {
            message = "Please pick some numbers";
        } else if(!stake) {
            message = "Please add a stake amount"
        }
    }

    function handleNumberBoardClick() {
        message = null;
    }

    function handleStakeClick(amount) {
        stake = amount;
    }

    $: {
        const regexp = /^\d+(\.\d{1,2})?$/;
        if(stake && !regexp.test(stake)) {
            stake = parseFloat(stake).toFixed(2);
        }
    }

</script>

<section>
    <div class="number-board" on:click={handleNumberBoardClick}>
        {#each numbers as _ , index }
            <NumberItem
                bind:selection
                label={`${index + 1}`}
                color={index > 39 ? "red" : "blue"}
                ariaLabel={`Select ${index + 1}`}
            />
        {/each}
    </div>

    {#if message}
        <div class="section-wrapper">
            <div class="message">{message}</div>
        </div>
    {/if}

    <div class="section-wrapper">
        {#each [1, 2, 5, 10, 20] as amount }
            <Button buttonType="pound" ariaLabel={`£ ${amount}`} onClick={() => handleStakeClick(amount.toFixed(2))}>{amount}</Button>
        {/each}
        <input class="stake" type="number" step="0.01" placeholder="Stake amount" bind:value={stake}>
    </div>
    <div class="section-wrapper">
        <Button buttonType="lucky" ariaLabel="Lucky Pick" onClick={handleLuckyPickClick}>Lucky Pick</Button>
        <Button buttonType="clear" ariaLabel="Clear Board" onClick={handleClearBoardClick}>Clear Board</Button>
    </div>
    <div class="section-wrapper">
        <Button buttonType="primary" ariaLabel="Place Bet" onClick={handlePlaceBetClick}>Place Bet</Button>
    </div>
</section>

<style>
    .number-board {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        max-width: 50rem;
        margin: 0 auto;
        padding: 1rem;
    }
    .section-wrapper {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        max-width: 43.75rem;
        margin: 0 auto;
        padding: 0.5rem 1rem;
    }
    .message {
        font-size: 1.5rem;
        font-weight: 500;
        color: 333;
        margin-bottom: 0.5rem;
    }
    .stake {
        margin: 0 0.25rem 0.5rem 0.25rem;
        text-align: center;
        font-weight: bold;
    }
    ::-webkit-input-placeholder { /* Chrome/Opera/Safari */
        font-weight: normal;
    }
    ::-moz-placeholder { /* Firefox 19+ */
        font-weight: normal;
    }
    :-ms-input-placeholder { /* IE 10+ */
        font-weight: normal;
    }
    :-moz-placeholder { /* Firefox 18- */
        font-weight: normal;
    }
</style>
