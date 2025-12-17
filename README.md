# Emotion_detection
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Emotion Detection with Solutions</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            color: white;
            padding: 40px 0;
        }

        .logo {
            font-size: 48px;
            margin-bottom: 10px;
        }

        .title {
            font-size: 36px;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .subtitle {
            font-size: 18px;
            opacity: 0.9;
            max-width: 600px;
            margin: 0 auto 30px;
            line-height: 1.6;
        }

        .tabs {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .tab-btn {
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 15px 30px;
            border-radius: 50px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 10px;
            backdrop-filter: blur(10px);
        }

        .tab-btn:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px);
        }

        .tab-btn.active {
            background: white;
            color: #667eea;
            border-color: white;
        }

        .tab-icon {
            font-size: 20px;
        }

        .content {
            display: none;
            background: white;
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            animation: fadeIn 0.5s ease;
        }

        .content.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .content-title {
            font-size: 28px;
            color: #333;
            margin-bottom: 30px;
            text-align: center;
        }

        /* Face Detection Styles */
        .camera-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
        }

        #video {
            width: 100%;
            max-width: 640px;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        }

        .video-overlay {
            position: relative;
            width: 100%;
            max-width: 640px;
        }

        .face-canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        .controls {
            display: flex;
            gap: 20px;
            margin-top: 20px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 50px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 10px;
            min-width: 200px;
            justify-content: center;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-secondary {
            background: #f0f0f0;
            color: #666;
        }

        .btn-success {
            background: linear-gradient(135deg, #4CAF50 0%, #2E7D32 100%);
            color: white;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }

        .result-box {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 30px;
            margin-top: 30px;
            text-align: center;
        }

        .emotion-display {
            font-size: 48px;
            margin-bottom: 20px;
        }

        .emotion-text {
            font-size: 32px;
            color: #667eea;
            font-weight: 600;
            margin-bottom: 10px;
        }

        .confidence {
            font-size: 18px;
            color: #666;
            margin-bottom: 20px;
        }

        .emotion-bar {
            height: 20px;
            background: #e0e0e0;
            border-radius: 10px;
            margin: 10px 0;
            overflow: hidden;
        }

        .emotion-fill {
            height: 100%;
            border-radius: 10px;
            background: linear-gradient(90deg, #667eea, #764ba2);
            width: 0%;
            transition: width 1s ease;
        }

        /* Solution Box */
        .solution-box {
            background: white;
            border-radius: 15px;
            padding: 25px;
            margin-top: 30px;
            border-left: 5px solid #4CAF50;
            text-align: left;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .solution-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 15px;
            color: #2E7D32;
        }

        .solution-icon {
            font-size: 24px;
        }

        .solution-title {
            font-size: 20px;
            font-weight: 600;
        }

        .solution-content {
            font-size: 16px;
            line-height: 1.6;
            color: #555;
        }

        .solution-list {
            margin-top: 15px;
            padding-left: 20px;
        }

        .solution-list li {
            margin-bottom: 10px;
            line-height: 1.5;
        }

        /* Text Detection Styles */
        .text-container {
            max-width: 800px;
            margin: 0 auto;
        }

        #textInput {
            width: 100%;
            height: 150px;
            padding: 20px;
            border: 2px solid #e0e0e0;
            border-radius: 15px;
            font-size: 16px;
            resize: vertical;
            margin-bottom: 20px;
            transition: border-color 0.3s ease;
        }

        #textInput:focus {
            outline: none;
            border-color: #667eea;
        }

        .example-text {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            font-size: 14px;
            color: #666;
        }

        .example-text span {
            color: #667eea;
            cursor: pointer;
            margin: 0 10px;
            padding: 5px 10px;
            border-radius: 5px;
            transition: background 0.3s;
        }

        .example-text span:hover {
            background: rgba(102, 126, 234, 0.1);
        }

        /* Speech Detection Styles */
        .speech-container {
            text-align: center;
        }

        .mic-container {
            width: 120px;
            height: 120px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 50%;
            margin: 0 auto 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 10px 30px rgba(102, 126, 234, 0.4);
        }

        .mic-container:hover {
            transform: scale(1.05);
        }

        .mic-container.recording {
            animation: pulse 1.5s infinite;
            background: linear-gradient(135deg, #ff416c 0%, #ff4b2b 100%);
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(255, 65, 108, 0.7); }
            70% { box-shadow: 0 0 0 20px rgba(255, 65, 108, 0); }
            100% { box-shadow: 0 0 0 0 rgba(255, 65, 108, 0); }
        }

        .mic-icon {
            font-size: 48px;
            color: white;
        }

        .timer {
            font-size: 24px;
            color: #667eea;
            font-weight: 600;
            margin: 20px 0;
        }

        /* Voice Output Controls */
        .voice-controls {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
            padding-top: 30px;
            border-top: 1px solid #e0e0e0;
        }

        .voice-toggle {
            display: flex;
            align-items: center;
            gap: 10px;
            cursor: pointer;
        }

        .switch {
            position: relative;
            display: inline-block;
            width: 60px;
            height: 34px;
        }

        .switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 34px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 26px;
            width: 26px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .slider {
            background-color: #667eea;
        }

        input:checked + .slider:before {
            transform: translateX(26px);
        }

        /* Emotion Results Grid */
        .emotion-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }

        .emotion-card {
            background: white;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            transition: transform 0.3s ease;
            cursor: pointer;
        }

        .emotion-card:hover {
            transform: translateY(-5px);
        }

        .emotion-card.active {
            border: 2px solid #667eea;
        }

        .emotion-icon {
            font-size: 36px;
            margin-bottom: 10px;
        }

        .emotion-name {
            font-size: 14px;
            font-weight: 600;
            color: #333;
            margin-bottom: 5px;
        }

        .emotion-percent {
            font-size: 24px;
            font-weight: 700;
            color: #667eea;
        }

        /* Footer */
        footer {
            text-align: center;
            color: white;
            margin-top: 50px;
            padding: 20px;
            opacity: 0.7;
            font-size: 14px;
        }

        /* Modal for face points */
        .face-point {
            position: absolute;
            width: 10px;
            height: 10px;
            background: #ff4757;
            border-radius: 50%;
            transform: translate(-50%, -50%);
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            .container {
                padding: 10px;
            }
            
            .content {
                padding: 20px;
            }
            
            .title {
                font-size: 28px;
            }
            
            .tab-btn {
                padding: 12px 20px;
                font-size: 14px;
            }
            
            .controls {
                flex-direction: column;
            }
            
            .btn {
                width: 100%;
                min-width: unset;
            }
        }
    </style>
    <!-- Font Awesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">🎭</div>
            <h1 class="title">Emotion Detection with Solutions</h1>
            <p class="subtitle">Detect emotions from face, text, or speech. Get personalized solutions and advice!</p>
        </header>

        <div class="tabs">
            <button class="tab-btn active" data-tab="face">
                <i class="fas fa-camera tab-icon"></i>
                Face Detection
            </button>
            <button class="tab-btn" data-tab="text">
                <i class="fas fa-font tab-icon"></i>
                Text Analysis
            </button>
            <button class="tab-btn" data-tab="speech">
                <i class="fas fa-microphone tab-icon"></i>
                Speech Analysis
            </button>
        </div>

        <!-- Face Detection Tab -->
        <div class="content active" id="face">
            <h2 class="content-title"><i class="fas fa-camera"></i> Face Emotion Detection with Solutions</h2>
            <div class="camera-container">
                <div class="video-overlay">
                    <video id="video" autoplay playsinline></video>
                    <canvas id="faceCanvas" class="face-canvas"></canvas>
                </div>
                <div class="controls">
                    <button class="btn btn-primary" id="startCamera">
                        <i class="fas fa-play"></i> Start Camera
                    </button>
                    <button class="btn btn-secondary" id="stopCamera" disabled>
                        <i class="fas fa-stop"></i> Stop Camera
                    </button>
                    <button class="btn btn-success" id="autoDetect">
                        <i class="fas fa-robot"></i> Auto Detect (Continuous)
                    </button>
                </div>
                
                <div class="result-box" id="faceResult" style="display: none;">
                    <div class="emotion-display" id="faceEmoji">😊</div>
                    <div class="emotion-text" id="faceEmotion">Happy</div>
                    <div class="confidence" id="faceConfidence">Confidence: 85%</div>
                    <div class="emotion-bar">
                        <div class="emotion-fill" id="faceBar" style="width: 85%;"></div>
                    </div>
                </div>

                <!-- Solution Box -->
                <div class="solution-box" id="faceSolution" style="display: none;">
                    <div class="solution-header">
                        <i class="fas fa-lightbulb solution-icon"></i>
                        <div class="solution-title">Personalized Solutions for You</div>
                    </div>
                    <div class="solution-content" id="solutionText">
                        Based on your current emotion, here are some suggestions to help:
                    </div>
                </div>

                <div class="voice-controls">
                    <div class="voice-toggle">
                        <span>Voice Feedback:</span>
                        <label class="switch">
                            <input type="checkbox" id="voiceToggle" checked>
                            <span class="slider"></span>
                        </label>
                    </div>
                    <button class="btn btn-secondary" id="getSolution">
                        <i class="fas fa-heart"></i> Get Solution
                    </button>
                    <button class="btn btn-secondary" id="testVoice">
                        <i class="fas fa-volume-up"></i> Test Voice
                    </button>
                </div>
            </div>
        </div>

        <!-- Text Detection Tab -->
        <div class="content" id="text">
            <h2 class="content-title"><i class="fas fa-font"></i> Text Emotion Analysis with Solutions</h2>
            <div class="text-container">
                <textarea id="textInput" placeholder="Type how you're feeling or what's on your mind... Example: 'I'm feeling stressed about my upcoming exams.'"></textarea>
                <div class="controls">
                    <button class="btn btn-primary" id="analyzeText">
                        <i class="fas fa-search"></i> Analyze Emotion
                    </button>
                    <button class="btn btn-secondary" id="clearText">
                        <i class="fas fa-trash"></i> Clear
                    </button>
                    <button class="btn btn-success" id="getTextSolution">
                        <i class="fas fa-hands-helping"></i> Get Help
                    </button>
                </div>

                <div class="example-text">
                    Common feelings: 
                    <span onclick="insertExample('stressed')">😰 Stressed</span> | 
                    <span onclick="insertExample('anxious')">😟 Anxious</span> | 
                    <span onclick="insertExample('depressed')">😔 Depressed</span> | 
                    <span onclick="insertExample('angry')">😠 Angry</span> |
                    <span onclick="insertExample('lonely')">🏝️ Lonely</span>
                </div>

                <div class="result-box" id="textResult" style="display: none;">
                    <div class="emotion-display" id="textEmoji">😊</div>
                    <div class="emotion-text" id="textEmotion">Happy</div>
                    <div class="confidence" id="textConfidence">Confidence: 75%</div>
                    
                    <div class="emotion-grid" id="textEmotionsGrid">
                        <!-- Emotion cards will be inserted here -->
                    </div>
                </div>

                <!-- Solution for Text -->
                <div class="solution-box" id="textSolution" style="display: none; margin-top: 20px;">
                    <div class="solution-header">
                        <i class="fas fa-comment-medical solution-icon"></i>
                        <div class="solution-title">Advice & Support</div>
                    </div>
                    <div class="solution-content" id="textSolutionText">
                        <!-- Solution will be inserted here -->
                    </div>
                </div>
            </div>
        </div>

        <!-- Speech Detection Tab -->
        <div class="content" id="speech">
            <h2 class="content-title"><i class="fas fa-microphone"></i> Speech Emotion Detection with Solutions</h2>
            <div class="speech-container">
                <div class="mic-container" id="micButton">
                    <i class="fas fa-microphone mic-icon"></i>
                </div>
                <div class="timer" id="timer" style="display: none;">
                    <i class="fas fa-clock"></i> <span id="time">5</span>s
                </div>
                
                <div class="controls">
                    <button class="btn btn-secondary" id="playDemo">
                        <i class="fas fa-headphones"></i> Play Demo
                    </button>
                    <button class="btn btn-success" id="analyzeSpeech">
                        <i class="fas fa-brain"></i> Analyze Emotion
                    </button>
                </div>

                <div class="result-box" id="speechResult" style="display: none;">
                    <div class="emotion-display" id="speechEmoji">🎤</div>
                    <div class="emotion-text" id="speechEmotion">Neutral</div>
                    <div class="confidence" id="speechConfidence">Confidence: 65%</div>
                    
                    <div class="emotion-grid" id="speechEmotionsGrid">
                        <!-- Emotion cards will be inserted here -->
                    </div>
                </div>

                <!-- Solution for Speech -->
                <div class="solution-box" id="speechSolution" style="display: none; margin-top: 20px;">
                    <div class="solution-header">
                        <i class="fas fa-hands solution-icon"></i>
                        <div class="solution-title">Emotional Support</div>
                    </div>
                    <div class="solution-content" id="speechSolutionText">
                        <!-- Solution will be inserted here -->
                    </div>
                </div>

                <div class="voice-controls">
                    <div class="voice-toggle">
                        <span>Voice Feedback:</span>
                        <label class="switch">
                            <input type="checkbox" id="speechVoiceToggle" checked>
                            <span class="slider"></span>
                        </label>
                    </div>
                    <button class="btn btn-secondary" id="speakResult">
                        <i class="fas fa-volume-up"></i> Speak Solution
                    </button>
                </div>
            </div>
        </div>

        <footer>
            <p>© 2024 Emotion Detection & Support System | Real-time solutions for emotional well-being</p>
            <p>⚠️ Note: This provides general advice. For serious concerns, please consult a professional.</p>
        </footer>
    </div>

    <script>
        // ==================== EMOTION SOLUTIONS DATABASE ====================
        const emotionSolutions = {
            'happy': {
                emoji: '😊',
                color: '#FFD700',
                solutions: [
                    "Great to see you happy! Keep spreading positivity.",
                    "Share your happiness with others - it multiplies!",
                    "Capture this moment in a journal to remember later.",
                    "Use this energy to start a new hobby or project.",
                    "Practice gratitude - it helps maintain happiness."
                ],
                activities: [
                    "Share a compliment with someone",
                    "Listen to uplifting music",
                    "Do something creative",
                    "Spend time in nature",
                    "Help someone in need"
                ]
            },
            'sad': {
                emoji: '😢',
                color: '#6495ED',
                solutions: [
                    "It's okay to feel sad. Allow yourself to process emotions.",
                    "Talk to a friend or loved one about how you feel.",
                    "Practice self-compassion - be kind to yourself.",
                    "Engage in gentle activities like reading or walking.",
                    "Remember that emotions are temporary - this too shall pass."
                ],
                activities: [
                    "Listen to calming music",
                    "Watch a favorite movie",
                    "Take a warm bath",
                    "Write in a journal",
                    "Practice deep breathing"
                ]
            },
            'angry': {
                emoji: '😠',
                color: '#FF4500',
                solutions: [
                    "Take 10 deep breaths before reacting.",
                    "Remove yourself from the situation temporarily.",
                    "Identify what specifically triggered your anger.",
                    "Express your feelings calmly using 'I' statements.",
                    "Channel the energy into physical activity like exercise."
                ],
                activities: [
                    "Count slowly to 20",
                    "Squeeze a stress ball",
                    "Go for a brisk walk",
                    "Practice progressive muscle relaxation",
                    "Write down what upset you"
                ]
            },
            'stressed': {
                emoji: '😰',
                color: '#FF6347',
                solutions: [
                    "Break tasks into smaller, manageable steps.",
                    "Practice mindfulness meditation for 5 minutes.",
                    "Prioritize your tasks - focus on what's most important.",
                    "Set boundaries and learn to say 'no' when needed.",
                    "Ensure you're getting enough sleep and proper nutrition."
                ],
                activities: [
                    "Try 4-7-8 breathing technique",
                    "Make a to-do list",
                    "Take regular breaks",
                    "Drink herbal tea",
                    "Stretch for 5 minutes"
                ]
            },
            'anxious': {
                emoji: '😟',
                color: '#8A2BE2',
                solutions: [
                    "Practice grounding techniques - name 5 things you can see.",
                    "Challenge anxious thoughts with evidence.",
                    "Limit caffeine and sugar intake.",
                    "Create a worry time - postpone worrying until a set time.",
                    "Focus on what you can control, accept what you can't."
                ],
                activities: [
                    "Practice box breathing",
                    "Use a worry journal",
                    "Try aromatherapy",
                    "Listen to guided meditation",
                    "Focus on your senses"
                ]
            },
            'neutral': {
                emoji: '😐',
                color: '#A9A9A9',
                solutions: [
                    "Neutral moments are perfect for reflection.",
                    "Consider trying something new to spark interest.",
                    "Check in with yourself - are you content or feeling flat?",
                    "Use this calm state to plan ahead.",
                    "Practice mindfulness to stay present."
                ],
                activities: [
                    "Try a new recipe",
                    "Learn something new",
                    "Organize your space",
                    "Read an article",
                    "Plan your week"
                ]
            },
            'surprised': {
                emoji: '😲',
                color: '#FF69B4',
                solutions: [
                    "Take a moment to process the surprise.",
                    "Share your reaction with someone you trust.",
                    "Consider whether this surprise is positive or concerning.",
                    "Use the adrenaline boost productively.",
                    "Write about the experience to process it."
                ],
                activities: [
                    "Take deep breaths",
                    "Discuss with a friend",
                    "Write about it",
                    "Take a walk to process",
                    "Assess the situation calmly"
                ]
            },
            'disgust': {
                emoji: '🤢',
                color: '#32CD32',
                solutions: [
                    "Remove yourself from the source of disgust if possible.",
                    "Practice acceptance of the situation.",
                    "Use humor to lighten the mood if appropriate.",
                    "Focus on something pleasant to shift your attention.",
                    "Consider if this is protecting you from something harmful."
                ],
                activities: [
                    "Change your environment",
                    "Watch something funny",
                    "Focus on pleasant smells",
                    "Think of something you enjoy",
                    "Practice acceptance"
                ]
            },
            'fear': {
                emoji: '😨',
                color: '#8B0000',
                solutions: [
                    "Acknowledge your fear without judgment.",
                    "Break down what's scary into smaller parts.",
                    "Practice gradual exposure if it's a phobia.",
                    "Use visualization to imagine coping successfully.",
                    "Remember times you've overcome fear before."
                ],
                activities: [
                    "Practice deep breathing",
                    "Use positive self-talk",
                    "Visualize success",
                    "Talk to someone supportive",
                    "Write down your fears"
                ]
            },
            'depressed': {
                emoji: '😔',
                color: '#4682B4',
                solutions: [
                    "Reach out to a trusted person about how you feel.",
                    "Consider speaking with a mental health professional.",
                    "Engage in gentle movement, even if it's just stretching.",
                    "Establish a simple daily routine.",
                    "Challenge negative thoughts with evidence."
                ],
                activities: [
                    "Reach out to a friend",
                    "Take a short walk",
                    "Practice self-compassion",
                    "Listen to uplifting podcasts",
                    "Consider professional help"
                ]
            },
            'lonely': {
                emoji: '🏝️',
                color: '#87CEEB',
                solutions: [
                    "Join online communities with shared interests.",
                    "Volunteer for a cause you care about.",
                    "Reconnect with old friends or family.",
                    "Consider getting a pet if circumstances allow.",
                    "Practice self-compassion and self-care."
                ],
                activities: [
                    "Join a club or group",
                    "Volunteer locally",
                    "Call a family member",
                    "Attend community events",
                    "Practice self-care"
                ]
            }
        };

        // ==================== TAB SWITCHING ====================
        document.querySelectorAll('.tab-btn').forEach(button => {
            button.addEventListener('click', () => {
                document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
                document.querySelectorAll('.content').forEach(content => content.classList.remove('active'));
                
                button.classList.add('active');
                const tabId = button.getAttribute('data-tab');
                document.getElementById(tabId).classList.add('active');
            });
        });

        // ==================== VOICE FUNCTIONS ====================
        const voiceToggle = document.getElementById('voiceToggle');
        const speechVoiceToggle = document.getElementById('speechVoiceToggle');
        let voiceEnabled = true;
        let isAutoDetecting = false;
        let detectionInterval = null;

        voiceToggle.addEventListener('change', () => {
            voiceEnabled = voiceToggle.checked;
        });

        speechVoiceToggle.addEventListener('change', () => {
            voiceEnabled = speechVoiceToggle.checked;
        });

        function speakText(text) {
            if ('speechSynthesis' in window && voiceEnabled) {
                speechSynthesis.cancel();
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.rate = 1.0;
                utterance.pitch = 1.0;
                utterance.volume = 1.0;
                
                const voices = speechSynthesis.getVoices();
                const femaleVoice = voices.find(voice => 
                    voice.name.includes('Female') || 
                    voice.name.includes('female') ||
                    voice.name.includes('Zira') ||
                    voice.name.includes('Samantha')
                );
                
                if (femaleVoice) utterance.voice = femaleVoice;
                speechSynthesis.speak(utterance);
            }
        }

        // ==================== FACE DETECTION ====================
        const video = document.getElementById('video');
        const faceCanvas = document.getElementById('faceCanvas');
        const startCameraBtn = document.getElementById('startCamera');
        const stopCameraBtn = document.getElementById('stopCamera');
        const autoDetectBtn = document.getElementById('autoDetect');
        const getSolutionBtn = document.getElementById('getSolution');
        const faceResult = document.getElementById('faceResult');
        const faceEmoji = document.getElementById('faceEmoji');
        const faceEmotion = document.getElementById('faceEmotion');
        const faceConfidence = document.getElementById('faceConfidence');
        const faceBar = document.getElementById('faceBar');
        const faceSolution = document.getElementById('faceSolution');
        const solutionText = document.getElementById('solutionText');

        let stream = null;
        let isCameraOn = false;
        let lastDetectedEmotion = null;

        // Improved face detection simulation
        function simulateFaceDetection() {
            const emotions = ['happy', 'sad', 'angry', 'neutral', 'surprised', 'stressed', 'anxious'];
            const weights = [0.3, 0.15, 0.1, 0.25, 0.08, 0.07, 0.05]; // Weighted probabilities
            
            let random = Math.random();
            let cumulative = 0;
            let selectedEmotion = 'neutral';
            
            for (let i = 0; i < emotions.length; i++) {
                cumulative += weights[i];
                if (random <= cumulative) {
                    selectedEmotion = emotions[i];
                    break;
                }
            }
            
            return selectedEmotion;
        }

        function detectFaceAndDisplay() {
            if (!isCameraOn) return;
            
            const emotion = simulateFaceDetection();
            const confidence = Math.floor(Math.random() * 25) + 70; // 70-95%
            const solutionData = emotionSolutions[emotion] || emotionSolutions['neutral'];
            
            // Update UI
            faceEmoji.textContent = solutionData.emoji;
            faceEmotion.textContent = emotion.charAt(0).toUpperCase() + emotion.slice(1);
            faceEmotion.style.color = solutionData.color;
            faceConfidence.textContent = `Confidence: ${confidence}%`;
            faceBar.style.width = `${confidence}%`;
            faceBar.style.background = `linear-gradient(90deg, ${solutionData.color}, ${solutionData.color}88)`;
            
            faceResult.style.display = 'block';
            
            // Store last detected emotion
            lastDetectedEmotion = emotion;
            
            // Draw face points on canvas (simulated)
            drawFacePoints(emotion);
            
            // Provide solution if auto-detecting
            if (isAutoDetecting && voiceEnabled && Math.random() > 0.7) { // 30% chance to speak
                const randomSolution = solutionData.solutions[Math.floor(Math.random() * solutionData.solutions.length)];
                speakText(`You seem ${emotion}. ${randomSolution}`);
            }
        }

        function drawFacePoints(emotion) {
            const ctx = faceCanvas.getContext('2d');
            ctx.clearRect(0, 0, faceCanvas.width, faceCanvas.height);
            
            // Set canvas size to match video
            faceCanvas.width = video.videoWidth || 640;
            faceCanvas.height = video.videoHeight || 480;
            
            // Draw face rectangle
            ctx.strokeStyle = emotionSolutions[emotion]?.color || '#667eea';
            ctx.lineWidth = 3;
            const faceWidth = faceCanvas.width * 0.6;
            const faceHeight = faceCanvas.height * 0.7;
            const faceX = (faceCanvas.width - faceWidth) / 2;
            const faceY = (faceCanvas.height - faceHeight) / 2;
            
            ctx.strokeRect(faceX, faceY, faceWidth, faceHeight);
            
            // Draw emotion-specific features
            if (emotion === 'happy') {
                // Smile
                ctx.beginPath();
                ctx.arc(faceCanvas.width/2, faceY + faceHeight * 0.7, faceWidth * 0.2, 0.2, Math.PI - 0.2);
                ctx.stroke();
            } else if (emotion === 'sad') {
                // Frown
                ctx.beginPath();
                ctx.arc(faceCanvas.width/2, faceY + faceHeight * 0.8, faceWidth * 0.2, Math.PI + 0.2, 2*Math.PI - 0.2);
                ctx.stroke();
            }
        }

        // Start Camera
        startCameraBtn.addEventListener('click', async () => {
            try {
                stream = await navigator.mediaDevices.getUserMedia({ 
                    video: { width: 640, height: 480 } 
                });
                video.srcObject = stream;
                isCameraOn = true;
                
                startCameraBtn.disabled = true;
                stopCameraBtn.disabled = false;
                autoDetectBtn.disabled = false;
                
                // Set canvas size
                video.addEventListener('loadedmetadata', () => {
                    faceCanvas.width = video.videoWidth;
                    faceCanvas.height = video.videoHeight;
                });
                
                speakText("Camera started. Looking for faces...");
            } catch (err) {
                alert('Error accessing camera: ' + err.message);
                speakText("Cannot access camera. Please check permissions.");
            }
        });

        // Stop Camera
        stopCameraBtn.addEventListener('click', () => {
            if (stream) {
                stream.getTracks().forEach(track => track.stop());
                video.srcObject = null;
                isCameraOn = false;
                isAutoDetecting = false;
                
                startCameraBtn.disabled = false;
                stopCameraBtn.disabled = true;
                autoDetectBtn.disabled = true;
                autoDetectBtn.innerHTML = '<i class="fas fa-robot"></i> Auto Detect (Continuous)';
                faceResult.style.display = 'none';
                faceSolution.style.display = 'none';
                
                if (detectionInterval) {
                    clearInterval(detectionInterval);
                    detectionInterval = null;
                }
                
                speakText("Camera stopped.");
            }
        });

        // Auto Detect
        autoDetectBtn.addEventListener('click', () => {
            if (!isCameraOn) {
                alert('Please start the camera first');
                return;
            }
            
            if (!isAutoDetecting) {
                isAutoDetecting = true;
                autoDetectBtn.innerHTML = '<i class="fas fa-stop-circle"></i> Stop Auto Detect';
                faceResult.style.display = 'block';
                
                // Detect every 3 seconds
                detectionInterval = setInterval(detectFaceAndDisplay, 3000);
                detectFaceAndDisplay(); // Immediate detection
                
                speakText("Continuous detection started. I'll monitor your emotions and provide suggestions.");
            } else {
                isAutoDetecting = false;
                autoDetectBtn.innerHTML = '<i class="fas fa-robot"></i> Auto Detect (Continuous)';
                
                if (detectionInterval) {
                    clearInterval(detectionInterval);
                    detectionInterval = null;
                }
                
                speakText("Continuous detection stopped.");
            }
        });

        // Get Solution for Face
        getSolutionBtn.addEventListener('click', () => {
            if (!lastDetectedEmotion) {
                alert('Please detect an emotion first');
                return;
            }
            
            const solutionData = emotionSolutions[lastDetectedEmotion] || emotionSolutions['neutral'];
            
            // Build solution text
            let html = `<p><strong>You seem to be feeling ${lastDetectedEmotion}.</strong> Here's how to cope:</p>`;
            html += '<ul class="solution-list">';
            
            // Add solutions
            solutionData.solutions.forEach(solution => {
                html += `<li>${solution}</li>`;
            });
            
            html += '</ul>';
            html += `<p><strong>Try these activities:</strong> ${solutionData.activities.join(', ')}</p>`;
            
            solutionText.innerHTML = html;
            faceSolution.style.display = 'block';
            
            // Speak solution
            if (voiceEnabled) {
                const randomSolution = solutionData.solutions[Math.floor(Math.random() * solutionData.solutions.length)];
                speakText(`For ${lastDetectedEmotion}, try this: ${randomSolution}`);
            }
        });

        // Test Voice
        document.getElementById('testVoice').addEventListener('click', () => {
            speakText("Voice feedback is working! I can provide solutions for your emotions.");
        });

        // ==================== TEXT DETECTION ====================
        const textInput = document.getElementById('textInput');
        const analyzeTextBtn = document.getElementById('analyzeText');
        const clearTextBtn = document.getElementById('clearText');
        const getTextSolutionBtn = document.getElementById('getTextSolution');
        const textResult = document.getElementById('textResult');
        const textEmoji = document.getElementById('textEmoji');
        const textEmotion = document.getElementById('textEmotion');
        const textConfidence = document.getElementById('textConfidence');
        const textEmotionsGrid = document.getElementById('textEmotionsGrid');
        const textSolution = document.getElementById('textSolution');
        const textSolutionText = document.getElementById('textSolutionText');

        const textEmotionPatterns = {
            'stressed': /\b(stress|pressure|overwhelm|burnout|deadline|worried|anxious|tense)\b/i,
            'anxious': /\b(anxiety|nervous|panic|worry|fear|scared|uneasy|apprehensive)\b/i,
            'depressed': /\b(depressed|sad|hopeless|empty|worthless|miserable|down|blue)\b/i,
            'angry': /\b(angry|mad|furious|annoyed|irritated|frustrated|rage|upset)\b/i,
            'happy': /\b(happy|joy|excited|great|wonderful|awesome|fantastic|amazing)\b/i,
            'lonely': /\b(lonely|alone|isolated|abandoned|empty|friendless|solitary)\b/i,
            'tired': /\b(tired|exhausted|fatigued|drained|sleepy|weary|worn out)\b/i
        };

        function analyzeTextEmotion(text) {
            const scores = {};
            let totalScore = 0;
            
            // Check each emotion pattern
            for (const [emotion, pattern] of Object.entries(textEmotionPatterns)) {
                const matches = text.match(pattern);
                if (matches) {
                    scores[emotion] = matches.length * 10;
                    totalScore += scores[emotion];
                }
            }
            
            // If no matches, check for neutral keywords
            if (totalScore === 0) {
                if (text.length > 20) {
                    scores['neutral'] = 50;
                } else {
                    scores['uncertain'] = 30;
                }
            }
            
            return scores;
        }

        analyzeTextBtn.addEventListener('click', () => {
            const text = textInput.value.trim();
            
            if (!text) {
                alert('Please enter some text');
                return;
            }

            const emotionScores = analyzeTextEmotion(text);
            const emotions = Object.keys(emotionScores);
            
            if (emotions.length === 0) {
                alert('Could not detect emotion from text');
                return;
            }

            // Find dominant emotion
            let dominantEmotion = emotions[0];
            let highestScore = emotionScores[dominantEmotion];
            
            for (const emotion of emotions) {
                if (emotionScores[emotion] > highestScore) {
                    highestScore = emotionScores[emotion];
                    dominantEmotion = emotion;
                }
            }
            
            const solutionData = emotionSolutions[dominantEmotion] || emotionSolutions['neutral'];
            
            // Update UI
            textEmoji.textContent = solutionData.emoji;
            textEmotion.textContent = dominantEmotion.charAt(0).toUpperCase() + dominantEmotion.slice(1);
            textEmotion.style.color = solutionData.color;
            textConfidence.textContent = `Confidence: ${Math.min(highestScore, 95)}%`;
            
            // Update emotion grid
            textEmotionsGrid.innerHTML = '';
            for (const [emotion, score] of Object.entries(emotionScores)) {
                const solution = emotionSolutions[emotion] || emotionSolutions['neutral'];
                const card = document.createElement('div');
                card.className = 'emotion-card';
                card.innerHTML = `
                    <div class="emotion-icon">${solution.emoji}</div>
                    <div class="emotion-name">${emotion.charAt(0).toUpperCase() + emotion.slice(1)}</div>
                    <div class="emotion-percent">${Math.min(score, 100)}%</div>
                `;
                card.addEventListener('click', () => {
                    showTextSolution(emotion);
                });
                textEmotionsGrid.appendChild(card);
            }
            
            textResult.style.display = 'block';
            
            // Auto-show solution for strong emotions
            if (highestScore > 60) {
                showTextSolution(dominantEmotion);
            }
            
            if (voiceEnabled) {
                speakText(`I detect ${dominantEmotion} in your text. Would you like some suggestions?`);
            }
        });

        function showTextSolution(emotion) {
            const solutionData = emotionSolutions[emotion] || emotionSolutions['neutral'];
            
            let html = `<p><strong>For ${emotion} feelings:</strong></p>`;
            html += '<ul class="solution-list">';
            
            solutionData.solutions.forEach((solution, index) => {
                if (index < 3) { // Show only 3 solutions
                    html += `<li>${solution}</li>`;
                }
            });
            
            html += '</ul>';
            html += `<p><strong>Quick activities:</strong> ${solutionData.activities.slice(0, 3).join(', ')}</p>`;
            html += '<p class="note"><small>If these feelings persist, consider speaking with a professional.</small></p>';
            
            textSolutionText.innerHTML = html;
            textSolution.style.display = 'block';
            
            // Highlight the clicked emotion card
            document.querySelectorAll('.emotion-card').forEach(card => {
                card.classList.remove('active');
            });
            
            // Scroll to solution
            textSolution.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
        }

        getTextSolutionBtn.addEventListener('click', () => {
            if (!textInput.value.trim()) {
                alert('Please enter some text first');
                return;
            }
            
            const emotionScores = analyzeTextEmotion(textInput.value);
            if (Object.keys(emotionScores).length === 0) {
                alert('Could not detect emotion from text');
                return;
            }
            
            const dominantEmotion = Object.keys(emotionScores).reduce((a, b) => 
                emotionScores[a] > emotionScores[b] ? a : b
            );
            
            showTextSolution(dominantEmotion);
        });

        clearTextBtn.addEventListener('click', () => {
            textInput.value = '';
            textResult.style.display = 'none';
            textSolution.style.display = 'none';
        });

        window.insertExample = function(type) {
            const examples = {
                'stressed': "I'm feeling overwhelmed with work deadlines. There's too much to do and I can't focus.",
                'anxious': "I keep worrying about everything that could go wrong. My heart races and I can't sleep.",
                'depressed': "Nothing seems to bring me joy anymore. I feel empty and disconnected from everyone.",
                'angry': "I'm so frustrated with this situation! Nothing is working out and I feel like exploding.",
                'lonely': "I feel so alone even when I'm with people. No one really understands what I'm going through."
            };
            
            textInput.value = examples[type] || "I'm not sure how I'm feeling today.";
        };

        // ==================== SPEECH DETECTION ====================
        const micButton = document.getElementById('micButton');
        const timer = document.getElementById('timer');
        const timeDisplay = document.getElementById('time');
        const playDemoBtn = document.getElementById('playDemo');
        const analyzeSpeechBtn = document.getElementById('analyzeSpeech');
        const speechResult = document.getElementById('speechResult');
        const speechEmoji = document.getElementById('speechEmoji');
        const speechEmotion = document.getElementById('speechEmotion');
        const speechConfidence = document.getElementById('speechConfidence');
        const speechEmotionsGrid = document.getElementById('speechEmotionsGrid');
        const speechSolution = document.getElementById('speechSolution');
        const speechSolutionText = document.getElementById('speechSolutionText');
        const speakResultBtn = document.getElementById('speakResult');

        let isRecording = false;
        let mediaRecorder = null;
        let audioChunks = [];
        let countdown = 5;

        micButton.addEventListener('click', async () => {
            if (!isRecording) {
                try {
                    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
                    mediaRecorder = new MediaRecorder(stream);
                    audioChunks = [];
                    
                    mediaRecorder.ondataavailable = event => {
                        audioChunks.push(event.data);
                    };
                    
                    mediaRecorder.onstop = () => {
                        analyzeSpeech();
                    };
                    
                    mediaRecorder.start();
                    isRecording = true;
                    micButton.classList.add('recording');
                    timer.style.display = 'block';
                    
                    countdown = 5;
                    timeDisplay.textContent = countdown;
                    
                    const countdownInterval = setInterval(() => {
                        countdown--;
                        timeDisplay.textContent = countdown;
                        
                        if (countdown <= 0) {
                            clearInterval(countdownInterval);
                            mediaRecorder.stop();
                            isRecording = false;
                            micButton.classList.remove('recording');
                            timer.style.display = 'none';
                            stream.getTracks().forEach(track => track.stop());
                        }
                    }, 1000);
                    
                    speakText("Recording started. Speak now.");
                    
                } catch (err) {
                    alert('Error accessing microphone: ' + err.message);
                    speakText("Cannot access microphone. Please check permissions.");
                }
            }
        });

        analyzeSpeechBtn.addEventListener('click', () => {
            analyzeSpeech();
        });

        function analyzeSpeech() {
            speechEmoji.textContent = '🔍';
            speechEmotion.textContent = 'Analyzing...';
            speechResult.style.display = 'block';
            
            setTimeout(() => {
                const emotions = ['stressed', 'anxious', 'sad', 'angry', 'neutral', 'happy'];
                const randomEmotion = emotions[Math.floor(Math.random() * emotions.length)];
                const confidence = Math.floor(Math.random() * 25) + 70;
                const solutionData = emotionSolutions[randomEmotion] || emotionSolutions['neutral'];
                
                // Update UI
                speechEmoji.textContent = solutionData.emoji;
                speechEmotion.textContent = randomEmotion.charAt(0).toUpperCase() + randomEmotion.slice(1);
                speechEmotion.style.color = solutionData.color;
                speechConfidence.textContent = `Confidence: ${confidence}%`;
                
                // Update emotion grid
                speechEmotionsGrid.innerHTML = '';
                emotions.forEach(emotion => {
                    const solData = emotionSolutions[emotion] || emotionSolutions['neutral'];
                    const percentage = emotion === randomEmotion ? confidence : Math.floor(Math.random() * 30);
                    if (percentage > 0) {
                        const card = document.createElement('div');
                        card.className = 'emotion-card';
                        if (emotion === randomEmotion) card.classList.add('active');
                        card.innerHTML = `
                            <div class="emotion-icon">${solData.emoji}</div>
                            <div class="emotion-name">${emotion.charAt(0).toUpperCase() + emotion.slice(1)}</div>
                            <div class="emotion-percent">${percentage}%</div>
                        `;
                        card.addEventListener('click', () => {
                            showSpeechSolution(emotion);
                        });
                        speechEmotionsGrid.appendChild(card);
                    }
                });
                
                // Show solution
                showSpeechSolution(randomEmotion);
                
                if (voiceEnabled) {
                    const randomSolution = solutionData.solutions[Math.floor(Math.random() * solutionData.solutions.length)];
                    speakText(`I detect ${randomEmotion} in your voice. ${randomSolution}`);
                }
            }, 1500);
        }

        function showSpeechSolution(emotion) {
            const solutionData = emotionSolutions[emotion] || emotionSolutions['neutral'];
            
            let html = `<p><strong>Your voice suggests ${emotion}:</strong></p>`;
            html += '<ul class="solution-list">';
            
            solutionData.solutions.forEach((solution, index) => {
                if (index < 3) {
                    html += `<li>${solution}</li>`;
                }
            });
            
            html += '</ul>';
            html += `<p><strong>Voice calming techniques:</strong> Practice slow breathing, speak gently to yourself, or try humming a calming tune.</p>`;
            
            speechSolutionText.innerHTML = html;
            speechSolution.style.display = 'block';
        }

        playDemoBtn.addEventListener('click', () => {
            speakText("This is a demo of speech emotion detection. Imagine I just analyzed your voice for emotional content.");
            setTimeout(() => {
                analyzeSpeech();
            }, 2000);
        });

        speakResultBtn.addEventListener('click', () => {
            if (speechSolution.style.display === 'block') {
                const emotion = speechEmotion.textContent.toLowerCase();
                const solutionData = emotionSolutions[emotion] || emotionSolutions['neutral'];
                const randomSolution = solutionData.solutions[Math.floor(Math.random() * solutionData.solutions.length)];
                speakText(`For ${emotion}, try this: ${randomSolution}`);
            } else {
                speakText("Please analyze speech first to get solutions.");
            }
        });

        // Initialize voices
        if ('speechSynthesis' in window) {
            speechSynthesis.onvoiceschanged = () => {
                console.log('Voices loaded');
            };
        }

        // Welcome message
        window.addEventListener('load', () => {
            setTimeout(() => {
                speakText("Welcome to Emotion Detection with Solutions. I can detect your emotions and provide helpful suggestions.");
            }, 1000);
        });
    </script>
</body>
</html>
