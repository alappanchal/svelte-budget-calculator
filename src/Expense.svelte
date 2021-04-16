<script>
    // your script goes here
    import { slide } from "svelte/transition";
    import { quintOut }  from "svelte/easing";
    import { getContext } from "svelte";
    
    export  let expense = {};  
    
    let displayAmount = false;
    function toggleAmount(){
        displayAmount = !displayAmount;
    }

    const removeExpense = getContext('remove'); //retriving the function from the top component.
    const modifyExpense = getContext('modify'); //retriving the function from the top component.   
    
</script>

<style>
    /* your styles go here */
</style>

<article class="single-expense">
    <div class="expense-info">
        <h2>
            {expense.name}
            <button class="amount-btn" on:click="{toggleAmount}">
                <i class="fas fa-caret-down"></i>
            </button>
        </h2>
        
        {#if displayAmount}
            <!-- 
                below is a way to control the transition on when adding/remvoing elements
                in:tranistionname = transition applys when the elements adds into DOM
                out:tranistionname = transition applys when the elements removes from DOM
            -->
            <!-- <h4 
                in:fly="{{x:-100,y:-100,duration:2000,delay:500,easing:quintOut}}"
                out:slide
            > -->
            <h4 transition:slide>
                Amount: ${expense.amount}
            </h4>    
        {/if}
        
    </div>
    <div class="expense-button">
        <button class="expense-btn edit-btn" on:click="{ modifyExpense(expense.id)}">
            <i class="fas fa-pen"></i>
        </button>
        <button class="expense-btn delete-btn" on:click="{ removeExpense(expense.id)}">
            <i class="fas fa-trash"></i>
        </button>
    </div>
</article>  