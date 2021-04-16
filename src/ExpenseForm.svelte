<script>
    //import { afterUpdate, beforeUpdate, onDestroy, onMount } from "svelte";
    
    // runs initially and everytime something is changed on the component, say changing input value
    /* beforeUpdate(() => {
        console.count ("Expense Form beforeUpdate");
    }) */

    // runs only once
    /* onMount( ()=>{
        console.log ("Expense Form mounted");
    }); */    

    // runs initially and everytime something is changed on the component, say changing input value
    /* afterUpdate(() =>{
        console.count ("Expense Form afterUpdate");
    }) */

    // runs only once
    /* onDestroy( ()=>{
        console.log ("Expense Form destroyed");
    }) */

    // receives from parent component
    export let addExpense;  // function
    export let editExpense; // function
    export let hideForm;    // variable

    // your script goes here
    import Title from "./Title.svelte";

    export let name = '';
    export let amount = null;
    export let isEditing;

    $: isEmpty = !name || !amount // form validation - isEmpty set to true if one of the field is empty or set to 0 (amount)
    
    function saveForm(){        
        // new expense details
        let newExp = {};      
        newExp.name = name;
        newExp.amount = amount; 
        if (isEditing){
            editExpense(newExp);
        }else{
            newExp.id = Math.random() * Date.now();
            addExpense(newExp); // received the function from the parent component, which is 'app.svelte' in this case.
        }
        
        hideForm();
        
        // clearing out the form fields
        clearForm();
    }

    function clearForm(){
        name='';
        amount='';
    }
    
</script>

<style>
    /* your styles go here */
</style>

<section class="form">
    <Title title="Add Expense" />
    <form class="expense-form" on:submit|preventDefault="{saveForm}">
        <div class="form-control">
            <label for="name">name</label>
            <input type="text" id="name" bind:value={name} />
        </div>
        <div class="form-control">
            <label for="amount">amount</label>
            <input type="number" id="amount" bind:value={amount} />
        <div>
        
        {#if isEmpty}
            <p class="form-empty">Please fill out all form fields</p>
        {/if}

        <button class="btn btn-block" type="submit" class:disabled="{isEmpty}" disabled="{isEmpty}">
            {#if isEditing }
                Edit Expense
            {:else}
                Add Expense
            {/if}
        </button>
        <button class="close-btn" type="button" on:click="{hideForm}">
            <i class="fas fa-times"></i>
            Close
        </button>
    </form>
</section>

