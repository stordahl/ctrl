<script>
	//component imports
	import Header from './components/Header.svelte'
	import Footer from './components/Footer.svelte'
	import Content from './components/Content.svelte'
  import Modal from './components/Modal.svelte'

	//module imports
	import { onMount } from 'svelte'
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
	const rollDice = (sizeOfDice) => {
		let dice = createDice(sizeOfDice);
		if (remainder === 0){
			return rollValue = dice[Math.floor(Math.random() * dice.length)];
		} else { alert('You can not roll until the remainder is 0!') }
	}
	
	//create and store tasks
	let tasks = [{id: 0, task: '', priority: 0, completed: false}]
	const addTask = () => { 
		let taskShape = {id: tasks.length, task: '', priority: 0, completed: false}
		tasks.push(taskShape)
		tasks = tasks
	}
	const removeTask = (elem) => {
		tasks = tasks.filter((e) => {return e.id !== elem.id});
	};
  
	$: priorityTotal = tasks.map(a => a.priority).reduce((t, n) => t + n, 0);
	
	$: remainder = sizeOfDice - priorityTotal;
	
	let message = {
		color: '',
		text: ''
	};
	$: if (priorityTotal === sizeOfDice){ 
		message = {color: 'green', text: 'success!'}
	} else if(priorityTotal === 0) {
		message = {color: ' ', text: ' '}
	} else {
		message = {color: 'red', text: 'error'}
	}
	
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
		
		let range = `${n1} - ${n2}`
		return range
	};
	
	$: setCompleted = (task) => {
		if(task.completed){
			return 50
		} else {
			return 100
		}
	}

	onMount(() => {
		const keyPress = (e) => {
    	if(e.key === "Escape") {
        document.getElementsByTagName("input").blur(); 
			}
		};  	
	});
</script> 

<Modal>
	<Content />
</Modal>
<Header />
<div id="grid">
	<div id="dice-grid">
		<div>
			<h1 on:click={() => rollDice(sizeOfDice)}>{ rollValue }</h1>
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
					<h3 style=color:{message.color}>{priorityTotal} of {sizeOfDice} <span>{message.text}</span></h3>
					<p>remainder: {remainder} </p>
				</div>
				<button on:click|preventDefault={addTask}> Add task </button>
			</div>
			<div id="tasks-container">
				{#each tasks as task}
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
<Footer />

<style>
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
		grid-template-columns: 8fr 2fr 1fr 2fr 1fr;
		grid-template-rows: auto;
		gap: 1rem;
		align-items: center;
		margin-bottom: .75rem;
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