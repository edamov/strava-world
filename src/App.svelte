<script>
	export let name;

	let result = null;

	const queryString = window.location.search;
	const urlParams = new URLSearchParams(queryString);
	if (urlParams.has('code')) {
		const code = urlParams.get('code');
		console.log(code);

		doPost();

	// 	curl -X POST https://www.strava.com/api/v3/oauth/token \
	// 			-d client_id=75539 \
  // -d client_secret=333336ab8d983b96baef6a6c0c36bfe7b0ef374b \
  // -d code=352f251097adc5f09513e56526b761e935051788 \
  // -d grant_type=authorization_code

		async function doPost () {
			const res = await fetch('https://www.strava.com/api/v3/oauth/token', {
				method: 'POST',
				body: JSON.stringify({
					client_id: 75539,
					client_secret: "333336ab8d983b96baef6a6c0c36bfe7b0ef374b",
					code: code,
					grant_type: "authorization_code"
				})
			})

			const json = await res.json()
			result = JSON.stringify(json)
		}

		// var xhr = new XMLHttpRequest();
		// xhr.open("POST", 'https://www.strava.com/api/v3/oauth/token', true);
	//
	// 	xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
	//
	// 	xhr.onreadystatechange = function() {//Вызывает функцию при смене состояния.
	// 		if(xhr.readyState == XMLHttpRequest.DONE && xhr.status == 200) {
	//
	// 		}
	// 		console.log(xhr.statusText)
	// 	}
	// 	xhr.send(`client_id=75539&client_secret=333336ab8d983b96baef6a6c0c36bfe7b0ef374b&code=${code}&grant_type=authorization_code`);
	// } else {
	//
	}

	function handleClick() {
		window.location = `http://www.strava.com/oauth/authorize?client_id=75539&response_type=code&redirect_uri=http://localhost:5000&approval_prompt=force&scope=activity:read_all`;
	}


</script>

<main>
	<h1>Hello {name}!</h1>

	<button on:click={handleClick}>
		Connect to Strava
	</button>

	<pre>
{result}
</pre>
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
