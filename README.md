<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ML Journey - Interactive README</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'JetBrains Mono', 'Fira Code', monospace;
            background: linear-gradient(135deg, #0a0a0a, #1a1a2e, #16213e);
            color: #ffffff;
            overflow-x: hidden;
            line-height: 1.6;
        }

        .neural-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            opacity: 0.1;
        }

        .hero {
            height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            position: relative;
            background: radial-gradient(circle at center, rgba(138, 43, 226, 0.3), transparent 70%);
        }

        .glitch-title {
            font-size: clamp(3rem, 8vw, 8rem);
            font-weight: bold;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4, #45b7d1, #96ceb4, #feca57);
            background-size: 300% 300%;
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            animation: gradientShift 3s ease-in-out infinite, glitch 2s infinite;
            margin-bottom: 1rem;
        }

        @keyframes gradientShift {
            0%, 100% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
        }

        @keyframes glitch {
            0%, 100% { transform: translateX(0); }
            20% { transform: translateX(-2px) skew(-1deg); }
            40% { transform: translateX(2px) skew(1deg); }
            60% { transform: translateX(-1px) skew(0deg); }
            80% { transform: translateX(1px) skew(-0.5deg); }
        }

        .subtitle {
            font-size: 1.5rem;
            color: #64ffda;
            margin-bottom: 2rem;
            opacity: 0;
            animation: fadeInUp 1s ease-out 0.5s forwards;
        }

        @keyframes fadeInUp {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .matrix-rain {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        .scroll-indicator {
            position: absolute;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            animation: bounce 2s infinite;
            cursor: pointer;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% { transform: translateX(-50%) translateY(0); }
            40% { transform: translateX(-50%) translateY(-10px); }
            60% { transform: translateX(-50%) translateY(-5px); }
        }

        .section {
            min-height: 100vh;
            padding: 4rem 2rem;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .card-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        .neon-card {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 20px;
            padding: 2rem;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            cursor: pointer;
        }

        .neon-card:hover {
            transform: translateY(-10px) scale(1.02);
            box-shadow: 0 20px 40px rgba(138, 43, 226, 0.3),
                        0 0 50px rgba(64, 255, 218, 0.2);
            border-color: #40ffda;
        }

        .neon-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
            transition: left 0.5s;
        }

        .neon-card:hover::before {
            left: 100%;
        }

        .card-icon {
            font-size: 3rem;
            margin-bottom: 1rem;
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }

        .interactive-terminal {
            background: #0f0f0f;
            border-radius: 10px;
            padding: 1rem;
            margin: 2rem 0;
            max-width: 800px;
            font-family: 'Courier New', monospace;
            border: 1px solid #333;
            position: relative;
        }

        .terminal-header {
            display: flex;
            align-items: center;
            margin-bottom: 1rem;
            padding-bottom: 0.5rem;
            border-bottom: 1px solid #333;
        }

        .terminal-dots {
            display: flex;
            gap: 0.5rem;
        }

        .dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
        }
        .red { background: #ff5f56; }
        .yellow { background: #ffbd2e; }
        .green { background: #27ca3f; }

        .terminal-content {
            color: #00ff00;
        }

        .typing-animation {
            overflow: hidden;
            border-right: 2px solid #00ff00;
            white-space: nowrap;
            animation: typing 3s steps(40, end), blink-caret 0.75s step-end infinite;
        }

        @keyframes typing {
            from { width: 0; }
            to { width: 100%; }
        }

        @keyframes blink-caret {
            from, to { border-color: transparent; }
            50% { border-color: #00ff00; }
        }

        .hologram-section {
            perspective: 1000px;
            margin: 4rem 0;
        }

        .hologram-card {
            background: linear-gradient(135deg, rgba(138, 43, 226, 0.1), rgba(64, 255, 218, 0.1));
            border: 2px solid;
            border-image: linear-gradient(45deg, #8a2be2, #40ffda) 1;
            transform-style: preserve-3d;
            transition: transform 0.3s ease;
            padding: 2rem;
            border-radius: 15px;
        }

        .hologram-card:hover {
            transform: rotateY(10deg) rotateX(5deg);
        }

        .stats-bar {
            display: flex;
            gap: 2rem;
            margin: 2rem 0;
            flex-wrap: wrap;
            justify-content: center;
        }

        .stat-item {
            text-align: center;
            padding: 1rem;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
            min-width: 120px;
        }

        .stat-number {
            font-size: 2rem;
            font-weight: bold;
            color: #40ffda;
            display: block;
        }

        .floating-particles {
            position: absolute;
            width: 100%;
            height: 100%;
            overflow: hidden;
            pointer-events: none;
        }

        .particle {
            position: absolute;
            width: 4px;
            height: 4px;
            background: #40ffda;
            border-radius: 50%;
            animation: float-particle 20s linear infinite;
        }

        @keyframes float-particle {
            0% {
                transform: translateY(100vh) rotate(0deg);
                opacity: 0;
            }
            10% {
                opacity: 1;
            }
            90% {
                opacity: 1;
            }
            100% {
                transform: translateY(-100px) rotate(360deg);
                opacity: 0;
            }
        }

        .quantum-button {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border: none;
            color: white;
            padding: 12px 30px;
            border-radius: 25px;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 1rem;
            position: relative;
            overflow: hidden;
        }

        .quantum-button:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.4);
        }

        .quantum-button::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.5);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }

        .quantum-button:active::after {
            width: 300px;
            height: 300px;
        }

        .progress-ring {
            width: 120px;
            height: 120px;
            margin: 1rem;
        }

        .progress-ring__circle {
            stroke: #40ffda;
            stroke-linecap: round;
            stroke-width: 4;
            fill: transparent;
            stroke-dasharray: 283;
            stroke-dashoffset: 283;
            animation: progress 3s ease-in-out;
        }

        @keyframes progress {
            to {
                stroke-dashoffset: 70.75;
            }
        }

        .section-title {
            font-size: 3rem;
            text-align: center;
            margin-bottom: 2rem;
            background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .tech-stack {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            justify-content: center;
            margin: 2rem 0;
        }

        .tech-badge {
            padding: 0.5rem 1rem;
            background: linear-gradient(135deg, rgba(255, 107, 107, 0.2), rgba(78, 205, 196, 0.2));
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 20px;
            font-size: 0.9rem;
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .tech-badge:hover {
            transform: scale(1.1);
            background: linear-gradient(135deg, rgba(255, 107, 107, 0.4), rgba(78, 205, 196, 0.4));
        }

        @media (max-width: 768px) {
            .hero { padding: 2rem 1rem; }
            .section { padding: 2rem 1rem; }
            .card-grid { grid-template-columns: 1fr; }
            .stats-bar { flex-direction: column; align-items: center; }
        }
    </style>
</head>
<body>
    <div class="neural-bg">
        <canvas id="neuralNetwork"></canvas>
    </div>

    <div class="floating-particles" id="particles"></div>

    <section class="hero">
        <div class="matrix-rain" id="matrixRain"></div>
        <h1 class="glitch-title">🧠 ML JOURNEY</h1>
        <p class="subtitle">A Neural Odyssey Through Machine Learning</p>
        
        <div class="interactive-terminal">
            <div class="terminal-header">
                <div class="terminal-dots">
                    <div class="dot red"></div>
                    <div class="dot yellow"></div>
                    <div class="dot green"></div>
                </div>
                <span style="margin-left: 1rem; color: #888;">ml-journey@github:~$</span>
            </div>
            <div class="terminal-content">
                <div class="typing-animation" id="terminalText">git clone https://github.com/your-username/ml-journey.git</div>
            </div>
        </div>

        <div class="scroll-indicator" onclick="scrollToSection('about')">
            <svg width="30" height="40" viewBox="0 0 30 40" fill="none">
                <path d="M15 5V35M15 35L10 30M15 35L20 30" stroke="#40ffda" stroke-width="2"/>
            </svg>
        </div>
    </section>

    <section class="section" id="about">
        <h2 class="section-title">🚀 Mission Control</h2>
        
        <div class="hologram-section">
            <div class="hologram-card">
                <h3 style="color: #40ffda; margin-bottom: 1rem;">📊 Repository Stats</h3>
                <div class="stats-bar">
                    <div class="stat-item">
                        <span class="stat-number">42+</span>
                        <span>Projects</span>
                    </div>
                    <div class="stat-item">
                        <span class="stat-number">15K+</span>
                        <span>Lines of Code</span>
                    </div>
                    <div class="stat-item">
                        <span class="stat-number">∞</span>
                        <span>Learning Hours</span>
                    </div>
                    <div class="stat-item">
                        <span class="stat-number">🎯</span>
                        <span>Accuracy Goal</span>
                    </div>
                </div>
                
                <div style="text-align: center; margin: 2rem 0;">
                    <svg class="progress-ring" width="120" height="120">
                        <circle class="progress-ring__circle" stroke-width="4" fill="transparent" r="45" cx="60" cy="60"/>
                    </svg>
                    <p style="color: #64ffda;">Journey Progress: 75%</p>
                </div>
            </div>
        </div>
    </section>

    <section class="section" id="projects">
        <h2 class="section-title">🔬 Research Labs</h2>
        
        <div class="card-grid">
            <div class="neon-card" onclick="expandCard(this)">
                <div class="card-icon">🤖</div>
                <h3 style="color: #ff6b6b;">Deep Learning Arsenal</h3>
                <p>Neural networks, CNNs, RNNs, and Transformers. From MNIST to GPT-style architectures.</p>
                <div class="tech-stack">
                    <span class="tech-badge">PyTorch</span>
                    <span class="tech-badge">TensorFlow</span>
                    <span class="tech-badge">Keras</span>
                </div>
            </div>

            <div class="neon-card" onclick="expandCard(this)">
                <div class="card-icon">📈</div>
                <h3 style="color: #4ecdc4;">Data Science Experiments</h3>
                <p>EDA, feature engineering, statistical modeling, and predictive analytics.</p>
                <div class="tech-stack">
                    <span class="tech-badge">Pandas</span>
                    <span class="tech-badge">NumPy</span>
                    <span class="tech-badge">Scikit-learn</span>
                </div>
            </div>

            <div class="neon-card" onclick="expandCard(this)">
                <div class="card-icon">🎨</div>
                <h3 style="color: #45b7d1;">Computer Vision Studio</h3>
                <p>Image classification, object detection, GANs, and style transfer experiments.</p>
                <div class="tech-stack">
                    <span class="tech-badge">OpenCV</span>
                    <span class="tech-badge">PIL</span>
                    <span class="tech-badge">YOLO</span>
                </div>
            </div>

            <div class="neon-card" onclick="expandCard(this)">
                <div class="card-icon">🗣️</div>
                <h3 style="color: #96ceb4;">NLP Laboratory</h3>
                <p>Text processing, sentiment analysis, chatbots, and language model fine-tuning.</p>
                <div class="tech-stack">
                    <span class="tech-badge">NLTK</span>
                    <span class="tech-badge">spaCy</span>
                    <span class="tech-badge">Transformers</span>
                </div>
            </div>

            <div class="neon-card" onclick="expandCard(this)">
                <div class="card-icon">🎮</div>
                <h3 style="color: #feca57;">Reinforcement Learning</h3>
                <p>Q-learning, policy gradients, and game-playing AI agents.</p>
                <div class="tech-stack">
                    <span class="tech-badge">OpenAI Gym</span>
                    <span class="tech-badge">Stable Baselines</span>
                    <span class="tech-badge">Ray</span>
                </div>
            </div>

            <div class="neon-card" onclick="expandCard(this)">
                <div class="card-icon">☁️</div>
                <h3 style="color: #ff7675;">MLOps & Deployment</h3>
                <p>Model deployment, monitoring, and production ML pipelines.</p>
                <div class="tech-stack">
                    <span class="tech-badge">Docker</span>
                    <span class="tech-badge">MLflow</span>
                    <span class="tech-badge">FastAPI</span>
                </div>
            </div>
        </div>

        <div style="text-align: center; margin: 3rem 0;">
            <button class="quantum-button" onclick="showAllProjects()">🚀 Launch All Projects</button>
            <button class="quantum-button" onclick="randomProject()">🎲 Random Discovery</button>
        </div>
    </section>

    <section class="section" id="journey">
        <h2 class="section-title">🗺️ Learning Pathways</h2>
        
        <div class="interactive-terminal" style="max-width: 1000px;">
            <div class="terminal-header">
                <div class="terminal-dots">
                    <div class="dot red"></div>
                    <div class="dot yellow"></div>
                    <div class="dot green"></div>
                </div>
                <span style="margin-left: 1rem; color: #888;">learning-timeline@ml-journey:~$</span>
            </div>
            <div class="terminal-content" style="color: #00ff00; font-size: 0.9rem;">
                <div id="learningTimeline">
                    <p>📚 Phase 1: Foundations [████████████████████] 100%</p>
                    <p>🔢 Phase 2: Statistics & Math [████████████████████] 100%</p>
                    <p>🧠 Phase 3: Classical ML [███████████████████▓] 95%</p>
                    <p>🤖 Phase 4: Deep Learning [██████████████▓▓▓▓▓▓] 70%</p>
                    <p>🎯 Phase 5: Specialization [████████▓▓▓▓▓▓▓▓▓▓▓▓] 40%</p>
                    <p>🚀 Phase 6: Production ML [███▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓] 15%</p>
                </div>
            </div>
        </div>

        <div style="margin: 3rem 0; text-align: center;">
            <h3 style="color: #40ffda; margin-bottom: 2rem;">🎯 Current Focus Areas</h3>
            <div class="tech-stack">
                <span class="tech-badge" style="background: linear-gradient(135deg, rgba(255, 107, 107, 0.3), rgba(138, 43, 226, 0.3));">Transformer Architecture</span>
                <span class="tech-badge" style="background: linear-gradient(135deg, rgba(78, 205, 196, 0.3), rgba(69, 183, 209, 0.3));">Computer Vision</span>
                <span class="tech-badge" style="background: linear-gradient(135deg, rgba(150, 206, 180, 0.3), rgba(254, 202, 87, 0.3));">MLOps</span>
                <span class="tech-badge" style="background: linear-gradient(135deg, rgba(255, 118, 117, 0.3), rgba(253, 121, 168, 0.3));">Edge AI</span>
            </div>
        </div>
    </section>

    <section class="section" id="contact">
        <h2 class="section-title">🌌 Connect with the Neural Network</h2>
        
        <div class="hologram-card" style="max-width: 600px; text-align: center;">
            <p style="font-size: 1.2rem; margin-bottom: 2rem; color: #64ffda;">
                Join me in exploring the infinite possibilities of Machine Learning!
            </p>
            
            <div style="display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap;">
                <button class="quantum-button" onclick="window.open('https://github.com/your-username', '_blank')">
                    🐙 GitHub
                </button>
                <button class="quantum-button" onclick="window.open('https://linkedin.com/in/your-profile', '_blank')">
                    💼 LinkedIn
                </button>
                <button class="quantum-button" onclick="window.open('mailto:your-email@domain.com', '_blank')">
                    📧 Email
                </button>
                <button class="quantum-button" onclick="window.open('https://your-portfolio.com', '_blank')">
                    🌐 Portfolio
                </button>
            </div>
            
            <div style="margin-top: 2rem; padding: 1rem; background: rgba(0, 0, 0, 0.3); border-radius: 10px;">
                <p style="color: #888; font-size: 0.9rem;">
                    "The best way to predict the future is to create it." - Alan Kay
                </p>
            </div>
        </div>
    </section>

    <script>
        // Neural Network Background Animation
        const canvas = document.getElementById('neuralNetwork');
        const ctx = canvas.getContext('2d');
        
        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        
        resizeCanvas();
        window.addEventListener('resize', resizeCanvas);
        
        const nodes = [];
        const connections = [];
        
        for (let i = 0; i < 50; i++) {
            nodes.push({
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height,
                vx: (Math.random() - 0.5) * 0.5,
                vy: (Math.random() - 0.5) * 0.5,
                radius: Math.random() * 3 + 1
            });
        }
        
        function animate() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Update nodes
            nodes.forEach(node => {
                node.x += node.vx;
                node.y += node.vy;
                
                if (node.x < 0 || node.x > canvas.width) node.vx *= -1;
                if (node.y < 0 || node.y > canvas.height) node.vy *= -1;
                
                // Draw node
                ctx.beginPath();
                ctx.arc(node.x, node.y, node.radius, 0, Math.PI * 2);
                ctx.fillStyle = 'rgba(64, 255, 218, 0.6)';
                ctx.fill();
            });
            
            // Draw connections
            nodes.forEach((node, i) => {
                nodes.slice(i + 1).forEach(otherNode => {
                    const distance = Math.sqrt(
                        Math.pow(node.x - otherNode.x, 2) + 
                        Math.pow(node.y - otherNode.y, 2)
                    );
                    
                    if (distance < 100) {
                        ctx.beginPath();
                        ctx.moveTo(node.x, node.y);
                        ctx.lineTo(otherNode.x, otherNode.y);
                        ctx.strokeStyle = `rgba(64, 255, 218, ${1 - distance / 100})`;
                        ctx.lineWidth = 1;
                        ctx.stroke();
                    }
                });
            });
            
            requestAnimationFrame(animate);
        }
        
        animate();
        
        // Matrix Rain Effect
        const matrixCanvas = document.createElement('canvas');
        const matrixCtx = matrixCanvas.getContext('2d');
        const matrixContainer = document.getElementById('matrixRain');
        
        matrixCanvas.width = window.innerWidth;
        matrixCanvas.height = window.innerHeight;
        matrixContainer.appendChild(matrixCanvas);
        
        const matrix = "ABCDEFGHIJKLMNOPQRSTUVWXYZ123456789@#$%^&*()*&^%+-/~{[|`]}";
        const matrixArray = matrix.split("");
        
        const fontSize = 10;
        const columns = matrixCanvas.width / fontSize;
        const drops = [];
        
        for (let x = 0; x < columns; x++) {
            drops[x] = 1;
        }
        
        function drawMatrix() {
            matrixCtx.fillStyle = 'rgba(0, 0, 0, 0.05)';
            matrixCtx.fillRect(0, 0, matrixCanvas.width, matrixCanvas.height);
            
            matrixCtx.fillStyle = '#0F3';
            matrixCtx.font = fontSize + 'px monospace';
            
            for (let i = 0; i < drops.length; i++) {
                const text = matrixArray[Math.floor(Math.random() * matrixArray.length)];
                matrixCtx.fillText(text, i * fontSize, drops[i] * fontSize);
                
                if (drops[i] * fontSize > matrixCanvas.height && Math.random() > 0.975) {
                    drops[i] = 0;
                }
                drops[i]++;
            }
        }
        
        setInterval(drawMatrix, 35);
        
        // Floating Particles
        function createParticles() {
            const particleContainer = document.getElementById('particles');
            
            setInterval(() => {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.animationDuration = (Math.random() * 3 + 2) + 's';
                particleContainer.appendChild(particle);
                
                setTimeout(() => {
                    particle.remove();
                }, 5000);
            }, 300);
        }
        
        createParticles();
        
        // Interactive Functions
        function scrollToSection(sectionId) {
            document.getElementById(sectionId).scrollIntoView({ 
                behavior: 'smooth',
                block: 'start'
            });
        }
        
        function expandCard(card) {
            card.style.transform = 'scale(1.05) rotateY(5deg)';
            setTimeout(() => {
                card.style.transform = '';
            }, 300);
        }
        
        function showAllProjects() {
            alert('🚀 Launching all projects! Check out the repository folders for detailed implementations.');
        }
        
        function randomProject() {
            const projects = [
                'Neural Style Transfer',
                'Chatbot with BERT',
                'Stock Price Predictor',
                'Image Classifier',
                'Recommendation System',
                'Sentiment Analyzer'
            ];
            const randomProj = projects[Math.floor(Math.random() * projects.length)];
            alert(`🎲 Random discovery: ${randomProj}! Check it out in the projects folder.`);
        }
        
        // Terminal typing animation
        const terminalTexts = [
            'git clone https://github.com/your-username/ml-journey.git',
            'cd ml-journey && python main.py',
            'pip install -r requirements.txt',
            'jupyter notebook experiments/',
            'python train_model.py --epochs 100'
        ];
        
        let currentTextIndex = 0;
        
        function changeTerminalText() {
            const terminalElement = document.getElementById('terminalText');
            terminalElement.textContent = terminalTexts[currentTextIndex];
            terminalElement.style.animation = 'none';
            setTimeout(() => {
                terminalElement.style.animation = 'typing 3s steps(40, end), blink-caret 0.75s step-end infinite';
            }, 10);
            currentTextIndex = (currentTextIndex + 1) % terminalTexts.length;
        }
        
        setInterval(changeTerminalText, 4000);
        
        // Learning Timeline Animation
        function animateProgress() {
            const timeline = document.getElementById('learningTimeline');
            const phases = timeline.children;
            
            Array.from(phases).forEach((phase, index) => {
                setTimeout(() => {
                    phase.style.opacity = '0';
                    phase.style.transform = 'translateX(-20px)';
                    setTimeout(() => {
                        phase.style.transition = 'all 0.5s ease';
                        phase.style.opacity = '1';
                        phase.style.transform = 'translateX(0)';
                    }, 100);
                }, index * 200);
            });
        }
        
        // Intersection Observer for animations
        const observerOptions = {
            threshold: 0.1,
            rootMargin: '0px 0px -50px 0px'
        };
        
        const observer = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    if (entry.target.id === 'journey') {
                        animateProgress();
                    }
                    entry.target.style.opacity = '1';
                    entry.target.style.transform = 'translateY(0)';
                }
            });
        }, observerOptions);
        
        // Observe all sections
        document.querySelectorAll('.section').forEach(section => {
            section.style.opacity = '0';
            section.style.transform = 'translateY(30px)';
            section.style.transition = 'opacity 0.8s ease, transform 0.8s ease';
            observer.observe(section);
        });
        
        // Mouse parallax effect
        document.addEventListener('mousemove', (e) => {
            const mouseX = e.clientX / window.innerWidth;
            const mouseY = e.clientY / window.innerHeight;
            
            document.querySelectorAll('.neon-card').forEach((card, index) => {
                const speed = (index + 1) * 0.5;
                const x = (mouseX - 0.5) * speed;
                const y = (mouseY - 0.5) * speed;
                card.style.transform = `translate(${x}px, ${y}px)`;
            });
        });
        
        // Easter eggs
        let konamiCode = [];
        const konamiSequence = [38, 38, 40, 40, 37, 39, 37, 39, 66, 65]; // ↑↑↓↓←→←→BA
        
        document.addEventListener('keydown', (e) => {
            konamiCode.push(e.keyCode);
            if (konamiCode.length > konamiSequence.length) {
                konamiCode.shift();
            }
            
            if (JSON.stringify(konamiCode) === JSON.stringify(konamiSequence)) {
                activateEasterEgg();
            }
        });
        
        function activateEasterEgg() {
            document.body.style.animation = 'rainbow 2s ease infinite';
            setTimeout(() => {
                document.body.style.animation = '';
                alert('🎉 Konami Code activated! You found the hidden easter egg!');
            }, 2000);
        }
        
        // Add rainbow animation for easter egg
        const style = document.createElement('style');
        style.textContent = `
            @keyframes rainbow {
                0% { filter: hue-rotate(0deg); }
                100% { filter: hue-rotate(360deg); }
            }
        `;
        document.head.appendChild(style);
        
        // Load animations on page load
        window.addEventListener('load', () => {
            document.querySelector('.hero').style.opacity = '1';
            document.querySelector('.hero').style.transform = 'translateY(0)';
        });
        
        // Smooth scrolling for better UX
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    target.scrollIntoView({
                        behavior: 'smooth',
                        block: 'start'
                    });
                }
            });
        });
        
        // Dynamic stats counter
        function animateStats() {
            const statNumbers = document.querySelectorAll('.stat-number');
            statNumbers.forEach(stat => {
                if (stat.textContent.match(/\d+/)) {
                    const target = parseInt(stat.textContent.match(/\d+/)[0]);
                    let current = 0;
                    const increment = target / 100;
                    const timer = setInterval(() => {
                        current += increment;
                        if (current >= target) {
                            stat.textContent = stat.textContent.replace(/\d+/, target);
                            clearInterval(timer);
                        } else {
                            stat.textContent = stat.textContent.replace(/\d+/, Math.floor(current));
                        }
                    }, 20);
                }
            });
        }
        
        // Trigger stats animation when section is visible
        const statsObserver = new IntersectionObserver((entries) => {
            entries.forEach(entry => {
                if (entry.isIntersecting) {
                    animateStats();
                    statsObserver.unobserve(entry.target);
                }
            });
        });
        
        const aboutSection = document.getElementById('about');
        if (aboutSection) {
            statsObserver.observe(aboutSection);
        }
    </script>
</body>
</html>
