```html
<!DOCTYPE html>
<html lang="zh-HK">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>💗 給許如清的一封信 💗</title>
    <!-- canvas-confetti 輕量彩帶特效 -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none; /* 避免拖拽選中文字，保留美感 */
        }

        body {
            min-height: 100vh;
            background: linear-gradient(135deg, #ffe6f0 0%, #ffb7c5 100%);
            font-family: 'Segoe UI', 'Quicksand', system-ui, -apple-system, 'PingFang TC', 'Microsoft YaHei', sans-serif;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow-x: hidden;
            cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="27" viewBox="0 0 24 27"><polygon points="12,2 15,9 22,9 16,14 19,22 12,17 5,22 8,14 2,9 9,9 12,2" fill="%23ff4d6d" stroke="%23fff" stroke-width="1"/></svg>') 12 5, auto;
        }

        /* 飄浮愛心背景 */
        .floating-heart {
            position: fixed;
            pointer-events: none;
            font-size: 1.8rem;
            opacity: 0.6;
            animation: floatUp 9s linear forwards;
            z-index: 1;
            filter: drop-shadow(0 0 4px rgba(255,80,120,0.5));
        }

        @keyframes floatUp {
            0% {
                transform: translateY(100vh) rotate(0deg);
                opacity: 0.8;
            }
            100% {
                transform: translateY(-20vh) rotate(20deg);
                opacity: 0;
            }
        }

        /* 主卡片 */
        .card {
            position: relative;
            z-index: 20;
            max-width: 620px;
            width: 90%;
            background: rgba(255, 248, 250, 0.96);
            backdrop-filter: blur(2px);
            border-radius: 68px 48px 80px 52px;
            box-shadow: 0 30px 45px rgba(0, 0, 0, 0.2), 0 0 0 10px rgba(255, 215, 225, 0.6), 0 0 0 15px #ffb7c5;
            padding: 2rem 1.8rem 2.5rem;
            transition: all 0.3s ease;
            text-align: center;
            animation: fadeInUp 1s ease-out;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(40px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* 閃亮名字 */
        .name-glow {
            font-family: 'Dancing Script', 'Segoe UI', cursive;
            font-size: 3.5rem;
            font-weight: bold;
            background: linear-gradient(135deg, #e8436e, #ff8da1, #ff5470);
            background-size: 200%;
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            animation: shimmer 3s infinite linear;
            display: inline-block;
            margin-bottom: 0.5rem;
            text-shadow: 0 2px 8px rgba(255,80,120,0.2);
        }

        @keyframes shimmer {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .greeting {
            font-size: 1.8rem;
            font-weight: 600;
            color: #b13e5c;
            margin-top: 0.5rem;
            letter-spacing: 1px;
        }

        .letter {
            background: #fffafc;
            padding: 1.5rem;
            border-radius: 48px;
            margin: 1.4rem 0;
            text-align: left;
            font-size: 1.05rem;
            line-height: 1.55;
            color: #4a2a34;
            box-shadow: inset 0 0 0 2px #ffe0e8, 0 8px 18px rgba(0,0,0,0.05);
            font-weight: 500;
        }

        .letter p {
            margin-bottom: 12px;
        }

        .letter .sign {
            text-align: right;
            font-family: 'Dancing Script', cursive;
            font-size: 1.3rem;
            margin-top: 15px;
            color: #dd5f7e;
        }

        .heart-icon {
            display: inline-block;
            animation: beat 1.2s infinite ease-in-out;
            font-size: 1.4rem;
        }

        @keyframes beat {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }

        /* 按鈕區 */
        .buttons {
            display: flex;
            justify-content: center;
            gap: 24px;
            flex-wrap: wrap;
            margin-top: 20px;
        }

        .btn {
            padding: 14px 28px;
            font-size: 1.3rem;
            font-weight: bold;
            border: none;
            border-radius: 60px;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 5px 12px rgba(0,0,0,0.15);
            font-family: inherit;
        }

        .btn-yes {
            background: #ff5c8a;
            color: white;
            border-bottom: 4px solid #b12b51;
        }

        .btn-yes:hover {
            background: #ff3b6f;
            transform: scale(1.05);
            box-shadow: 0 12px 20px rgba(255, 60, 110, 0.4);
        }

        .btn-no {
            background: #d9d0d4;
            color: #5e3b48;
            border-bottom: 4px solid #a58794;
            position: relative;
        }

        .btn-no:hover {
            background: #cbbbc1;
        }

        /* 拒絕後的小氣泡提示 */
        .toast-msg {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            background: #fffaec;
            color: #c7375c;
            padding: 10px 20px;
            border-radius: 60px;
            font-weight: bold;
            font-size: 1rem;
            box-shadow: 0 5px 20px rgba(0,0,0,0.2);
            z-index: 100;
            backdrop-filter: blur(12px);
            background: rgba(255, 235, 240, 0.95);
            border-left: 6px solid #ff5c8a;
            pointer-events: none;
            animation: fadeSlide 2s forwards;
        }

        @keyframes fadeSlide {
            0% { opacity: 0; transform: translateX(-50%) translateY(20px); }
            15% { opacity: 1; transform: translateX(-50%) translateY(0); }
            85% { opacity: 1; transform: translateX(-50%) translateY(0); }
            100% { opacity: 0; transform: translateX(-50%) translateY(-15px); visibility: hidden; }
        }

        /* 點擊頁面噴出愛心小特效 */
        .click-heart {
            position: absolute;
            pointer-events: none;
            font-size: 1.5rem;
            animation: burst 0.8s ease-out forwards;
            z-index: 999;
        }

        @keyframes burst {
            0% { opacity: 1; transform: scale(0.5) translateY(0); }
            100% { opacity: 0; transform: scale(1.5) translateY(-40px); }
        }

        /* 慶祝覆蓋層 */
        .celebrate-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.7);
            backdrop-filter: blur(6px);
            z-index: 200;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            animation: fadeBg 0.5s ease;
        }

        @keyframes fadeBg {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .celebrate-card {
            background: white;
            border-radius: 70px;
            padding: 40px 30px;
            max-width: 85%;
            text-align: center;
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
            animation: bouncePop 0.5s cubic-bezier(0.34, 1.2, 0.64, 1);
        }

        @keyframes bouncePop {
            0% { transform: scale(0.8); opacity: 0; }
            80% { transform: scale(1.05); }
            100% { transform: scale(1); opacity: 1; }
        }

        .celebrate-card h1 {
            font-size: 2.5rem;
            color: #ff4d6d;
            margin: 20px 0 10px;
        }

        .celebrate-card p {
            font-size: 1.4rem;
            margin: 20px;
            color: #653142;
        }

        .close-celebrate {
            background: #ff5c8a;
            border: none;
            color: white;
            padding: 12px 28px;
            border-radius: 50px;
            font-size: 1.2rem;
            font-weight: bold;
            margin-top: 15px;
            cursor: pointer;
        }

        /* 動態躲避按鈕邏輯(不離開視窗) */
        @media (max-width: 550px) {
            .btn { padding: 12px 20px; font-size: 1.1rem; }
            .name-glow { font-size: 2.5rem; }
            .letter { font-size: 0.95rem; }
        }

        footer {
            margin-top: 25px;
            font-size: 0.7rem;
            opacity: 0.7;
            color: #b34e6e;
        }
    </style>
</head>
<body>

    <!-- 動態漂浮愛心會由JS生成，但先給一些static背景 -->
    <div id="heartContainer"></div>

    <div class="card">
        <div class="greeting">💌 親愛的</div>
        <div class="name-glow">許如清 💖</div>
        
        <div class="letter">
            <p>🌟 遇見你的那天，星光都落在你眼裡。從那之後，我的四季只為你而溫柔。</p>
            <p>🌷 你的笑容是我每日最想守護的風景，你的聲音是我最熟悉的旋律。不論晴天或下雨，我都想陪在你身邊，分享所有平凡又閃亮的日常。</p>
            <p>🍀 我的心不大，但剛剛好能裝下一個你。未來的每個章節，我都想與你一起書寫，一起笑、一起鬧、一起慢慢變老。</p>
            <p>🎀 所以今天鼓起所有勇氣，想認真問你——</p>
            <div class="sign">—— 永遠愛慕你的人 💗</div>
        </div>

        <div class="buttons">
            <button class="btn btn-yes" id="yesBtn">💕 我願意 💕</button>
            <button class="btn btn-no" id="noBtn">🤔 再想想</button>
        </div>
        <footer>✨ 點一下畫面任意處，會冒出驚喜小愛心 ✨</footer>
    </div>

    <script>
        // ---------- 浮動愛心背景 ----------
        function createFloatingHeart() {
            const heartContainer = document.getElementById('heartContainer');
            if (!heartContainer) return;
            const heart = document.createElement('div');
            heart.classList.add('floating-heart');
            const heartList = ['❤️', '💖', '💗', '💓', '💕', '💞', '🌸', '🌺', '✨'];
            heart.innerText = heartList[Math.floor(Math.random() * heartList.length)];
            const size = Math.random() * 30 + 20; // 20px - 50px
            heart.style.fontSize = size + 'px';
            heart.style.left = Math.random() * 100 + '%';
            heart.style.animationDuration = 6 + Math.random() * 6 + 's';
            heart.style.animationDelay = Math.random() * 2 + 's';
            heart.style.opacity = Math.random() * 0.5 + 0.3;
            heartContainer.appendChild(heart);
            setTimeout(() => {
                heart.remove();
            }, 10000);
        }
        // 每0.7秒生成一個浮動愛心
        setInterval(createFloatingHeart, 700);
        for(let i=0;i<8;i++) setTimeout(createFloatingHeart, i*200);

        // ---------- 點擊螢幕噴射小愛心特效 ----------
        function spawnClickHeart(e) {
            const x = e.clientX;
            const y = e.clientY;
            const heartDiv = document.createElement('div');
            heartDiv.classList.add('click-heart');
            heartDiv.innerText = ['❤️', '💖', '💗', '💝', '💘', '🥰'][Math.floor(Math.random()*6)];
            heartDiv.style.left = x + 'px';
            heartDiv.style.top = y + 'px';
            heartDiv.style.position = 'fixed';
            heartDiv.style.fontSize = (20 + Math.random() * 20) + 'px';
            heartDiv.style.zIndex = '9999';
            document.body.appendChild(heartDiv);
            setTimeout(() => heartDiv.remove(), 800);
        }
        document.body.addEventListener('click', (e) => {
            // 避免點擊按鈕時重複觸發太多次，但仍然保留浪漫感
            spawnClickHeart(e);
        });

        // ---------- 拒絕按鈕 調皮移動邏輯 ----------
        const noBtn = document.getElementById('noBtn');
        const yesBtn = document.getElementById('yesBtn');
        let moveTimeout = null;
        let isCelebrating = false;   // 慶祝時不讓搞笑移動

        function getRandomPosition(btn) {
            const btnRect = btn.getBoundingClientRect();
            const winWidth = window.innerWidth;
            const winHeight = window.innerHeight;
            const btnWidth = btnRect.width;
            const btnHeight = btnRect.height;
            // 邊界保留20px
            let maxX = winWidth - btnWidth - 30;
            let maxY = winHeight - btnHeight - 80;
            let minX = 20;
            let minY = 80;
            if (maxX < minX) maxX = minX + 10;
            if (maxY < minY) maxY = minY + 10;
            let randX = Math.random() * (maxX - minX) + minX;
            let randY = Math.random() * (maxY - minY) + minY;
            return { x: randX, y: randY };
        }

        function moveNoButton() {
            if (isCelebrating) return;
            if (!noBtn) return;
            // 獲取按鈕位置，使其變為絕對定位並移動
            const style = window.getComputedStyle(noBtn);
            if (style.position !== 'fixed') {
                noBtn.style.position = 'fixed';
                const rect = noBtn.getBoundingClientRect();
                noBtn.style.left = rect.left + 'px';
                noBtn.style.top = rect.top + 'px';
                noBtn.style.width = rect.width + 'px';
                noBtn.style.zIndex = '99';
                // 原來的位置佔位保持flex佈局不亂？但移走後其他按鈕會移動，yesBtn保持static沒問題，我們不需要保留原佔位
                // 但為了佈局漂亮，讓yesBtn依然在卡片內，noBtn變浮動後不會破壞卡片，我們將複製一份文本？
                // 更順滑方案：把noBtn從buttons內取出放到body，但仍保留視覺。
                // 簡單處理：直接改變樣式並加入body避免父容器影響。
                const parent = noBtn.parentNode;
                parent.removeChild(noBtn);
                document.body.appendChild(noBtn);
                noBtn.style.left = rect.left + 'px';
                noBtn.style.top = rect.top + 'px';
            }
            const { x, y } = getRandomPosition(noBtn);
            noBtn.style.transition = 'left 0.2s ease, top 0.2s ease';
            noBtn.style.left = x + 'px';
            noBtn.style.top = y + 'px';
            
            // 顯示溫柔提醒
            showToastMessage('🥺 再考慮一下嘛，我會一直等你的～', 1800);
        }

        function showToastMessage(msg, duration=2000) {
            if (document.querySelector('.toast-msg')) return;
            const toast = document.createElement('div');
            toast.className = 'toast-msg';
            toast.innerText = msg;
            document.body.appendChild(toast);
            setTimeout(() => {
                if(toast) toast.remove();
            }, duration);
        }

        // 處理拒絕按鈕事件（滑鼠移上/觸碰）
        function bindNoBtnEscape() {
            if (!noBtn) return;
            const handleEscape = (e) => {
                if (isCelebrating) return;
                e.preventDefault();
                moveNoButton();
            };
            noBtn.addEventListener('mouseenter', handleEscape);
            noBtn.addEventListener('touchstart', (e) => {
                e.preventDefault();
                if (isCelebrating) return;
                moveNoButton();
            });
            noBtn.addEventListener('click', (e) => {
                e.preventDefault();
                if (isCelebrating) return;
                moveNoButton();
                showToastMessage('💗 命運不讓你拒絕～ 點我願意好不好 💗', 1500);
            });
        }
        bindNoBtnEscape();

        // ---------- 華麗慶祝「我願意」 ----------
        function startCelebration() {
            if (isCelebrating) return;
            isCelebrating = true;
            
            // 禁掉noBtn移動 / 隱藏no按鈕
            if (noBtn && noBtn.parentNode) {
                noBtn.style.display = 'none';
            }
            // 把YES按鈕暫時置灰但保留顯示
            yesBtn.disabled = true;
            yesBtn.style.opacity = '0.7';
            
            // 發射大量彩帶
            function shootConfetti() {
                canvasConfetti({
                    particleCount: 180,
                    spread: 100,
                    origin: { y: 0.6 },
                    startVelocity: 18,
                    colors: ['#ff5e8e', '#ffb3c6', '#ffd966', '#fa8072']
                });
                canvasConfetti({
                    particleCount: 80,
                    angle: 60,
                    spread: 55,
                    origin: { x: 0, y: 0.7 },
                    startVelocity: 25,
                });
                canvasConfetti({
                    particleCount: 80,
                    angle: 120,
                    spread: 55,
                    origin: { x: 1, y: 0.7 },
                    startVelocity: 25,
                });
            }
            shootConfetti();
            let confettiInterval = setInterval(() => {
                if (!isCelebrating) clearInterval(confettiInterval);
                else shootConfetti();
            }, 400);
            
            // 顯示浪漫覆蓋層
            const overlay = document.createElement('div');
            overlay.className = 'celebrate-overlay';
            overlay.innerHTML = `
                <div class="celebrate-card">
                    <span style="font-size:4rem;">💞✨💞</span>
                    <h1>我們在一起吧 ❤️</h1>
                    <p>許如清，我愛你！<br>以後的每一天，都讓我守護妳 🌹</p>
                    <button class="close-celebrate" id="closeCelebrateBtn">💋 牽住我的手 💋</button>
                </div>
            `;
            document.body.appendChild(overlay);
            
            // 持續背景音樂無法自動播放，但我們可以溫柔提示；如果想播放內置聲音可略
            // 增加超級多爆裂愛心動畫效果
            for(let i=0;i<60;i++) {
                setTimeout(() => {
                    const fakeEvent = { clientX: Math.random() * window.innerWidth, clientY: Math.random() * window.innerHeight };
                    spawnClickHeart(fakeEvent);
                }, i*80);
            }
            
            const closeBtn = overlay.querySelector('#closeCelebrateBtn');
            closeBtn.addEventListener('click', () => {
                clearInterval(confettiInterval);
                overlay.remove();
                // 重置狀態，但保留不讓NO按鈕再出現（既然在一起了，不需要再拒絕）
                if (noBtn) noBtn.remove();
                // 改變yes按鈕文字和狀態
                yesBtn.innerText = '💗 我們永遠在一起 💗';
                yesBtn.disabled = false;
                yesBtn.style.opacity = '1';
                yesBtn.style.background = '#ff8dae';
                yesBtn.style.cursor = 'default';
                yesBtn.style.transform = 'none';
                // 額外顯示永久浪漫訊息
                const loveMsg = document.createElement('div');
                loveMsg.style.marginTop = '20px';
                loveMsg.style.fontSize = '1.2rem';
                loveMsg.style.fontWeight = 'bold';
                loveMsg.style.color = '#e8436e';
                loveMsg.innerHTML = '❤️ 我願意 + 永遠 ❤️';
                document.querySelector('.buttons').appendChild(loveMsg);
                isCelebrating = false;
            });
        }
        
        yesBtn.addEventListener('click', (e) => {
            e.stopPropagation();
            if (isCelebrating) return;
            startCelebration();
        });
        
        // 額外：如果用戶嘗試拖曳no按鈕返回？不用擔心，只會更加有趣
        // 窗口調整時如果no按鈕是fixed，需重新確保不飛出邊界，但簡單處理
        window.addEventListener('resize', () => {
            if (noBtn && noBtn.style.position === 'fixed' && !isCelebrating) {
                const rect = noBtn.getBoundingClientRect();
                let left = rect.left;
                let top = rect.top;
                const maxLeft = window.innerWidth - rect.width - 20;
                const maxTop = window.innerHeight - rect.height - 50;
                if (left < 10) left = 10;
                if (top < 50) top = 50;
                if (left > maxLeft) left = maxLeft;
                if (top > maxTop) top = maxTop;
                noBtn.style.left = left + 'px';
                noBtn.style.top = top + 'px';
            }
        });
        
        // 增加一個小細節，讓滾動時背景更夢幻，阻止body滾動條（無需滾動）
        document.body.style.overflowX = 'hidden';
        document.body.style.overflowY = 'auto';
        
        // 預先設定按鈕卡片不會因移動noBtn破版, 確保無bug
        // 另存一份拒絕幽默提示 也會在點再想想觸發移動
        console.log("%c💖 許如清，你是我心裡最美的夢 💖", "color:#ff4d6d; font-size:18px");
    </script>
</body>
</html>
```
