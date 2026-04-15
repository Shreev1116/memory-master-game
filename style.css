// Game Configuration
const LEVELS = [
    { level: 1, theme: 'insects', grid: [2, 2], matchReq: 2, time: 45, preview: 2, penalty: 0, themeClass: 'theme-kids', title: "Insects" },
    { level: 2, theme: 'animals', grid: [3, 2], matchReq: 2, time: 45, preview: 3, penalty: 1, themeClass: 'theme-kids', title: "Animals" },
    { level: 3, theme: 'birds', grid: [4, 3], matchReq: 2, time: 60, preview: 4, penalty: 1, themeClass: 'theme-kids', title: "Mythical Birds" },
    { level: 4, theme: 'vehicles', grid: [4, 4], matchReq: 2, time: 80, preview: 3, penalty: 2, themeClass: 'theme-mid', title: "Vehicles" },
    { level: 5, theme: 'heroes', grid: [5, 4], matchReq: 2, time: 110, preview: 2, penalty: 2, themeClass: 'theme-mid', title: "Weather" },
    { level: 6, theme: 'gadgets', grid: [6, 5], matchReq: 2, time: 110, preview: 2, penalty: 3, themeClass: 'theme-mid', title: "Tech Gadgets" },
    { level: 7, theme: 'food', grid: [6, 6], matchReq: 3, time: 120, preview: 2, penalty: 3, themeClass: 'theme-tech', title: "Yummy Food" },
    { level: 8, theme: 'sports', grid: [7, 6], matchReq: 3, time: 130, preview: 1.5, penalty: 3, themeClass: 'theme-tech', title: "Sports" },
    { level: 9, theme: 'emojis', grid: [7, 7], matchReq: 3, time: 145, preview: 1, penalty: 3, themeClass: 'theme-tech', title: "Tricky Emojis", gap: true },
    { level: 10, theme: 'chinese', grid: [7, 7], matchReq: 3, time: 160, preview: 1, penalty: 3, themeClass: 'theme-master', title: "Master Chinese", gap: true }
];

// Content Assets (Emojis/Chars)
// Content Assets (Refined for User Requirements)
const ASSETS = {
    insects: ['🐞', '🦋', '🐝', '🐜', '🦗', '🕷️', '🦂', '🦟'],
    animals: ['🦁', '🐯', '🐻', '🐨', '🐼', '🐸', '🐙', '🐵'],
    birds: ['🦅', '🦉', '🦆', '🦢', '🦜', '🦚', '🦃', '🐧'],
    vehicles: ['🚗', '🏍️', '🚚', '🚜', '🚲', '🛴', '🚌', '🚓'],
    // Heroes replaced with Weather
    heroes: ['☀️', '☁️', '❄️', '⚡', '🌈', '🌪️', '☔', '🌑', '🌡️', '🌫️'],
    // Tech Gadgets for Level 6
    gadgets: ['📱', '💻', '⌚', '📷', '🎧', '🎮', '🔋', '🖱️', '⌨️', '💾', '🔌', '📡', '🔭', '🔬', '🤖', '🚁'],
    food: ['🍕', '🍔', '🍟', '🌭', '🍿', '🍩', '🍪', '🎂', '🍰', '🧁', '🍫', '🍬', '🍭', '🍮', '🍯'],
    sports: ['⚽', '🏀', '🏈', '⚾', '🎾', '🏐', '🏉', '🎱', '🏓', '🏸', '🏒', '🥊', '🥋', '⛳', '⛸️', '🎣'],
    // Tricky Emojis for Level 9 (Very similar faces)
    emojis: ['😀', '😃', '😄', '😁', '😆', '😅', '🤣', '😂', '😉', '😊', '😇', '🙂', '🙃', '😋', '😌', '😍'],
    chinese: ['愛', '安', '八', '白', '百', '北', '本', '比', '邊', '便', '表', '別', '病', '不', '步', '才']
};

class Game {
    constructor() {
        this.currentLevelIdx = 0;
        this.score = 0;
        this.timeLeft = 0;
        this.timerInterval = null;
        this.flippedCards = [];
        this.matchedCards = [];
        this.isLocked = false;

        // DOM Elements
        this.screens = {
            menu: document.getElementById('main-menu'),
            game: document.getElementById('game-screen'),
            hud: document.getElementById('game-hud')
        };
        this.board = document.getElementById('game-board');
        this.modals = {
            overlay: document.getElementById('modal-overlay'),
            complete: document.getElementById('modal-level-complete'),
            gameOver: document.getElementById('modal-game-over'),
            pause: document.getElementById('modal-pause')
        };

        this.initListeners();
    }

    initListeners() {
        document.getElementById('btn-start').onclick = () => this.startLevel(0);
        document.getElementById('btn-next-level').onclick = () => {
            this.hideModals();
            this.startLevel(this.currentLevelIdx + 1);
        };
        document.getElementById('btn-replay-level').onclick = () => {
            this.hideModals();
            this.startLevel(this.currentLevelIdx);
        };
        document.getElementById('btn-retry-level').onclick = () => {
            this.hideModals();
            this.startLevel(this.currentLevelIdx);
        };
        // Fix Quit Buttons
        const quitToMenu = () => {
            this.hideModals();
            this.showScreen('menu'); // Correctly switch to menu
        };
        document.getElementById('btn-quit').onclick = quitToMenu;
        document.getElementById('btn-quit-pause').onclick = quitToMenu; // Ensure both use same logic
        document.getElementById('btn-pause').onclick = () => this.togglePause();
        document.getElementById('btn-resume').onclick = () => this.togglePause();

        // New HUD Restart Button
        document.getElementById('btn-restart').onclick = () => {
            // Confirm? Nah, instant action is better for "Retry" usually, or maybe safer.
            // Let's just restart.
            this.startLevel(this.currentLevelIdx);
        };

        // --- Settings Menu Wiring ---
        document.getElementById('btn-settings').onclick = () => {
            document.querySelector('.menu-controls').classList.add('hidden');
            document.querySelector('.settings-menu-container').classList.remove('hidden');
        };

        document.getElementById('btn-back-settings').onclick = () => {
            document.querySelector('.settings-menu-container').classList.add('hidden');
            document.querySelector('.menu-controls').classList.remove('hidden');
        };



        // Audio Controls (Main Settings)
        document.getElementById('set-music').onchange = (e) => {
            const isUnchecked = !e.target.checked;
            audioMgr.toggleMusic(isUnchecked);
            document.getElementById('chk-music').checked = e.target.checked; // Sync Pause Toggle
        };
        document.getElementById('set-sfx').onchange = (e) => {
            const isUnchecked = !e.target.checked;
            audioMgr.toggleSFX(isUnchecked);
            document.getElementById('chk-sound').checked = e.target.checked; // Sync Pause Toggle
        };
        document.getElementById('set-volume').oninput = (e) => {
            const val = e.target.value;
            document.getElementById('vol-display').textContent = val + '%';
            audioMgr.setVolume(val / 100);
        };

        // Audio Controls (Pause Menu)
        document.getElementById('chk-music').onchange = (e) => {
            const isChecked = e.target.checked;
            audioMgr.toggleMusic(!isChecked); // If checked (On), muted is false
            document.getElementById('set-music').checked = isChecked; // Sync Main Setting
        };
        document.getElementById('chk-sound').onchange = (e) => {
            const isChecked = e.target.checked;
            audioMgr.toggleSFX(!isChecked);
            document.getElementById('set-sfx').checked = isChecked; // Sync Main Setting
        };

        // Gameplay Controls
        document.getElementById('set-speed').onchange = (e) => {
            document.documentElement.style.setProperty('--anim-speed', e.target.value);
        };

        document.getElementById('set-theme-mode').onchange = (e) => {
            if (e.target.value === 'dark') {
                document.body.classList.add('dark-mode');
            } else {
                document.body.classList.remove('dark-mode');
            }
        };

        // Level Select
        document.getElementById('btn-levels').onclick = () => {
            document.querySelector('.menu-controls').classList.add('hidden');
            document.querySelector('.level-select-container').classList.remove('hidden');
            this.generateLevelGrid();
        };
        document.getElementById('btn-back-levels').onclick = () => {
            document.querySelector('.level-select-container').classList.add('hidden');
            document.querySelector('.menu-controls').classList.remove('hidden');
        };

        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape' && !this.screens.game.classList.contains('hidden')) {
                this.togglePause();
            }
        });
    }

    generateLevelGrid() {
        const grid = document.getElementById('level-grid');
        grid.innerHTML = '';
        LEVELS.forEach((lvl, idx) => {
            const btn = document.createElement('button');
            btn.className = 'btn-secondary level-btn';
            btn.textContent = lvl.level;
            btn.style.margin = '5px';
            btn.style.width = '60px'; // Consistent size

            // Highlight current level? Or just all available? Assuming all unlocked for now.
            btn.onclick = () => {
                this.startLevel(idx);
            };
            grid.appendChild(btn);
        });
    }

    showScreen(screenName) {
        Object.values(this.screens).forEach(s => s.classList.add('hidden'));
        Object.values(this.screens).forEach(s => s.classList.remove('active'));

        if (screenName === 'game') {
            this.screens.game.classList.remove('hidden');
            this.screens.game.classList.add('active'); // Ensure pointer events
            this.screens.hud.classList.remove('hidden');
        } else {
            // Menu
            this.screens.menu.classList.add('active');
            this.screens.menu.classList.remove('hidden'); // Ensure visible
            this.screens.hud.classList.add('hidden');
            audioMgr.stopMusic(); // Silence on menu
        }
    }


    hideModals() {
        this.modals.overlay.classList.add('hidden');
        Object.values(this.modals).forEach(m => {
            if (m !== this.modals.overlay) m.classList.add('hidden');
        });
    }

    startLevel(levelIdx) {
        if (levelIdx >= LEVELS.length) {
            this.showVictory();
            return;
        }

        this.currentLevelIdx = levelIdx;
        this.config = LEVELS[levelIdx];
        this.timeLeft = this.config.time;
        this.score = 0; // Reset score per level or keep cumulative? Let's reset for now to simplify star calc.
        this.flippedCards = [];
        this.matchedCards = [];
        this.isLocked = true; // Locked during preview

        this.applyTheme();
        this.generateBoard();
        this.showScreen('game');
        this.updateHUD();
        this.startPreview();
    }

    applyTheme() {
        audioMgr.stopMusic(); // Stop previous

        // Persist Dark Mode check
        // We check the actual DOM class or the settings input value
        const isDarkMode = document.body.classList.contains('dark-mode') || document.getElementById('set-theme-mode').value === 'dark';

        document.body.className = this.config.themeClass;

        if (isDarkMode) {
            document.body.classList.add('dark-mode');
        }

        // Set CSS grid vars
        document.documentElement.style.setProperty('--grid-cols', this.config.grid[1]);

        // Update HUD
        document.getElementById('level-display').textContent = `Level ${this.config.level}`;
        document.getElementById('theme-display').textContent = this.config.title;

        // Start Music
        audioMgr.playThemeMusic(this.config.theme);
    }

    generateBoard() {
        this.board.innerHTML = '';
        const totalSlots = this.config.grid[0] * this.config.grid[1];
        let cardCount = totalSlots;

        if (this.config.gap) {
            cardCount -= 1; // Center gap for 7x7
        }

        const matchGroupSize = this.config.matchReq;
        const numGroups = Math.floor(cardCount / matchGroupSize);
        // Safety check if math doesn't align? 48 / 3 = 16 groups. Perfect.

        let deck = [];
        const themeAssets = ASSETS[this.config.theme] || ASSETS.insects; // Fallback

        for (let i = 0; i < numGroups; i++) {
            const asset = themeAssets[i % themeAssets.length];
            for (let j = 0; j < matchGroupSize; j++) {
                deck.push({ id: i, content: asset });
            }
        }

        // Shuffle
        deck.sort(() => Math.random() - 0.5);

        // Render
        deck.forEach((cardData, index) => {
            // Handle gap for 7x7 (index 24 is center of 49)
            if (this.config.gap && index === 24) {
                const gap = document.createElement('div');
                this.board.appendChild(gap);
                // We need to inject the gap, but deck has 48 items. 
                // So we pause deck iteration? 
                // Actually easier: iterate slots.
            }
        });

        // Better render loop for Grid with Gap
        this.board.innerHTML = '';
        let cardIdx = 0;
        for (let i = 0; i < totalSlots; i++) {
            if (this.config.gap && i === Math.floor(totalSlots / 2)) {
                const gap = document.createElement('div'); // Empty gap
                this.board.appendChild(gap);
                continue;
            }
            if (cardIdx >= deck.length) break;

            const card = this.createCard(deck[cardIdx], i);
            this.board.appendChild(card);
            cardIdx++;
        }
    }

    createCard(data, index) {
        const div = document.createElement('div');
        div.className = 'card'; // Start unchecked (Face Down). Preview will flip them.
        div.dataset.id = data.id;
        div.dataset.index = index;

        // Structure: Front = Pattern (Visible default). Back = Content (Visible when flipped).
        div.innerHTML = `
            <div class="card-face card-front"></div>
            <div class="card-face card-back">${data.content}</div>
        `;

        div.onclick = () => this.handleCardClick(div);
        return div;
    }

    startPreview() {
        const previewTime = this.config.preview;
        const previewEl = document.getElementById('preview-overlay');
        const countEl = document.getElementById('preview-countdown');
        previewEl.classList.remove('hidden');

        // Show all cards face up
        const cards = document.querySelectorAll('.card');
        cards.forEach(c => c.classList.add('flipped')); // Force Flip Up for Preview

        let seconds = previewTime;
        countEl.textContent = seconds;

        const interval = setInterval(() => {
            seconds--;
            countEl.textContent = seconds;
            if (seconds <= 0) {
                clearInterval(interval);
                previewEl.classList.add('hidden');
                this.flipAllBack();
                this.startTimer();
                this.isLocked = false;
            }
        }, 1000);
    }

    flipAllBack() {
        document.querySelectorAll('.card').forEach(card => {
            card.classList.remove('flipped');
        });
    }

    startTimer() {
        if (!this.modals.pause.classList.contains('hidden') || !this.modals.overlay.classList.contains('hidden')) {
            // Game is paused. Do not start the timer loop yet.
            // But we should ensure HUD shows initial time?
            this.updateHUD(); // Ensure UI is fresh
            return;
        }

        clearInterval(this.timerInterval);
        this.timerInterval = setInterval(() => {
            this.timeLeft--;
            this.updateHUD();

            if (this.timeLeft <= 0) {
                this.endGame(false);
            }
        }, 1000);
    }

    updateHUD() {
        const min = Math.floor(this.timeLeft / 60);
        const sec = this.timeLeft % 60;
        document.getElementById('time-display').textContent =
            `${min}:${sec.toString().padStart(2, '0')}`;

        const barFill = document.getElementById('timer-bar-fill');
        const pct = (this.timeLeft / this.config.time) * 100;
        barFill.style.width = `${pct}%`;

        if (this.timeLeft <= 10) {
            barFill.classList.add('warning');
            audioMgr.playWarning(); // Beep
        } else {
            barFill.classList.remove('warning');
        }

        document.getElementById('score-display').textContent = this.score;
    }

    handleCardClick(card) {
        if (this.isLocked) return;
        if (card.classList.contains('flipped') || card.classList.contains('matched')) return;

        audioMgr.playFlip();
        card.classList.add('flipped');
        this.flippedCards.push(card);

        if (this.flippedCards.length === this.config.matchReq) {
            this.checkMatch();
        }
    }

    checkMatch() {
        this.isLocked = true;
        const [first, ...others] = this.flippedCards;
        const isMatch = others.every(c => c.dataset.id === first.dataset.id);

        if (isMatch) {
            audioMgr.playMatch();
            this.score += 100 * this.currentLevelIdx + 50; // Simple logic

            setTimeout(() => {
                this.flippedCards.forEach(c => c.classList.add('matched'));
                this.matchedCards.push(...this.flippedCards);
                this.flippedCards = [];
                this.isLocked = false;
                this.checkWin();
            }, 500);
        } else {
            audioMgr.playMismatch();
            // Penalty
            if (this.config.penalty > 0) {
                this.timeLeft -= this.config.penalty;
                this.showFloatingText(`-${this.config.penalty}s`, 'red');
                this.updateHUD(); // Immediate update
            }

            setTimeout(() => {
                this.flippedCards.forEach(c => {
                    c.classList.remove('flipped');
                    c.classList.add('shake');
                    setTimeout(() => c.classList.remove('shake'), 500);
                });
                this.flippedCards = [];
                this.isLocked = false;
            }, 1000);
        }
        this.updateHUD();
    }

    checkWin() {
        // Total cards - matched cards
        // Note: total slots might include gap. matchedCards.length should equal number of actual cards.
        let totalCards = this.config.grid[0] * this.config.grid[1];
        if (this.config.gap) totalCards -= 1;

        if (this.matchedCards.length === totalCards) {
            this.endGame(true);
        }
    }

    endGame(win) {
        clearInterval(this.timerInterval);
        this.isLocked = true;
        this.modals.overlay.classList.remove('hidden');

        if (win) {
            audioMgr.playWin();
            document.getElementById('modal-level-complete').classList.remove('hidden');
            // Calc Stars (dummy logic)
            const pct = this.timeLeft / this.config.time;
            let stars = "★";
            if (pct > 0.3) stars += "★";
            if (pct > 0.6) stars += "★";
            document.getElementById('modal-stars').textContent = stars;
            document.getElementById('modal-score').textContent = `Score: ${this.score}`;
        } else {
            document.getElementById('modal-game-over').classList.remove('hidden');
        }
    }

    togglePause() {
        const isPaused = !this.modals.overlay.classList.contains('hidden')
            && !document.getElementById('modal-pause').classList.contains('hidden');

        if (isPaused) { // Resuming
            this.modals.overlay.classList.add('hidden');
            document.getElementById('modal-pause').classList.add('hidden');
            this.startTimer();
            this.isLocked = false; // Careful if we were in middle of match check? 
            // Better: restore prev state. For simplicity: unlock.
        } else { // Pausing
            clearInterval(this.timerInterval);
            this.modals.overlay.classList.remove('hidden');
            document.getElementById('modal-pause').classList.remove('hidden');
            this.isLocked = true;
        }
    }

    showFloatingText(text, color) {
        // Implementation for visual penality feedback could go here
    }
}

// Start
window.onload = () => {
    audioMgr.init();
    const game = new Game();
};
