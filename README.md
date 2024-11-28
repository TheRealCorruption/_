<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RX</title>
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
        #commandBox {
            width: 100%;
            padding: 10px;
            font-size: 16px;
            border: 2px solid #555;
            margin-top: 20px;
            background-color: black;
            color: white;
            border-radius: 5px;
            resize: none;
        }
        .footer {
            margin-top: 20px;
            text-align: center;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <!-- Always display version and serial number at the top -->
    <div class="footer">
        <p id="version-info">Version: 1.00 | Model: RXAI-SN-001</p>
    </div>

    <div id="console" class="console center"></div>

    <script>
        const version = "1.00";
        const serialNumber = "RXAI-SN-001";
        const userDatabase = {
            "Wiralm": { password: "vuCdszAepmJ7W0c", permissionLevel: 1000 },
            "guest": { password: "UserGuest124", permissionLevel: 10 }
        };

        let attemptsLeft = 3;
        let waitTime = 15;
        let username = "";
        let permissionLevel = 0;
        let isLoggedIn = false;

        // Display initial version and serial number
        document.getElementById("version-info").innerHTML = `Version: ${version} | Model: ${serialNumber}`;

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
            await printText("Welcome, I am RXAI your text assistance,\n", 50);

            await printText("Please type your username.\n", 50);
            username = await userInput();

            if (userDatabase[username]) {
                permissionLevel = userDatabase[username].permissionLevel;
                await printText("Please type the password corresponding with this username.\n", 50);
                await checkPassword();
            } else {
                await printText("Invalid username, try again.\n", 50);
                await login();
            }
        }

        async function checkPassword() {
            let passwordTries = 0;

            while (passwordTries < 3) {
                let password = await userInput();

                if (userDatabase[username].password === password) {
                    await printText(`Password correct! Welcome ${username}.\n`, 50);
                    isLoggedIn = true;
                    return showCommands();
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

        async function showCommands() {
            // Clear previous text and reset
            document.getElementById("console").innerText = "";

            await printText("Here are the commands I can prompt to you. If you have any questions, feel free to ask. However, I do not know everything, and I will only answer with the generated responses I am able to use.\n", 50);
            
            let textBox = document.createElement("textarea");
            textBox.setAttribute("id", "commandBox");
            textBox.setAttribute("placeholder", "Type your command here (max 350 characters)");
            document.body.appendChild(textBox);

            textBox.addEventListener('input', function() {
                if (this.value.length > 350) {
                    this.value = this.value.substring(0, 350);
                }
            });

            await printText("\nCommands:\n/AboutRXAI\n//It will run the About Response\n/Help\n//It will preview commands and other important information\n", 50);

            const handleCommand = async (command) => {
                if (command === "/AboutRXAI") {
                    await printText("RXAI is your AI assistance created by the creator of this foundation, and will be used for various tasks and functions.\n", 50);
                } else if (command === "/Help") {
                    await printText("Commands:\n/AboutRXAI - Run the About Response\n/Help - Preview available commands\n", 50);
                } else if (command.toLowerCase().includes("can i see the database")) {
                    await showDatabaseAccess();
                } else {
                    await printText("I'm sorry, I couldn't quite understand that.\n", 50);
                }
            };

            // Wait for input after displaying commands
            textBox.addEventListener("keydown", async (event) => {
                if (event.key === "Enter") {
                    let command = textBox.value.trim();
                    textBox.value = "";
                    await handleCommand(command);
                }
            });
        }

        async function showDatabaseAccess() {
            if (permissionLevel >= 100) {
                await printText("Checking permission levels...\n", 50);
                await printText("Yes, here is a list of our available database files, if you wish to access them, please type 'AccessFile{FileNumber}' to view the report, or you can type 'Exit' and we can leave the database and continue with normal conversations.\n", 50);
                let textBox = document.createElement("input");
                textBox.setAttribute("type", "text");
                textBox.setAttribute("class", "user-input");
                textBox.setAttribute("placeholder", "Type 'Exit' to leave or 'AccessFile{FileNumber}' to access.");
                document.body.appendChild(textBox);

                textBox.addEventListener('keydown', async (event) => {
                    if (event.key === "Enter") {
                        if (textBox.value.toLowerCase() === "exit") {
                            await printText("Understood.\n", 50);
                            document.body.innerHTML = '';
                            showCommands();
                        } else if (textBox.value.toLowerCase().startsWith("accessfile")) {
                            await printText(`Accessing File: ${textBox.value}\n`, 50);
                        }
                    }
                });
            } else {
                await printText("Sorry, you do not have permission to view this.\n", 50);
            }
        }

        login();
    </script>
</body>
</html>
