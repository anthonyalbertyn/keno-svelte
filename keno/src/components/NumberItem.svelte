<script>
    // Component props

    export let label;
    export let color;
    export let selection;
    export let ariaLabel;

    // Component state

    let isActive;
    const maximum = 5;
    let wrapperClasses = "number-item";

    // Event handlers

    function handleNumberClick(selected) {
        if(selection.indexOf(selected) > -1) {
            selection = selection.filter((number) => number !== selected);
            isActive = false;
        } else if (selection.length === maximum) {
            return;
        } else {
            selection = [...selection, selected];
            isActive = true;
        }
    }

    // Runs on mount and then again when state that this
    // depends on changes
    
    $: {
        isActive = selection.indexOf(label) > -1;
        if(isActive) {
            wrapperClasses = "number-item green";
        } else if(color === "red") {
            wrapperClasses = "number-item red";
        } else {
            wrapperClasses = "number-item blue";
        }
    }
</script>

<div
    class={wrapperClasses}
    role="button"
    aria-label={ariaLabel}
    tabindex={0}
    on:click={() => handleNumberClick(label)}
    on:keyup={(event) => event.key === 'Enter' && handleNumberClick(label)}
>
    <div class="label">{label}</div>
</div>

<style>
    .number-item {
        font-family: "Exo 2";
        font-size: 2.5rem;
        line-height: 1;
        color: #fff;
        border: 1px solid #fff;
        width: 5.625rem;
        height: 3.75rem;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .number-item.red {
        background-color: #dd1e2f;
    }
    .number-item.blue {
        background-color: #06a2cb;
    }
    .number-item.green {
        background-color: #218559;
        font-size: 3.4375rem;
    }
    .number-item:hover {
        cursor: pointer;
        background-color: #218559;
    }
    .label {
        padding-bottom: 0.275rem;
    }
</style>