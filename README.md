<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Link Fixed - Prank Maker</title>
    <style>
        body { font-family: 'Poppins', sans-serif; background: #fff5f7; margin: 0; display: flex; justify-content: center; align-items: center; min-height: 100vh; overflow: hidden; }
        .card { background: white; padding: 25px; border-radius: 20px; box-shadow: 0 15px 35px rgba(255, 71, 87, 0.1); width: 90%; max-width: 450px; text-align: center; z-index: 10; position: relative; }
        .hidden { display: none !important; }
        input { width: 100%; padding: 10px; margin: 8px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; }
        button { padding: 12px 25px; border: none; border-radius: 50px; cursor: pointer; font-weight: bold; transition: 0.3s; margin: 10px; }
        .btn-main { background: #ff4757; color: white; width: 100%; }
        #yes-btn { background: #2ed573; color: white; }
        #no-btn { background: #ff4757; color: white; position: fixed; z-index: 999; }
        .gif-display { width: 100%; max-height: 200px; object-fit: contain; margin-bottom: 15px; border-radius: 10px; }
        #link-box { background: #eee; padding: 10px; word-break: break-all; margin-top: 10px; font-size: 11px; border: 1px dashed #ff4757; cursor: pointer; }
        .heart { position: absolute; color: #ff4757; animation: float 3s linear infinite; pointer-events: none; }
        @keyframes float { 0% { transform: translateY(100vh); opacity: 1; } 100% { transform: translateY(-10vh); opacity: 0; } }
    </style>
</head>
<body>

    <div id="setup-window" class="card">
        <h2 style="color: #ff4757;">Prank Setup 🛠️</h2>
        <input type="text" id="bName" placeholder="Cheler Nam">
        <input type="text" id="gName" placeholder="Meye-r Nam">
        <input type="text" id="ques" placeholder="Question? (e.g. Amay bhalobaso?)">
        <input type="text" id="quesGif" placeholder="Question GIF Link (Direct URL)">
        <input type="text" id="yesMsg" placeholder="'Yes' bolle ki text asbe?">
        <input type="text" id="yesGif" placeholder="'Yes' GIF Link (Direct URL)">
        <button class="btn-main" onclick="generateLink()">Link Toiri Koro</button>
        <div id="link-area" class="hidden">
            <p style="font-size: 12px; color: red;">Nicher box-e click korle link copy hobe:</p>
            <div id="link-box" onclick="copyToClipboard()"></div>
        </div>
    </div>

    <div id="prank-window" class="card hidden">
        <img id="displayQuesGif" class="gif-display" src="" alt="GIF">
        <h3 id="displayNames"></h3>
        <h2 id="displayQues"></h2>
        <button id="yes-btn" onclick="showFinal()">Yes</button>
        <button id="no-btn">No</button>
    </div>

    <div id="final-window" class="card hidden">
        <img id="displayYesGif" class="gif-display" src="" alt="Success GIF">
        <h1 id="finalMsg" style="color: #2ed573;"></h1>
        <p>❤️ Mission Success ❤️</p>
    </div>

    <script>
        const noBtn = document.getElementById('no-btn');

        function generateLink() {
            const data = {
                b: document.getElementById('bName').value || "Someone",
                g: document.getElementById('gName').value || "Someone",
                q: document.getElementById('ques').value || "Amay bhalobaso?",
                qg: document.getElementById('quesGif').value || "https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExOHp6bXF6Znd6eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4JmVwPXYxX2ludGVybmFsX2dpZl9ieV9pZCZjdD1n/c76IJLufpNwSULPk77/giphy.gif",
                ym: document.getElementById('yesMsg').value || "I Love You Too!",
                yg: document.getElementById('yesGif').value || "https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExOHp6bXF6Znd6eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4JmVwPXYxX2ludGVybmFsX2dpZl9ieV9pZCZjdD1n/KZT3u9vI62zS8/giphy.gif"
            };
            
            // Base64 encoding
            const jsonStr = JSON.stringify(data);
            const encodedData = btoa(unescape(encodeURIComponent(jsonStr)));
            
            // Current page link + data hash
            const currentPath = window.location.href.split('#')[0];
            const fullLink = currentPath + "#data=" + encodedData;
            
            document.getElementById('link-box').innerText = fullLink;
            document.getElementById('link-area').classList.remove('hidden');
        }

        function copyToClipboard() {
            const linkText = document.getElementById('link-box').innerText;
            navigator.clipboard.writeText(linkText);
            alert("Link Copy Hoyeche!");
        }

        // URL check kora
        function checkUrl() {
            const hash = window.location.hash;
            if(hash.startsWith("#data=")) {
                try {
                    const base64 = hash.replace("#data=", "");
                    const decodedData = decodeURIComponent(escape(atob(base64)));
                    const data = JSON.parse(decodedData);
                    
                    document.getElementById('setup-window').classList.add('hidden');
                    document.getElementById('prank-window').classList.remove('hidden');
                    
                    document.getElementById('displayNames').innerText = `${data.b} ❤️ ${data.g}`;
                    document.getElementById('displayQues').innerText = data.q;
                    document.getElementById('displayQuesGif').src = data.qg;
                    document.getElementById('finalMsg').innerText = data.ym;
                    document.getElementById('displayYesGif').src = data.yg;
                    
                    setInterval(createHeart, 300);
                } catch(e) {
                    alert("Link-e vul ache!");
                }
            }
        }

        window.onload = checkUrl;

        function showFinal() {
            document.getElementById('prank-window').classList.add('hidden');
            document.getElementById('final-window').classList.remove('hidden');
            noBtn.style.display = 'none';
        }

        function moveButton() {
            const x = Math.random() * (window.innerWidth - 100);
            const y = Math.random() * (window.innerHeight - 50);
            noBtn.style.left = x + 'px';
            noBtn.style.top = y + 'px';
        }

        noBtn.addEventListener('mouseover', moveButton);
        noBtn.addEventListener('touchstart', (e) => { e.preventDefault(); moveButton(); });

        function createHeart() {
            const heart = document.createElement('div');
            heart.className = 'heart';
            heart.innerHTML = '❤️';
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.fontSize = (Math.random() * 20 + 10) + 'px';
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 3000);
        }
    </script>
</body>
</html>

