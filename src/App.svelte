<script>
	//component imports
	import Seo from './components/Seo.svelte'
	import Header from './components/Header.svelte'
	import Footer from './components/Footer.svelte'
	import Content from './components/Content.svelte'
	import Modal from './components/Modal.svelte'

	//module imports
	import { fly } from 'svelte/transition'
	import { quintOut } from 'svelte/easing'

	//dice logic
	const possibleSides = [ 6, 8, 10, 12, 20 ]
	let numberOfSides
	let sizeOfDice
	$: sizeOfDice = numberOfSides
	
	const createDice = (sizeOfDice) => {
		return Array.from({length: sizeOfDice}, (_, i) => i + 1)
	}

	//current dice roll
	let rollValue = 0
	let error
	const rollDice = (sizeOfDice) => {
		let dice = createDice(sizeOfDice);
			if (remainder === 0){
				error = false
				return rollValue = dice[Math.floor(Math.random() * dice.length)];
			} else { 
				error = true; 
        setTimeout(() => { error = false; }, 500);
			}
	}
	
	//create and store tasks
	//initialized with one task
	let tasks = [{id: 0, task: '', priority: 1, completed: false}]
	const addTask = () => { 
		let taskShape = {id: tasks.length, task: '', priority: remainder, completed: false}
		tasks.push(taskShape)
		tasks = tasks
	}
	const removeTask = (elem) => {
		tasks = tasks.filter((e) => {return e.id !== elem.id});
	};
	
	//calculate total priority of all tasks & the remainder value
	$: priorityTotal = tasks.map(a => a.priority).reduce((t, n) => t + n, 0);
	$: remainder = sizeOfDice - priorityTotal;
	
	//validating the difference between priority of all tasks & size of the dice
	let message = {
		color: '',
		text: ''
	};
	$: if (priorityTotal === sizeOfDice){ 
		message = {color: 'rgb(22, 120, 39)', text: ' '}
	} else if(priorityTotal === 0) {
		message = {color: ' ', text: ' '}
	} else {
		message = {color: 'rgb(200, 70, 70)', text: ' '}
	}
	
	//calculate range to display based on # of tasks
	let calcState;
	const calcRange = (task) => {
		let index = tasks.indexOf(task)
		let n1
		let n2
		
		if (index === 0) { 
			n1 = 1 
			n2 = task.priority 
			calcState = n2
		} else { 
			n1 = calcState + 1 
			n2 = calcState + task.priority
			calcState = n2
		}
		
		let range = `${n1}-${n2}`
		return range
	};
	
	//change task style when checkbox is checked
	$: setCompleted = (task) => {
		if(task.completed){
			return 50
		} else {
			return 100
		}
	}

	//dynamic styling for footer
	let ifTasksIsLong;
	$: if(tasks.length >= 4)
	{ ifTasksIsLong = true } 
	else { ifTasksIsLong = false}
</script> 

<Seo />
<Modal>
	<Content />
</Modal>
<Header />
<div id="grid">
	<div id="dice-grid">
		<div>
			<h1 on:click={() => rollDice(sizeOfDice)} tabindex=0>{ rollValue }</h1>
			<p>click # to roll</p>
		</div>
		<div>
			<label for="diceSize">Dice Size</label>
			<select bind:value={numberOfSides} name="diceSize" style="width:100%;">
				{#each possibleSides as option}
					{#if option === 20}
						<option value={option} selected="selected">{option}</option>
					{:else}
						<option value={option}>{option}</option>
					{/if}
				{/each}
			</select>
		</div>
	</div>
	<hr>
	<div id="sub-grid">
		<form>
			<div id="task-header">
				<div>
					<h3 style=color:{message.color} class:error={error}>{priorityTotal} of {sizeOfDice} <span>{message.text}</span></h3>
					<p>remainder: {remainder} </p>
				</div>
				<button on:click|preventDefault={addTask}> Add task </button>
			</div>
			<div id="tasks-container">
				{#each tasks.slice(0,sizeOfDice) as task}
					<div class="task" style="opacity:{setCompleted(task)}%; transition: all .2s ease-in-out 0s ;" transition:fly="{{delay: 25, duration: 250, x: 0, y: 50, opacity: 0.5, easing: quintOut}}">
						<input type="text" bind:value={task.task} />
						<select bind:value={task.priority}>
								<option>{0}</option>
							{#each createDice(sizeOfDice) as option}
								<option value={option}>{option}</option>
							{/each}
						</select>
						<button on:click|preventDefault={removeTask(task)}> - </button>
						{#if task.priority !== 0}
						<p style="display:inline;">
							{calcRange(task)}
						</p>
						<input type=checkbox bind:checked={task.completed}/>
						{/if}
					</div>
				{/each}
			</div>
		</form>
	</div>
</div>
<Footer { ifTasksIsLong }/>

<style>

	.error {
		-webkit-animation: bgcolorchange 4s; /* Chrome, Safari, Opera */ 
		animation: .85s bgcolorchange;
	}
	@keyframes bgcolorchange {
		0% {
			background-color: transparent;
		}
		50% {
			background-color: rgb(200, 70, 70);
			color: white;
		}
		100% {
			background-color: transparent;
		}
	}

	h1 {
		font-size: 3rem;
	}
	h1:hover {
		cursor: pointer;
	}
	#grid {
		margin: 0 auto;
		width: max-content;
		min-width: 250px;
		max-width: 90vw;
		margin-top: 3rem;
		margin-bottom: 6rem;
		z-index: 1;
		position: relative;
	}
	#dice-grid {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	#dice-grid h1, #dice-grid p {
		margin: 0;
		margin-bottom: .4rem;
	}
	#dice-grid p {
		font-size: .8rem;
	}
	#task-header {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	#task-header > button {
		height:35px;
	}
	.task {
		max-height: 35px;
		display: grid;
		grid-template-columns: minmax(132px, 185px) 2fr 1fr 43px 1fr;
		grid-template-rows: auto;
		gap: .75rem;
		align-items: center;
		margin-bottom: .75rem;
		max-width: 100%;
	}
	.task > * {
		max-height: 35px;
	}
	.task > select {
		min-width: 50px;
	}
	.task > input[type = text]:focus {
		transform: translateX(-5px);
		outline: none;
	}
	.task > input[type = checkbox] {
		appearance: none;
		height: 100%;
		width: 100%;
	}
	.task > input[type = checkbox]:checked {
		background:  url(../assets/close.svg);
		background-size: 60%;
		background-position: center;
		background-repeat: no-repeat;
		transition: all .2s ease-in-out 0s ;
	}
</style>