# ademi-opme-sitesi
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ademi Öpme Sitesi</title>
    <style>
        :root {
            --background-color: #000;
            --text-color: #fff;
            --button-background-color: #ff6347;
            --button-hover-color: #e55347;
        }
        body {
            background-color: var(--background-color);
            color: var(--text-color);
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        header, footer {
            background-color: #333;
            padding: 20px;
            border-bottom: 1px solid #444;
        }
        main {
            padding: 20px;
        }
        h1, h2 {
            color: var(--button-background-color);
        }
        .site-name {
            font-size: 2em;
            margin: 20px 0;
        }
        .button {
            display: inline-block;
            padding: 10px 20px;
            font-size: 1.2em;
            color: var(--text-color);
            background-color: var(--button-background-color);
            border: none;
            border-radius: 5px;
            text-decoration: none;
            margin: 20px 0;
            cursor: pointer;
        }
        .button:hover {
            background-color: var(--button-hover-color);
        }
        .counter {
            font-size: 1.5em;
            margin: 20px 0;
        }
        .message {
            position: absolute;
            color: var(--button-background-color);
            font-size: 1.5em;
            font-weight: bold;
            white-space: nowrap;
            opacity: 0;
            transition: opacity 0.5s ease-in-out;
            z-index: 1000;
        }
        .fixed-message {
            color: var(--text-color);
            font-size: 1.2em;
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #444;
            border-radius: 5px;
            background-color: #333;
            text-align: center;
        }
        .theme-toggle {
            position: absolute;
            top: 10px;
            right: 10px;
            padding: 10px;
            background-color: var(--button-background-color);
            border: none;
            border-radius: 5px;
            color: var(--text-color);
            cursor: pointer;
        }
        .theme-toggle:hover {
            background-color: var(--button-hover-color);
        }
        .reward-popup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: var(--background-color);
            color: var(--text-color);
            padding: 20px;
            border: 2px solid var(--button-background-color);
            border-radius: 10px;
            z-index: 1001;
        }
        .close-popup {
            background-color: var(--button-background-color);
            border: none;
            color: var(--text-color);
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 20px;
        }
        .animation-container {
            position: absolute;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }
        .emoji-effect {
            position: fixed;
            font-size: 2em;
            z-index: 1000;
            animation: float 1.5s ease-in-out forwards;
        }
        @keyframes float {
            0% {
                transform: translateY(0);
                opacity: 1;
            }
            100% {
                transform: translateY(-100px);
                opacity: 0;
            }
        }
        .leaderboard {
            position: absolute;
            top: 10px;
            left: 10px;
            background-color: #333;
            border: 1px solid #444;
            border-radius: 5px;
            padding: 10px;
            text-align: left;
            color: var(--text-color);
        }
        .leaderboard h2 {
            margin: 0 0 10px 0;
            font-size: 1.2em;
            color: var(--button-background-color);
        }
        .leaderboard ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .leaderboard li {
            margin: 5px 0;
        }
    </style>
</head>
<body>
    <header>
        <div class="site-name">Ademi Öpme Sitesi</div>
    </header>
    <main>
        <h1>Ademi Öpme Sitesi</h1>
        <p>Hoş geldiniz! Bu sayfa tamamen <strong>Ademi Öpme Sitesi</strong> ile ilgilidir.</p>
        <h2>Ademi Öpme Sitesi</h2>
        <p>Burada sadece <strong>Ademi Öpme Sitesi</strong> hakkında bilgiler bulabilirsiniz.</p>
        <div class="leaderboard">
            <h2>En İyiler</h2>
            <ul id="leaderboardList"></ul>
        </div>
        <div>
            <label for="username">İsminiz: </label>
            <input type="text" id="username" required>
            <button class="button" id="startButton">Başla</button>
        </div>
        <button class="button" id="openButton" style="display:none;">Ademi Öp</button>
        <button class="button" id="quickModeButton" style="display:none;">Hızlı Öpücük Modu</button>
        <div class="counter">Öpme Sayacı: <span id="counter">0</span></div>
        <div class="fixed-message" id="fixedMessage"></div>
        <button class="theme-toggle" id="themeToggle">Karanlık/Aydınlık Mod</button>
        <div class="reward-popup" id="rewardPopup">
            <h2>Özel İfade</h2>
            <p id="rewardMessage"></p>
            <button class="close-popup" id="closePopup">Kapat</button>
        </div>
        <div class="animation-container" id="animationContainer"></div>
    </main>
    <footer>
        <div class="site-name">Ademi Öpme Sitesi</div>
    </footer>

    <script>
        var startButton = document.getElementById("startButton");
        var btn = document.getElementById("openButton");
        var quickModeButton = document.getElementById("quickModeButton");
        var counter = document.getElementById("counter");
        var fixedMessage = document.getElementById("fixedMessage");
        var themeToggle = document.getElementById("themeToggle");
        var rewardPopup = document.getElementById("rewardPopup");
        var closePopup = document.getElementById("closePopup");
        var animationContainer = document.getElementById("animationContainer");
        var leaderboardList = document.getElementById("leaderboardList");
        var usernameInput = document.getElementById("username");
        var count = 0;
        var username = '';
        var quickModeActive = false;

        var messages = [
            "Harikasın",
            "Mükemmelsin",
            "Dilin çok güzel",
            "Evet harika",
            "Böyle devam!",
            "Adem mutlu",
            "Ohh bebeğim",
            "Devam et",
            "Aşkımmm",
            "Çok sevdim aşkım",
            "Devam et aşkım",
            "Yavrum devam et",
            "Kedim devam et",
            "Kızım devam et",
            "Mutlu etmeye devam et",
            "Mutlu etmeye devam et",
            "Mutlu etmeye devam et",
            "Adem seni seviyor",
            "Adem çok mutlu",
            "Seni özlüyor",
            "Seninle gurur duyuyor",
            "Seninle hep birlikte",
            "Sana bayılıyor",
            "Sen harikasın"
        ];

        var fixedMessages = {
            5: "Adem öpmenize bayıldı",
            20: "Adem buna bayıldı",
            100: "Adem seni seviyor",
            150: "Karım olur musun?",
            200: "Artık karımsın",
            250: "Artık kaçışın yok, benimsin",
            300: "Çocuk vakti",
            350: "Adem çok mutlu, devam et",
            500: "Lütfen artık dur",
            1000: "Adem sadece senin, lütfen dur artık"
        };

        var rewardMessages = [
            "Adem size bir gül gönderdi",
            "Adem sizi çok seviyor",
            "Adem sizinle gurur duyuyor",
            "Adem sizi öpüyor",
            "Adem sizi kucaklıyor"
        ];

        var emojis = ["😊", "😍", "🥰", "😘", "💋", "😚", "❤️", "💕"];

        function showRandomMessage() {
            var message = messages[Math.floor(Math.random() * messages.length)];
            var messageElement = document.createElement("div");
            messageElement.className = "message";
            messageElement.innerText = message;

            messageElement.style.top = Math.random() * 80 + "vh";
            messageElement.style.left = Math.random() * 80 + "vw";
            messageElement.style.opacity = 1;

            animationContainer.appendChild(messageElement);

            setTimeout(function () {
                messageElement.style.opacity = 0;
                setTimeout(function () {
                    animationContainer.removeChild(messageElement);
                }, 500);
            }, 2000);
        }

        function showFixedMessage() {
            if (fixedMessages[count]) {
                fixedMessage.innerText = fixedMessages[count];
                fixedMessage.style.display = "block";
            } else {
                fixedMessage.style.display = "none";
            }
        }

        function showEmojiEffect() {
            var emoji = emojis[Math.floor(Math.random() * emojis.length)];
            var emojiElement = document.createElement("div");
            emojiElement.className = "emoji-effect";
            emojiElement.innerText = emoji;

            emojiElement.style.top = Math.random() * 80 + "vh";
            emojiElement.style.left = Math.random() * 80 + "vw";

            animationContainer.appendChild(emojiElement);

            setTimeout(function () {
                animationContainer.removeChild(emojiElement);
            }, 1500);
        }

        function toggleTheme() {
            if (document.body.style.backgroundColor === "black") {
                document.body.style.backgroundColor = "white";
                document.body.style.color = "black";
                themeToggle.innerText = "Karanlık Mod";
            } else {
                document.body.style.backgroundColor = "black";
                document.body.style.color = "white";
                themeToggle.innerText = "Aydınlık Mod";
            }
        }

        function startQuickMode() {
            quickModeActive = true;
            count = 0;
            counter.innerText = count;
            var quickModeDuration = 10000; // 10 seconds

            btn.style.display = "inline-block";
            quickModeButton.style.display = "none";

            setTimeout(function () {
                quickModeActive = false;
                btn.style.display = "none";
                quickModeButton.style.display = "inline-block";
                showRewardPopup();
            }, quickModeDuration);
        }

        function showRewardPopup() {
            var randomRewardMessage = rewardMessages[Math.floor(Math.random() * rewardMessages.length)];
            document.getElementById("rewardMessage").innerText = randomRewardMessage;
            rewardPopup.style.display = "block";
        }

        function closeRewardPopup() {
            rewardPopup.style.display = "none";
        }

        startButton.addEventListener("click", function () {
            username = usernameInput.value;
            if (username) {
                startButton.style.display = "none";
                btn.style.display = "inline-block";
                quickModeButton.style.display = "inline-block";
            }
        });

        btn.addEventListener("click", function () {
            count++;
            counter.innerText = count;
            showRandomMessage();
            showEmojiEffect();
            showFixedMessage();
        });

        quickModeButton.addEventListener("click", startQuickMode);

        themeToggle.addEventListener("click", toggleTheme);

        closePopup.addEventListener("click", closeRewardPopup);
    </script>
</body>
</html>

