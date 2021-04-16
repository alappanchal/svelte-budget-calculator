<script>
    // your script goes here
    import SectionTitle from "./Title.svelte";
    import Expense from "./Expense.svelte";
    import {fly} from "svelte/transition";
    import { flip } from "svelte/animate";
    // variables
    export let expenses = [];
</script>

<style>
    /* your styles go here */
</style>

<section>
    <SectionTitle title="Expense List" />
</section>

<ul>
    {#each expenses as expense,index(expense.id)  }
        <div class="se-container" 
            in:fly="{{x:200,delay:(index+1)*300}}"
            out:fly="{{x:-200,delay:(index+1)*300}}"
            animate:flip
        >
            <Expense {expense} />
        </div>
    {:else}
        <h2>No expenses added to the list</h2>
    {/each}
</ul>