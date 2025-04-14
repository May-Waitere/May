# May
<!DOCTYPE html>
<html>
<head>
    <title>动物单词消消乐</title>
    <style>
        .container {
            text-align: center;
            padding: 20px;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin: 20px auto;
            max-width: 300px;
        }
        .cell {
            background: #4CAF50;
            color: white;
            height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            cursor: pointer;
            border-radius: 8px;
            transition: 0.3s;
        }
        .cell.selected {
            background: #FF9800;
            transform: scale(0.95);
        }
        #status {
            font-size: 20px;
            margin: 15px;
            height: 24px;
        }
        #qrcode {
            margin: 20px auto;
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🐾 动物单词消消乐 🦁</h1>
        <div id="status">找出：CAT</div>
        <div class="grid" id="grid"></div>
        <div id="qrcode"></div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/qrcodejs/qrcode.min.js"></script>
    <script>
        const animals = ['CAT', 'DOG', 'PIG', 'COW', 'FOX'];
        let currentLevel = 0;
        let targetWord = '';
        let selectedIndex = 0;

        function initGame() {
            targetWord = animals[currentLevel];
            selectedIndex = 0;
            document.getElementById('status').textContent = `找出：${targetWord}`;
            createGrid();
        }

        function createGrid() {
            const grid = document.getElementById('grid');
            grid.innerHTML = '';
            
            // 生成包含目标字母和随机字母的9宫格
            const letters = [...targetWord.split('')];
            while(letters.length < 9) {
                letters.push(String.fromCharCode(65 + Math.floor(Math.random()*26));
            }
            letters.sort(() => Math.random() - 0.5);

            letters.forEach((letter, index) => {
                const cell = document.createElement('div');
                cell.className = 'cell';
                cell.textContent = letter;
                cell.onclick = () => checkLetter(letter, index);
                grid.appendChild(cell);
            });
        }

        function checkLetter(letter, index) {
            const cells = document.getElementsByClassName('cell');
            
            if(letter === targetWord[selectedIndex]) {
                cells[index].classList.add('selected');
                selectedIndex++;
                
                if(selectedIndex === targetWord.length) {
                    setTimeout(() => {
                        if(++currentLevel === animals.length) {
                            showVictory();
                        } else {
                            initGame();
                        }
                    }, 800);
                }
            } else {
                // 错误时重置选择
                selectedIndex = 0;
                Array.from(cells).forEach(cell => cell.classList.remove('selected'));
            }
        }

        function showVictory() {
            document.getElementById('status').innerHTML = '🎉 全部完成！扫码分享吧！';
            document.getElementById('grid').innerHTML = '';
            
            // 生成二维码
            const qrcodeDiv = document.getElementById('qrcode');
            qrcodeDiv.style.display = 'block';
            new QRCode(qrcodeDiv, {
                text: window.location.href,
                width: 180,
                height: 180,
                colorDark: "#000000",
                colorLight: "#ffffff",
                correctLevel: QRCode.CorrectLevel.H
            });
        }

        // 初始化游戏
        initGame();
    </script>
</body>
</html>
