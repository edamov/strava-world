<script>
	export let name;

	var strava = require('strava-v3')
    strava.config({
      "access_token"  : "Your apps access token (Required for Quickstart)",
      "client_id"     : "Your apps Client ID (Required for oauth)",
      "client_secret" : "Your apps Client Secret (Required for oauth)",
      "redirect_uri"  : "Your apps Authorization Redirection URI (Required for oauth)",
    });
</script>

<main>
	<h1>Hello {name}!</h1>
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
