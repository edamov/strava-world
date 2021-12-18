<script>
	export let name;

	let result = '';
	let info = '';
	let token = null;
	let activities = [];
	let total = 0;

	const queryString = window.location.search;
	const urlParams = new URLSearchParams(queryString);
	if (urlParams.has('code')) {
		const code = urlParams.get('code');
		console.log(code);

		var xhr = new XMLHttpRequest();
		xhr.open("POST", 'https://www.strava.com/api/v3/oauth/token', true);

		xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");

		xhr.onload = function() {//Вызывает функцию при смене состояния.
			const obj = JSON.parse(xhr.response);
			if (!obj.hasOwnProperty('athlete')) {
				return;
			}
			info = obj.athlete.firstname + ' ' + obj.athlete.lastname
			token = obj.access_token;
			console.log(obj)

			var xhr2 = new XMLHttpRequest();
			xhr2.open("GET", 'https://www.strava.com/api/v3/athletes/' + obj.athlete.id  + '/stats?access_token=' + token, true);

			xhr2.onload = function () {
				const result = JSON.parse(xhr2.response);

				activities = {
					run: result.all_run_totals.distance,
					ride: result.all_ride_totals.distance,
					swim: result.all_swim_totals.distance,
					total: result.all_run_totals.distance + result.all_ride_totals.distance + result.all_swim_totals.distance
				};
			}

			xhr2.send();
		}
		xhr.send(`client_id=75539&client_secret=333336ab8d983b96baef6a6c0c36bfe7b0ef374b&code=${code}&grant_type=authorization_code`);
	// } else {
	//
	}

	function handleClick() {
		window.location = `http://www.strava.com/oauth/authorize?client_id=75539&response_type=code&redirect_uri=https://strava-world.vercel.app/&approval_prompt=force&scope=activity:read_all`;
	}


</script>

<main>
	<h1>Hello {name}!</h1>

	<button on:click={handleClick}>
		Connect to Strava
	</button>

	<pre>
{info}
</pre>
	<div>
		{#if activities.hasOwnProperty('total')}
		Сумарна дистанція: <b>{Math.round(activities.total)} метрів</b>
		<br>
		<br>

		Біг: {activities.run}
		<br>
		Вело: {activities.ride}
		<br>
		Плавання: {activities.swim}
		{/if}
		<br>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
