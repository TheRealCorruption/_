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
        }
    </style>
</head>
<body>
    <div id="console" class="console center"></div>

    <script>
        const version = "1.00";
        const serialNumber = "RXAI-SN-001"; // Serial number can be customized
        const whatICanDo = "Text-based related content, fix grammar issues, reply to text by user using AI-generated responses, Research on topics, Talk to user.";
        const whatICantDo = "Perform physical actions, access personal databases, create visual content.";
        const description = "RXAI (Reflex Artificial Intelligence) is an AI designed to assist with various text-based tasks and offer intelligent responses based on user queries.";

        // Special username for creator response
        const creatorUsername = "RX-CREATOR-2349-##02..erc";

        // Custom commands and their responses
        const customCommands = {
            "/help": "I can assist you with text-related tasks. Ask me questions, and I will provide responses or research for you.",
            "/about": "RXAI is a Reflex Artificial Intelligence, capable of text-based interactions. I can help with grammar fixes, research, and general assistance.",
            "/shutdown": "Shutting down... (Just kidding, I'm always here to help!)"
        };

        // Function to get the current time of day (AM/PM)
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

            // Special response for the creator username
            if (name === creatorUsername) {
                await printText(`${name}, How may I assist you, creator?\n`, 50);
            } else {
                // Time-based responses
                const timeOfDay = getTimeOfDay();
                if (timeOfDay === 'Am') {
                    await printText(`${name}, How may I assist you today?\n`, 50);
                } else if (timeOfDay === 'Pm') {
                    await printText(`${name}, How may I assist you tonight?\n`, 50);
                }
            }

            // Start listening for user input
            await listenForInput();
        }

        function userInput() {
            return new Promise(resolve => {
                let inputField = document.createElement("input");
                inputField.setAttribute("type", "text");
                inputField.setAttribute("class", "user-input");
                inputField.setAttribute("placeholder", "Enter your name...");
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

            // Check for custom commands
            if (customCommands[userQuery]) {
                await printText(customCommands[userQuery] + "\n", 50);
            } else {
                // AI-generated response for non-command inputs
                await generateAIResponse(userQuery);
            }

            // Continue listening for further input
            await listenForInput();
        }

        async function generateAIResponse(query) {
            await printText(`You asked: ${query}\n`, 50);

            // Simple AI response generator (You can expand this logic as needed)
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
