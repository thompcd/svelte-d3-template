<script>
	import * as d3 from 'd3';

    // css vars
    let gridRowCount = 3;
    let gridColCount = 0;

    // data source loading
    const spreadsheetUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTrjMuX6haFMe0H5K3WE62OPQo2cDIxOtBB28hk4KQj43uGu06PaYKtyZJrEqCu8A/pub?gid=317408256&single=true&output=csv';
    let locations = []
    let promise = loadData();
	async function loadData(){
		// d3.csv returns a promise. await "pauses" the execution untill the promise resolves
        locations = await d3.csv(spreadsheetUrl)
	}

    // build an array of history from imported flat data
    $:{
        locations.forEach(val => {
            val.event_history = 
            [val.Q42018, 
            val.Q12019, val.Q22019, val.Q32019, val.Q42019, 
            val.Q12020, val.Q22020, val.Q32020, val.Q42020, 
            val.Q12021, val.Q22021, val.Q32021, val.Q42021, 
            val.Q12022];
        });
        locations.entry_titles =
            [
                'Q4 2018',
                'Q1 2019', 'Q2 2019', 'Q3 2019', 'Q4 2019',
                'Q1 2020', 'Q2 2020', 'Q3 2020', 'Q4 2020',
                'Q1 2021', 'Q2 2021', 'Q3 2021', 'Q4 2021',
                'Q1 2022'
            ];
    }
    // calculate number of css grid columns based on time entries
    $:{
        gridColCount = locations.entry_titles.length + 2;
    }
</script>
<header class="header headerGrid">
    <h1 class="title">Dispensary Business Health in Sapulpa</h1>
    <h3 class="subtitle">How many dispensaries is too many?</h3>
    <h4 class="timelineLabel">Date by annual quarters</h4>
    <ul class="legend unformatted-list">
        <li>⏳ - Application for permit</li>
        <li>🚫 - Application denied</li>
        <li>🎉 - Business opened</li>
        <li>🚀 - Expanded permits</li>
        <li>⁉ - Change in ownership</li>
        <li>☠ - Business closed</li>
    </ul>
</header>
{#await promise}
<main class="grid">
    <span style="justify-self: center; padding: 4rem;">Loading...</span>
</main>
{:then value}
<main class="grid">
    <ul class="flatGrid unformatted-list">
        {#each locations.entry_titles as title, i}
             <li class="sideways" style="grid-row: 1 / span 1; grid-column: {i} / span 1">{title}</li>
        {/each}
    </ul>

    {#each locations as { company_name, location, event_history }, i}
        <div class="entry subgrid">
            <h1 class="name">{company_name}</h1>
            <h3 class="address">{location}</h3>
            <ul class="unformatted-list location subgrid point">
                    {#each event_history as item, i}
                    <li 
                    class="quarter follow" 
                    style="grid-row: 1 / span 1; grid-column: {i} / span 1">
                    {item === '' ? '|' : item}
                    </li>
                    {/each}
            </ul>
        </div>
    {/each}
    </main>
{:catch error}
<main class="grid">
    <span style="justify-self: center; padding: 4rem;">Error finding data</span>
</main>
{/await}

<style>
    /* top bar */
    .header{
        position: relative;
        height: 10rem;
        background-image: linear-gradient(#18993b, #037321);
        border-bottom-left-radius: 50% 20%;
        border-bottom-right-radius: 50% 20%;
        padding: 1rem 1rem 0 1rem;
    }
    .headerGrid{
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(3, 3fr);
    }
    .title{
        color: white;
        grid-row: 1 / span 1;
        grid-column: 1 / span 2;
        margin: 0;
    }
    .subtitle{
        color: white;
        grid-row: 2 / span 1;
        grid-column: 1 / span 2;
        margin: 0;
        justify-content: start;
    }
    .timelineLabel{
        color: white;
        grid-row: 3 / span 1;
        grid-column: 2 / span 1;
        justify-self: center;
        font-size: 1rem;
    }
    .legend{
        color: white;
        grid-row: 1 / span 4;
        grid-column: 3 / span 1;
        margin: 0;
        justify-self: end;
    }
    .sideways{
        transform: rotate(-90deg);
        margin-bottom: 2rem;
    }
    .unformatted-list{
        list-style: none;
        padding: 0;
        margin-left: 0;
    }

    /* layout timeline event labels */
    .flatGrid{
        display: grid;
        grid-template-columns: repeat(var(--gridColCount), 6rem);
        grid-template-rows: 3rem;
    }

    /* add layout boxes and borders */
    .grid{
        display: grid;
        grid-template-columns: repeat(var(--gridColCount), 6rem);
        grid-template-rows: repeat(var(--gridRowCount), 3rem);
        grid-row-gap: 1rem;
        padding: 1rem;
        padding-top: 0;
    }

    /* a subgrid entry should re-use the columns but have a different row structure */
    .subgrid{
        display: grid;
        grid-template-columns: repeat(var(--gridColCount), 6rem);
        grid-template-rows: repeat(4, 3rem);
    }
    .entry{
        border: #000 solid 1px;
        padding: 2rem;
    }

    /* icon size */
    .quarter{
        font-size: 2.5rem;
    }
    
    /* draw a line after the entries */
    .follow{
        display: flex;
        align-items: center;
    }
    .follow::after{
        content: '';
        flex: 1;
        height: 1px;
        background-color: #000;
    }
    
    /* align the items according to date, evenly spaced  */
    .name{
        grid-row: 1 / span 1;
        grid-column: 1 / span 1;  
    }
    .address{
        grid-row: 2 / span 1;
        grid-column: 1 / span 1;
    }
</style>