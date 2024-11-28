<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RXAI</title>
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
            margin-bottom: 20px;
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
        const whatICanDo = "Text-based related content, fix grammar issues, reply to text by user using AI-generated responses, Research on topics, Talk to user, Solve math problems.";
        const whatICantDo = "Perform physical actions, access personal databases, create visual content.";
        const description = "RXAI (Reflex Artificial Intelligence) is an AI designed to assist with various text-based tasks and offer intelligent responses based on user queries.";

        const creatorUsername = "RX-CREATOR-2349-##02..erc";
        const customCommands = {
            "/help": "I can assist you with text-related tasks, solve math problems, and answer your questions.",
            "/about": "RXAI is a Reflex Artificial Intelligence, capable of text-based interactions. I can help with grammar fixes, research, math problems, and general assistance.",
            "/CreatorAbout": "RXAI is created by RX-CREATOR-2349, a sophisticated AI designed to assist with everyday text tasks and answer questions.",
            "/SaveChat": "You want to save this chat and continue later? 'yes' or 'no'?",
            "/LoadChat": "Please paste your 'RXSAVECODE'."
        };

        function getTimeOfDay() {
            const hours = new Date().getHours();
            return hours >= 12 ? 'Pm' : 'Am';
        }

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

        async function intro() {
            await printText(`RXAI, Version: ${version}, Model: ${serialNumber}\n\n`, 50);
            await printText("Hello, I am RXAI (Reflex Artificial Intelligence), I can, " + whatICanDo + " and here's what I cannot do, " + whatICantDo + "\n", 50);
            await printText(description + "\n\n", 50);
            await new Promise(resolve => setTimeout(resolve, 2000));

            await printText("What is your name?\n", 50);

            let name = await userInput();

            if (name === creatorUsername) {
                await printText(`${name}, How may I assist you, creator?\n`, 50);
            } else {
                const timeOfDay = getTimeOfDay();
                if (timeOfDay === 'Am') {
                    await printText(`${name}, How may I assist you today?\n`, 50);
                } else {
                    await printText(`${name}, How may I assist you tonight?\n`, 50);
                }
            }

            await listenForInput();
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

        async function listenForInput() {
            await printText("You can type any command or question.\n", 50);
            let userQuery = await userInput();

            if (customCommands[userQuery]) {
                await printText(customCommands[userQuery] + "\n", 50);
            } else {
                // Solve math problems or generate AI responses
                if (isMathEquation(userQuery)) {
                    await solveMathProblem(userQuery);
                } else {
                    await generateAIResponse(userQuery);
                }
            }

            await listenForInput();
        }

        function isMathEquation(query) {
            // Simple regex to check if the user input is a math equation
            const mathRegex = /^[\d\+\-\*\/\(\)\.^\s]+$/;
            return mathRegex.test(query);
        }

        async function solveMathProblem(query) {
            try {
                // Use the built-in `eval` function to solve simple math equations (Note: eval can be dangerous, so avoid user input from untrusted sources)
                const result = eval(query);
                await printText(`The result of your math problem "${query}" is: ${result}\n`, 50);
            } catch (error) {
                await printText("Sorry, I couldn't solve that equation.\n", 50);
            }
        }

        async function generateAIResponse(query) {
            await printText(`You asked: ${query}\n`, 50);

            const responses = [
                "That's an interesting question!",
                "Let me think about that for a moment...",
                "I don't have a definite answer, but I'll try to help.",
                "Hmm... I need to look up more details on that.",
                "Could you clarify what you mean by that?"
            ];

            const randomResponse = responses[Math.floor(Math.random() * responses.length)];
            await printText(randomResponse + "\n", 50);
        }

        intro();
    </script>
</body>
</html>

