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
            color: cyan;
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

            await printText(`${name}, How may I assist you today?\n`, 50);
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

        intro();
    </script>
</body>
