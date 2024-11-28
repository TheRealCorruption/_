
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secret Base Login</title>
    <style>
        body {
            background-color: black;
            color: white;
            font-family: serif;
            font-size: 18px;
            padding: 20px;
        }
        .console {
            white-space: pre-wrap;
            word-wrap: break-word;
        }
        .center {
            text-align: center;
        }
        .user-input {
            color: black;
            background-color: white;
            border: none;
            width: 100%;
            font-size: 18px;
            padding: 10px;
            margin-top: 10px;
            border-radius: 5px;
        }
        input::placeholder {
            color: gray;
        }
    </style>
</head>
<body>
    <div id="console" class="console center"></div>

    <script>
        const version = "1.00";
        const serialNumber = "RXAI-SN-001";
        const userDatabase = {
            "Csdjjckzm": "RXAITest"
        };
        let attemptsLeft = 3;
        let waitTime = 15;

        function printText(text, delay) {
            return new Promise(resolve => {
                let index = 0;
                let interval = setInterval(() => {
                    document.getElementById("console").innerText += text[index];
                    index++;
                    if (index === text.length) {
                        clearInterval(interval);
                        resolve();
                    }
                }, delay);
            });
        }

        function userInput() {
            return new Promise(resolve => {
                let inputField = document.createElement("input");
                inputField.setAttribute("type", "text");
                inputField.setAttribute("class", "user-input");
                inputField.setAttribute("placeholder", "Type here...");
                document.getElementById("console").appendChild(inputField);
                inputField.focus();

                inputField.addEventListener("keydown", (event) => {
                    if (event.key === "Enter") {
                        resolve(inputField.value);
                        inputField.remove();
                    }
                });
            });
        }

        async function login() {
            await printText("Welcome, I am RXAI your personal Assistance.\n", 50);

            await printText("Please type your username.\n", 50);
            let username = await userInput();

            if (userDatabase[username]) {
                await printText("Please type the password corresponding with this username.\n", 50);
                await checkPassword(username);
            } else {
                await printText("Invalid username, try again.\n", 50);
                await login();
            }
        }

        async function checkPassword(username) {
            let passwordTries = 0;

            while (passwordTries < 3) {
                let password = await userInput();

                if (userDatabase[username] === password) {
                    await printText(`Password correct! Welcome ${username}.\n`, 50);
                    return;
                } else {
                    attemptsLeft--;
                    if (attemptsLeft > 0) {
                        await printText(`Invalid password, ${attemptsLeft} tries remain.\n`, 50);
                    } else {
                        await printText(`Invalid password, 0 tries remain. Please wait ${waitTime} seconds and try again.\n`, 50);
                        await waitForCooldown();
                        attemptsLeft = 3;
                        waitTime += 15;
                        await login();
                    }
                }

                passwordTries++;
            }
        }

        async function waitForCooldown() {
            for (let i = waitTime; i > 0; i--) {
                await printText(`Cooldown time: ${i} seconds remaining.\n`, 50);
                await new Promise(resolve => setTimeout(resolve, 1000));
            }
        }

        login();
    </script>
</body>
</html>
