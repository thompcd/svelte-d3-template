<script>
	import * as d3 from 'd3';

    const spreadsheetUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTrjMuX6haFMe0H5K3WE62OPQo2cDIxOtBB28hk4KQj43uGu06PaYKtyZJrEqCu8A/pub?gid=317408256&single=true&output=csv';
    let locations = []
    let promise = loadData();

	async function loadData(){
		// d3.csv returns a promise. await "pauses" the execution untill the promise resolves
        locations = await d3.csv(spreadsheetUrl)
	}

    //run
    $:console.log(locations);
</script>

{#await promise}
<p>Loading...</p>
{:then value}
<li>
    <ul class="locations location">
            <li class="q418">Q4 2018</li>
            <li class="q119">Q1 2019</li>
            <li class="q219">Q2 2019</li>
            <li class="q319">Q3 2019</li>
            <li class="q419">Q4 2019</li>
    </ul>
</li>

<ul class="locations">
	{#each locations as { company_name, Q42018, Q12019, Q22019, Q32019, Q42019 }, i}
		<li>
			<h4>{company_name}</h4>
            <ul class="locations location">
                    <li class="q418">{Q42018}</li>
                    <li class="q119">{Q12019}</li>
                    <li class="q219">{Q22019}</li>
                    <li class="q319">{Q32019}</li>
                    <li class="q419">{Q42019}</li>
            </ul>
		</li>
	{/each}
</ul>
{:catch error}
<h4>Error finding data</h4>
{/await}

<style>
    .locations{
        list-style: none;
    }

    .location{
        display: grid;
        grid-template-columns: 300px 100px 100px 100px 100px 100px;
        grid-template-rows: 3fr 1fr;
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