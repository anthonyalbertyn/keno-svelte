<script>
    // Component props

    export let ariaLabel;
    export let buttonType;
    export let onClick;

    // Component state
    
    let buttonClasses = {
        pound: "button pound",
        clear: "button clear",
        lucky: "button lucky",
        primary: "button primary",
    };

</script>

<div
    class={buttonClasses[buttonType]}
    role="button"
    tabindex={0}
    aria-label={ariaLabel}
    on:click={onClick}
    on:keyup={(event) => event.key === 'Enter' && onClick()}
>
    {#if buttonType === "pound"}
        <span class="icon fas fa-pound-sign" />
    {/if}
    {#if buttonType === "lucky"}
        <span class="icon fas fa-star" />
    {/if}
    {#if buttonType === "clear"}
        <span class="icon fas fa-trash" />
    {/if}
    <slot></slot>
</div>

<style>
    .button {
        line-height: 1;
        border-radius: 3px;
        background: #fff;
        border: 1px solid #d9d9d9;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 2rem;
        padding: 0 1.5rem;
        margin: 0 0.25rem 0.5rem 0.25rem;
        cursor: pointer;
        user-select: none;
    }
    .button:hover {
        color: #40a9ff;
        background: #fff;
        border-color: #40a9ff;
    }
    .button.primary {
        color: #fff;
        background: #1890ff;
        border-color: #1890ff;
        text-shadow: 0 -1px 0 rgb(0 0 0 / 12%);
        -webkit-box-shadow: 0 2px 0 rgb(0 0 0 / 5%);
        box-shadow: 0 2px 0 rgb(0 0 0 / 5%);
        padding: 0.25rem 4rem;
    }
    .button.primary:hover {
        color: #fff;
        background: #40a9ff;
        border-color: #40a9ff;
    }
    .button.pound {
        font-weight: bold;
    }
    .icon {
        padding-right: 0.25rem;
    }
</style>
