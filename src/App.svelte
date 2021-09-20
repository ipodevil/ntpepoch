<script>
	let timestamp = 0;
	const ntpDelta = parseInt(new Date('1900.01.01 00:00:00').getTime() / 1000);
	let delta = ntpDelta;
	$: timeString = new Date((timestamp + delta) * 1000).toLocaleString(undefined, {day: "2-digit", month: "2-digit", year: "numeric", hour: "numeric", minute: "numeric", second: "numeric"});
	let history = [];
	function insertCurrentTime() {
		timestamp = parseInt(new Date().getTime() / 1000) - delta;		
	}
	function addToHistory(item) {
		if(history.findIndex(i => i.input == item.input) == -1) {
			history = [...history, item];
		}		
	}
	$: addToHistory({input: timestamp, output:timeString});
</script>

<h1>
	NTP Timestamp Converter
</h1>
<label>Timestamp:
	<input type="number" bind:value={timestamp} />
	<button on:click={insertCurrentTime}>
		Now
	</button>
</label>

<div id="radio-btns">
	Epoch:
	<label>
		NTP
		<input type="radio" name="epoch" bind:group={delta} value={ntpDelta} checked="true"/>
	</label>
	<label>
		Unix
		<input type="radio" name="epoch" bind:group={delta} value={0}/>
	</label>
	
</div>


<p>
Corresponds to: {timeString}
</p>
<h2>History:</h2>
<table>
	<tr>
		<th>Timestamp</th>
		<th>String</th>
	</tr>
	{#each history as entry}
		<tr>
			<td on:click="{e => timestamp = entry.input}"><a role="button" href="javascript:undefined">{entry.input}</a></td>
			<td>{entry.output}</td>
		</tr>
	{/each}
</table>