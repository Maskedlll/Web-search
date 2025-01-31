<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mini Navigateur Web</title>
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background: #1e1e1e;
            color: white;
            display: flex;
            flex-direction: column;
            height: 100vh;
        }
        header {
            display: flex;
            padding: 10px;
            background: #333;
            align-items: center;
            gap: 10px;
        }
        button {
            background: #575fcf;
            color: white;
            border: none;
            padding: 8px;
            cursor: pointer;
            border-radius: 5px;
        }
        button:disabled {
            background: #555;
            cursor: not-allowed;
        }
        input {
            flex: 1;
            padding: 8px;
            border-radius: 5px;
            border: none;
            font-size: 16px;
            outline: none;
            text-align: center;
        }
        iframe {
            flex: 1;
            width: 100%;
            border: none;
        }
    </style>
</head>
<body>

    <header>
        <button id="backButton" disabled>⬅️</button>
        <button id="forwardButton" disabled>➡️</button>
        <input type="text" id="urlInput" placeholder="https://example.com">
        <button id="goButton">Go</button>
    </header>

    <iframe id="browserFrame" src="https://example.com"></iframe>

    <script>
        const backButton = document.getElementById("backButton");
        const forwardButton = document.getElementById("forwardButton");
        const urlInput = document.getElementById("urlInput");
        const goButton = document.getElementById("goButton");
        const browserFrame = document.getElementById("browserFrame");

        let historyStack = [];
        let currentIndex = -1;

        function navigateTo(url) {
            if (!url.startsWith("http://") && !url.startsWith("https://")) {
                url = "https://" + url;
            }
            browserFrame.src = url;

            if (currentIndex === historyStack.length - 1) {
                historyStack.push(url);
                currentIndex++;
            } else {
                historyStack = historyStack.slice(0, currentIndex + 1);
                historyStack.push(url);
                currentIndex++;
            }

            updateButtons();
        }

        function updateButtons() {
            backButton.disabled = currentIndex <= 0;
            forwardButton.disabled = currentIndex >= historyStack.length - 1;
        }

        goButton.addEventListener("click", () => {
            const url = urlInput.value.trim();
            if (url) navigateTo(url);
        });

        urlInput.addEventListener("keypress", (event) => {
            if (event.key === "Enter") {
                const url = urlInput.value.trim();
                if (url) navigateTo(url);
            }
        });

        backButton.addEventListener("click", () => {
            if (currentIndex > 0) {
                currentIndex--;
                browserFrame.src = historyStack[currentIndex];
                urlInput.value = historyStack[currentIndex];
                updateButtons();
            }
        });

        forwardButton.addEventListener("click", () => {
            if (currentIndex < historyStack.length - 1) {
                currentIndex++;
                browserFrame.src = historyStack[currentIndex];
                urlInput.value = historyStack[currentIndex];
                updateButtons();
            }
        });
    </script>

</body>
</html>
