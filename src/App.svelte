<script>	
	// your script goes here
	// components
	import Navbar from "./Navbar.svelte";	
	//import expenseData from "./expenses.js";
	import ExpensesList from "./ExpensesList.svelte";
	import Totals from "./Totals.svelte";
	import ExpenseForm from "./ExpenseForm.svelte";
	import Modal from "./Modal.svelte";

	import { setContext } from "svelte";
	import { afterUpdate, beforeUpdate, onDestroy, onMount } from "svelte";

	// runs only once
    onMount( ()=>{		
		getLocalStorage();
        console.log ("App mounted");
    });

	// runs initiallly and everytime.
	afterUpdate( ()=>{
		setLocalStorage();
		console.log("afterUpdate");
	})
	
	// variables
	//let expenses = [...expenseData];
	let expenses = [];
	
	// set editing variables
	let currExpID = null;
	let currExpName = '';
	let currExpAmt = null;

	// toggel form variable
	let isFormOpen = false;

	// reactive
	$: isEditing = currExpID ? true : false ;
	
	$: total = expenses.reduce( (acc,curr) => {
		return acc += curr.amount;
	},0);

	function showForm(){
		isFormOpen = true;
	}

	function hideForm(){
		isFormOpen = false;
		currExpID = null;
		currExpName = '';
		currExpAmt = null;
	}

	// function
	function removeExpense(id){
		expenses = expenses.filter( item => item.id !== id );
		currExpID = null;
		currExpName='';
		currExpAmt=null;		
	}

	function clearExpenses(){
		expenses = [];		
	}

	function resetExpenses(){
		expenses = [];		
	}

	function addExpense(newExp){
		expenses = [newExp,...expenses];		
	}

	function editExpense(currentExp){		
		let name= currentExp.name;
		let amount = currentExp.amount;

		expenses = expenses.map( item =>{
			return item.id==currExpID ? {...item,name,amount} : {...item};
		});

		currExpID = null;
		currExpName='';
		currExpAmt=null;	
	}

	function setModifiedExpense(id){
		let currentExp = expenses.find( item => item.id==id );
		currExpID = currentExp.id;
		currExpName = currentExp.name;
		currExpAmt = currentExp.amount;
		showForm();
	}
	
	setContext('remove', removeExpense); // storing the function in context so it can be accessed in the deep down child component.
	setContext('modify', setModifiedExpense); // storing the function in context so it can be accessed in the deep down child component.

	function setLocalStorage(){
		localStorage.setItem("expenses",JSON.stringify(expenses));
	}

	function getLocalStorage(){
		let storedExpenses = localStorage.getItem('expenses');
		expenses = storedExpenses ? JSON.parse(storedExpenses) : [] ;
	}	
</script>

<Navbar {showForm} />
<main class="content">
	{#if isFormOpen}
		<Modal>
			<ExpenseForm 
				{editExpense} 
				{addExpense} 
				{hideForm} 
				name="{currExpName}" 
				amount="{currExpAmt}" 
				{isEditing}  
			/>
		</Modal>
	{/if}
	<Totals title="Total Expenses" {total} />
	<ExpensesList {expenses} />	
	<button class="btn btn-primary btn-block" on:click="{clearExpenses}">Clear Expenses</button><br />
	<button class="btn btn-primary btn-block" on:click="{resetExpenses}">Reset Expenses</button>
</main>