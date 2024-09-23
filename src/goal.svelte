<script>
    import { fly } from 'svelte/transition';
    import { onMount, onDestroy } from 'svelte';
    import vegaEmbed from 'vega-embed';

    let newGoal = '';
    let goals = [];
    let notification = '';

    // Predefined goal ideas
    const goalIdeas = [
        'Watch at least one movie per week',
        'Watch a comedy movie this week',
        'Watch a foreign language movie this week',
        'Finish the movie I started this morning',
        'Watch a horror movie this month',
        'Watch the newly released Telugu movie'
    ];

    // Goal status constants
    const GOAL_STATUS = {
        NOT_STARTED: 'Not Started',
        IN_PROGRESS: 'In Progress',
        COMPLETED: 'Completed'
    };

    // Add new goal
    function addGoal() {
        const trimmedGoal = newGoal.trim();
        if (trimmedGoal === '') {
            alert("Goal cannot be empty!");
            return;
        }
        if (!goals.some(goal => goal.name === trimmedGoal)) {
            goals = [...goals, { name: trimmedGoal, status: GOAL_STATUS.NOT_STARTED }];
            updateChart();
            newGoal = '';
        } else {
            alert("This goal already exists!");
        }
    }

    // Remove goal
    function removeGoal(index) {
        goals = goals.filter((_, i) => i !== index);
        updateChart();
    }

    // Mark goal as in progress
    function startProgress(index) {
        if (goals[index].status === GOAL_STATUS.NOT_STARTED) {
            goals[index].status = GOAL_STATUS.IN_PROGRESS;
            updateChart();
        }
    }

    // Mark goal as completed
    function completeGoal(index) {
        if (goals[index].status === GOAL_STATUS.IN_PROGRESS) {
            goals[index].status = GOAL_STATUS.COMPLETED;
            notification = `🎉 Goal "${goals[index].name}" completed!`;
            setTimeout(() => (notification = ''), 3000);
            updateChart();
        }
    }

    // Calculate counts
    $: totalGoals = goals.length;
    $: completedGoals = goals.filter(goal => goal.status === GOAL_STATUS.COMPLETED).length;
    function updateChart() {
        const totalGoals = goals.length;
        const completedGoals = goals.filter(goal => goal.status === GOAL_STATUS.COMPLETED).length;

        const data = [
            { status: 'Total Goals', count: totalGoals },
            { status: 'Completed Goals', count: completedGoals }
        ];

        const spec = {
            "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
            "data": { "values": data },
            "mark": "bar",
            "encoding": {
                "x": {
                    "field": "status",
                    "type": "ordinal"
                },
                "y": {
                    "field": "count",
                    "type": "quantitative"
                },
                "color": { value: "#d0ba1c" },
            }
        };

        // @ts-ignore
        vegaEmbed('#chart1', spec).catch(console.error);
    }

    onMount(() => {
        updateChart(); // Initial chart setup
    });
</script>


<div class="goal">
    <h2>Add a Goal</h2>
    <input
        type="text"
        bind:value={newGoal}
        placeholder="Type your goal here"
        aria-label="Goal input"
        class="goalInput"
    />
    <button class="normalButton goal_button" on:click={addGoal}>ADD GOAL</button>

    <h3>Goal Ideas</h3>
    <ul>
        {#each goalIdeas as idea}
            <li>
                <button class="normalButton"on:click={() => { newGoal = idea; }}>ADD "{idea.toUpperCase()}"</button>
            </li><br>
        {/each}
    </ul>

    <h3>Your Goals</h3>
    <ul class="goal-list">
        {#each goals as goal, index (goal.name)}
            <li class="goal-item {goal.status === GOAL_STATUS.COMPLETED ? 'completed' : (goal.status === GOAL_STATUS.IN_PROGRESS ? 'in-progress' : '')}" in:fly={{ y: 20, duration: 300 }}>
                {goal.name} ({goal.status})
                <div>
                    {#if goal.status === GOAL_STATUS.NOT_STARTED}
                        <button class="normalButton"on:click={() => startProgress(index)}>START PROGRESS</button>
                    {/if}
                    {#if goal.status === GOAL_STATUS.IN_PROGRESS}
                        <button class="normalButton"on:click={() => completeGoal(index)}>COMPLETE</button>
                    {/if}
                    <button class="normalButton"on:click={() => removeGoal(index)}>REMOVE</button>
                </div>
            </li>
        {/each}
    </ul>

    

    {#if notification}
        <div class="notification" in:fly={{ y: 100, duration: 400 }}>
            {notification}
        </div>
    {/if}
    <h3> Goals Overview</h3>
    <div id="chart1"></div>
</div>


<style>
    .goal-list {
     list-style-type: none;
     padding: 0;
 }
 .goal-item {
     display: flex;
     justify-content: space-between;
     margin: 5px 0;
     align-items: center;
     transition: background-color 0.2s;
 }
 .goal-item.completed {
     text-decoration: line-through;
     color: green;
     background-color: #e0ffe0;
 }
 .goal-item.in-progress {
     font-weight: bold;
     color: orange;
 }
 .notification {
     margin: 10px 0;
     background-color: lightgreen;
     padding: 10px;
     border-radius: 5px;
     text-align: center;
 }
 .goal{
  align-content: center;
  margin-left: 30px;
  margin-right: 20px;
  min-height:600px;
  width:95%;
  font-size: large;
  font-family:cursive;
  color: black;
  background-color: white;
  border-radius: 50px;
  border-color: black;
  border-width: 30px;
  position: relative;
  font-weight: bold;
  padding: 10px;
  padding-bottom: 20px;

}
.goal_button{
    border-color: none;
}
h2{
    text-align: center;
}
input{
  font-size: 20px;
  font-family: cursive;
}


</style>

