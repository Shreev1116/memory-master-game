class AudioManager {
    constructor() {
        this.ctx = new (window.AudioContext || window.webkitAudioContext)();
        this.bgmOsc = null;
        this.sfxMuted = false;
        this.musicMuted = false;
        this.masterVolume = 0.5;
        this.bgmAudio = null;
        this.currentTheme = null;
    }

    // Initialize checking audio context state
    init() {
        if (this.ctx.state === 'suspended') {
            this.ctx.resume();
        }
    }

    setVolume(val) {
        this.masterVolume = val;
        // Update MP3 music if playing
        if (this.bgmAudio) {
            this.bgmAudio.volume = this.masterVolume * 0.5; // Normalized base volume
        }
        // Update Synth music if playing (The Low Drone)
        if (this.bgmGain) {
            // Synth base is usually quieter (0.05), so we scale appropriately
            this.bgmGain.gain.setValueAtTime(this.masterVolume * 0.1, this.ctx.currentTime);
        }
    }

    toggleSFX(isMuted) {
        this.sfxMuted = isMuted;
    }

    toggleMusic(isMuted) {
        this.musicMuted = isMuted;
        if (this.musicMuted) {
            this.stopMusic();
        } else {
            // Resume/Restart if we have a theme
            if (this.currentTheme) {
                this.playThemeMusic(this.currentTheme);
            }
        }
    }

    playClick() {
        if (this.sfxMuted) return;
        this.playTone(400, 'sine', 0.1);
    }

    playFlip() {
        if (this.sfxMuted) return;
        // Soft swish
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();

        // Scale volume based on master setting. 
        // Base was 0.3 at 50% vol (master=0.5). formula: base * (master * 2)
        const vol = 0.3 * (this.masterVolume * 2);

        osc.frequency.setValueAtTime(300, this.ctx.currentTime);
        osc.frequency.exponentialRampToValueAtTime(800, this.ctx.currentTime + 0.1);
        gain.gain.setValueAtTime(vol, this.ctx.currentTime);
        gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + 0.1);

        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start();
        osc.stop(this.ctx.currentTime + 0.1);
    }

    playMatch() {
        if (this.sfxMuted) return;
        // Pleasant "Ding"
        this.playTone(600, 'sine', 0.1, 0.5);
        setTimeout(() => this.playTone(900, 'sine', 0.3, 0), 100);
    }

    playMismatch() {
        if (this.sfxMuted) return;
        // Low "Buzz"
        this.playTone(150, 'sawtooth', 0.3);
    }

    playWin() {
        if (this.sfxMuted) return;
        // Fanfare
        [523, 659, 784, 1046].forEach((freq, i) => {
            setTimeout(() => this.playTone(freq, 'square', 0.2), i * 100);
        });
    }

    playWarning() {
        if (this.sfxMuted) return;
        // Urgent beep
        this.playTone(800, 'square', 0.1);
    }

    // Helper for simple tones
    playTone(freq, type, duration, delay = 0) {
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = type;
        osc.frequency.value = freq;

        // Apply master volume
        // Base gain for Tone is 0.1, so we scale that by masterVolume (max 1.0)
        const vol = 0.1 * (this.masterVolume * 2); // Boost a bit since 0.1 is quiet

        gain.gain.setValueAtTime(vol, this.ctx.currentTime + delay);
        gain.gain.exponentialRampToValueAtTime(0.001, this.ctx.currentTime + delay + duration);

        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start(this.ctx.currentTime + delay);
        osc.stop(this.ctx.currentTime + delay + duration);
    }

    // Background music handler
    playThemeMusic(theme) {
        this.currentTheme = theme; // Store for resuming
        if (this.musicMuted) return;

        // 1. Try to play local file first (once per session or check existence)
        if (!this.bgmAudio) {
            this.bgmAudio = new Audio('bgm.mp3');
            this.bgmAudio.loop = true;
            this.bgmAudio.volume = this.masterVolume * 0.5; // Consistent vol

            // If it fails to load (404), fallback to synth
            this.bgmAudio.onerror = () => {
                console.log('bgm.mp3 not found, falling back to synthesized theme.');
                this.bgmAudio = null;
                this.playSynthTheme(theme);
            };

            // If it can play, it will start here
            this.bgmAudio.play().catch(e => {
                console.log('Autoplay prevented or file error:', e);
                this.playSynthTheme(theme);
            });
        } else {
            this.bgmAudio.volume = this.masterVolume * 0.5; // Ensure vol is correct on replay
            this.bgmAudio.play().catch(e => {
                console.error(e);
                this.playSynthTheme(theme);
            });
        }
    }

    // Synthesized fallback (Epic/Adventure style)
    playSynthTheme(theme) {
        if (this.bgmOsc) return; // Already playing synth

        // Create a sequencer for a more complex melody
        this.ctx.resume();
        this.tempo = 100;
        this.noteTime = 60 / this.tempo;
        this.sequenceStep = 0;
        this.isPlayingSynth = true;

        // Epic Chord Progression (Am - F - C - G)
        // Arpeggiated pattern
        const sequence = [
            // Am
            { f: 220, d: 0.25 }, { f: 329.6, d: 0.25 }, { f: 440, d: 0.25 }, { f: 329.6, d: 0.25 },
            { f: 220, d: 0.25 }, { f: 329.6, d: 0.25 }, { f: 440, d: 0.25 }, { f: 523.2, d: 0.25 },
            // F
            { f: 174.6, d: 0.25 }, { f: 261.6, d: 0.25 }, { f: 349.2, d: 0.25 }, { f: 261.6, d: 0.25 },
            { f: 174.6, d: 0.25 }, { f: 261.6, d: 0.25 }, { f: 349.2, d: 0.25 }, { f: 440, d: 0.25 },
            // C
            { f: 261.6, d: 0.25 }, { f: 329.6, d: 0.25 }, { f: 392, d: 0.25 }, { f: 329.6, d: 0.25 },
            { f: 261.6, d: 0.25 }, { f: 329.6, d: 0.25 }, { f: 392, d: 0.25 }, { f: 523.2, d: 0.25 },
            // G
            { f: 196, d: 0.25 }, { f: 246.9, d: 0.25 }, { f: 293.7, d: 0.25 }, { f: 246.9, d: 0.25 },
            { f: 196, d: 0.25 }, { f: 246.9, d: 0.25 }, { f: 293.7, d: 0.25 }, { f: 392, d: 0.25 },
        ];

        this.melodyLoop = () => {
            if (!this.isPlayingSynth) return;

            const note = sequence[this.sequenceStep % sequence.length];
            this.playSynthNote(note.f, note.d * 0.9); // Staccato

            this.sequenceStep++;
            this.synthTimeout = setTimeout(this.melodyLoop, note.d * 1000);
        };

        this.melodyLoop();

        // Add a bass drone for atmosphere
        this.bgmOsc = this.ctx.createOscillator();
        this.bgmGain = this.ctx.createGain();
        this.bgmOsc.type = 'sawtooth';
        this.bgmOsc.frequency.value = 55; // Low A
        this.bgmGain.gain.value = 0.05;

        // Filter the bass to make it less harsh
        this.bgmFilter = this.ctx.createBiquadFilter();
        this.bgmFilter.type = 'lowpass';
        this.bgmFilter.frequency.value = 400;

        this.bgmOsc.connect(this.bgmFilter);
        this.bgmFilter.connect(this.bgmGain);
        this.bgmGain.connect(this.ctx.destination);
        this.bgmOsc.start();
    }

    playSynthNote(freq, duration) {
        const osc = this.ctx.createOscillator();
        const gain = this.ctx.createGain();
        osc.type = 'triangle';
        osc.frequency.value = freq;

        // Envelope
        gain.gain.setValueAtTime(0, this.ctx.currentTime);
        gain.gain.linearRampToValueAtTime(0.1, this.ctx.currentTime + 0.05);
        gain.gain.exponentialRampToValueAtTime(0.01, this.ctx.currentTime + duration);

        osc.connect(gain);
        gain.connect(this.ctx.destination);
        osc.start();
        osc.stop(this.ctx.currentTime + duration + 0.1);
    }

    stopMusic() {
        // Stop MP3
        if (this.bgmAudio) {
            this.bgmAudio.pause();
            this.bgmAudio.currentTime = 0;
        }

        // Stop Synth
        this.isPlayingSynth = false;
        if (this.synthTimeout) clearTimeout(this.synthTimeout);

        if (this.bgmOsc) {
            this.bgmOsc.stop();
            this.bgmOsc.disconnect();
            this.bgmOsc = null;
        }
        if (this.bgmGain) {
            this.bgmGain.disconnect();
            this.bgmGain = null;
        }
    }
}

const audioMgr = new AudioManager();
