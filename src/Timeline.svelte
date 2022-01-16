<script>
	import * as d3 from 'd3';

    const spreadsheetUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTrjMuX6haFMe0H5K3WE62OPQo2cDIxOtBB28hk4KQj43uGu06PaYKtyZJrEqCu8A/pub?gid=317408256&single=true&output=csv';
    let locations = []
    let promise = loadData();

	async function loadData(){
		// d3.csv returns a promise. await "pauses" the execution untill the promise resolves
        locations = await d3.csv(spreadsheetUrl)
	}

    $:console.log(locations);
    // build an array of history from imported flat data
    $:{
        locations.forEach(val => {
            val.event_history = [val.Q42018, val.Q12019, val.Q22019, val.Q32019, val.Q42019];
        });
    }
</script>
<h1 style="display: flex; justify-content: center">Sapulpa,OK Dispensary Business Activity</h1>
{#await promise}
<p>Loading...</p>
{:then value}
<li class="heading">
    <ul class="locations location">
            <li class="q418">Q4 2018</li>
            <li class="q119">Q1 2019</li>
            <li class="q219">Q2 2019</li>
            <li class="q319">Q3 2019</li>
            <li class="q419">Q4 2019</li>
    </ul>
</li>

<ul class="locations">
	{#each locations as { company_name, location, event_history }, i}
		<li class="entry">
			<h2>{company_name}</h2>
            <h4 class="address">{location}</h4>
            <ul class="locations location">
                    {#each event_history as item, i}
                    <li 
                    class="quarter follow" 
                    style="grid-row: 1 / span 1; grid-column: {2 + i} / span 1">
                    {item}
                    </li>
                    {/each}
            </ul>
		</li>
	{/each}
</ul>
{:catch error}
<h4>Error finding data</h4>
{/await}

<style>
    /* add layout boxes and borders */
    .heading{
        padding: 2rem;
        margin: 1rem;
        list-style: none;
    }
    .entry{
        border: #000 solid 1px;
        padding: 2rem;
        margin: 1rem;
    }
    .locations{
        list-style: none;
        padding: 0;
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
    .location{
        display: grid;
        grid-template-columns: 300px 100px 100px 100px 100px 100px;
        grid-template-rows: 3fr 1fr;
    }
    .address{
        grid-row: 2 / span 1;
        grid-column: 1 / span 1;
    }
    .q418{
        grid-row: 1 / span 1;
        grid-column: 2 / span 1 ;
    }
    .q119{
        grid-row: 1 / span 1;
        grid-column: 3 / span 1 ;
    }
    .q219{
        grid-row: 1 / span 1;
        grid-column: 4 / span 1 ;
    }
    .q319{
        grid-row: 1 / span 1;
        grid-column: 5 / span 1 ;
    }
    .q419{
        grid-row: 1 / span 1;
        grid-column: 6 /span 1 ;
    }
</style>