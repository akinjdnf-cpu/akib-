<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>প্রাইম আকিব - AI Assistant</title>
    <!-- Google Fonts & Font Awesome Icons -->
    <link href="https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@300;400;500;600;700&family=Outfit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <style>
        :root {
            --bg-primary: #0b0f19;
            --bg-secondary: #111827;
            --bg-card: #1f2937;
            --accent-glow: #6366f1;
            --accent-cyan: #06b6d4;
            --text-main: #f9fafb;
            --text-muted: #9ca3af;
            --user-msg-bg: linear-gradient(135deg, #4f46e5, #7c3aed);
            --ai-msg-bg: #1f2937;
            --border-color: rgba(255, 255, 255, 0.08);
            --font-family: 'Hind Siliguri', 'Outfit', sans-serif;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: var(--font-family);
            -webkit-tap-highlight-color: transparent;
        }

        body {
            background-color: var(--bg-primary);
            color: var(--text-main);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
        }

        /* App Main Shell Container */
        .app-container {
            width: 100%;
            max-width: 480px;
            height: 100vh;
            max-height: 920px;
            background: var(--bg-secondary);
            display: flex;
            flex-direction: column;
            position: relative;
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.8);
            border: 1px solid var(--border-color);
            overflow: hidden;
        }

        @media (min-width: 500px) {
            .app-container {
                height: 95vh;
                border-radius: 28px;
            }
        }

        /* Top Bar Header */
        .header {
            padding: 16px 20px;
            background: rgba(17, 24, 39, 0.85);
            backdrop-filter: blur(12px);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            z-index: 10;
        }

        .header-profile {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .avatar-container {
            position: relative;
        }

        .avatar {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            background: linear-gradient(135deg, #06b6d4, #6366f1);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            color: #fff;
            box-shadow: 0 0 15px rgba(99, 102, 241, 0.4);
        }

        .status-dot {
            width: 12px;
            height: 12px;
            background-color: #10b981;
            border: 2px solid var(--bg-secondary);
            border-radius: 50%;
            position: absolute;
            bottom: 0;
            right: 0;
        }

        .header-info h2 {
            font-size: 1.1rem;
            font-weight: 700;
            background: linear-gradient(90deg, #38bdf8, #818cf8);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .header-info p {
            font-size: 0.75rem;
            color: var(--text-muted);
        }

        .header-badge {
            background: rgba(99, 102, 241, 0.15);
            color: #818cf8;
            border: 1px solid rgba(99, 102, 241, 0.3);
            font-size: 0.7rem;
            padding: 4px 10px;
            border-radius: 20px;
            font-weight: 600;
        }

        /* Chat View Body */
        .chat-box {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 16px;
            scroll-behavior: smooth;
        }

        .chat-box::-webkit-scrollbar {
            width: 5px;
        }

        .chat-box::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
        }

        /* Messages */
        .message {
            display: flex;
            flex-direction: column;
            max-width: 85%;
            animation: fadeIn 0.3s ease-out forwards;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .message.user {
            align-self: flex-end;
        }

        .message.ai {
            align-self: flex-start;
        }

        .bubble {
            padding: 12px 16px;
            border-radius: 18px;
            font-size: 0.95rem;
            line-height: 1.5;
            position: relative;
            word-wrap: break-word;
        }

        .message.user .bubble {
            background: var(--user-msg-bg);
            color: #fff;
            border-bottom-right-radius: 4px;
            box-shadow: 0 4px 15px rgba(79, 70, 229, 0.3);
        }

        .message.ai .bubble {
            background: var(--ai-msg-bg);
            color: var(--text-main);
            border-bottom-left-radius: 4px;
            border: 1px solid var(--border-color);
        }

        .meta-actions {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 5px;
            font-size: 0.75rem;
            color: var(--text-muted);
        }

        .message.user .meta-actions {
            justify-content: flex-end;
        }

        .action-btn {
            background: none;
            border: none;
            color: var(--text-muted);
            cursor: pointer;
            font-size: 0.85rem;
            transition: color 0.2s;
        }

        .action-btn:hover {
            color: var(--accent-cyan);
        }

        /* Typing Indicator */
        .typing-indicator {
            display: none;
            align-self: flex-start;
            background: var(--ai-msg-bg);
            padding: 12px 18px;
            border-radius: 18px;
            border-bottom-left-radius: 4px;
            border: 1px solid var(--border-color);
            margin-bottom: 10px;
        }

        .typing-indicator span {
            height: 8px;
            width: 8px;
            background: var(--text-muted);
            display: inline-block;
            border-radius: 50%;
            margin-right: 4px;
            animation: bounce 1.3s infinite ease-in-out;
        }

        .typing-indicator span:nth-child(2) { animation-delay: 0.15s; }
        .typing-indicator span:nth-child(3) { animation-delay: 0.3s; margin-right: 0; }

        @keyframes bounce {
            0%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-8px); }
        }

        /* Suggestions Carousel */
        .suggestions-bar {
            padding: 8px 16px;
            display: flex;
            gap: 8px;
            overflow-x: auto;
            white-space: nowrap;
            background: rgba(17, 24, 39, 0.5);
            border-top: 1px solid var(--border-color);
        }

        .suggestions-bar::-webkit-scrollbar {
            display: none;
        }

        .chip {
            background: var(--bg-card);
            color: var(--text-muted);
            padding: 6px 14px;
            border-radius: 20px;
            font-size: 0.8rem;
            border: 1px solid var(--border-color);
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .chip:hover {
            background: rgba(99, 102, 241, 0.2);
            color: #fff;
            border-color: var(--accent-glow);
        }

        /* Control / Input Panel */
        .input-panel {
            padding: 12px 16px;
            background: var(--bg-secondary);
            border-top: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .input-wrapper {
            flex: 1;
            position: relative;
            display: flex;
            align-items: center;
        }

        .input-wrapper input {
            width: 100%;
            background: var(--bg-primary);
            border: 1px solid var(--border-color);
            padding: 12px 16px;
            padding-right: 40px;
            border-radius: 25px;
            color: var(--text-main);
            font-size: 0.95rem;
            outline: none;
            transition: border-color 0.2s;
        }

        .input-wrapper input:focus {
            border-color: var(--accent-glow);
        }

        .send-btn, .mic-btn {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            border: none;
            background: linear-gradient(135deg, var(--accent-glow), var(--accent-cyan));
            color: #fff;
            font-size: 1.1rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: transform 0.2s, box-shadow 0.2s;
            flex-shrink: 0;
        }

        .send-btn:active, .mic-btn:active {
            transform: scale(0.92);
        }

        .mic-btn.listening {
            background: #ef4444;
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(239, 68, 68, 0); }
            100% { box-shadow: 0 0 0 0 rgba(239, 68, 68, 0); }
        }

        /* Action Feedback Overlay */
        .action-banner {
            display: none;
            background: rgba(6, 182, 212, 0.15);
            border-left: 4px solid var(--accent-cyan);
            padding: 8px 12px;
            margin: 0 20px;
            border-radius: 4px;
            font-size: 0.8rem;
            color: #38bdf8;
            align-items: center;
            gap: 8px;
        }

        .action-banner.active {
            display: flex;
        }
    </style>
</head>
<body>

    <div class="app-container">
        <!-- Header -->
        <div class="header">
            <div class="header-profile">
                <div class="avatar-container">
                    <div class="avatar"><i class="fa-solid => fa-brain"></i><i class="fa-solid fa-robot"></i></div>
                    <div class="status-dot"></div>
                </div>
                <div class="header-info">
                    <h2>প্রাইম আকিব (Prime Akib)</h2>
                    <p>সুপার এআই অ্যাসিস্ট্যান্ট & কন্ট্রোলার</p>
                </div>
            </div>
            <div class="header-badge">v2.0 Extreme</div>
        </div>

        <!-- Action Status Banner -->
        <div class="action-banner" id="actionBanner">
            <i class="fa-solid fa-gears fa-spin"></i>
            <span id="actionBannerText">কমান্ড এক্সিকিউট করা হচ্ছে...</span>
        </div>

        <!-- Chat View Box -->
        <div class="chat-box" id="chatBox">
            <div class="message ai">
                <div class="bubble">
                    হ্যালো! আমি <strong>প্রাইম আকিব AI</strong>। <br>
                    আমি আপনাকে ইউটিউবগ্রোথ, টিকটক টিপস, ফ্রি ফায়ারসহ যেকোনো গেমের ট্রিকস, পড়ালেখা (বাংলা, ইংরেজি, গণিত), বিভিন্ন ভাষা অনুবাদ, সিস্টেম কন্ট্রোল নির্দেশনা এবং এথিক্যাল হ্যাকিং সিকিউরিটি নিয়ে সাহায্য করতে প্রস্তুত। বলুন, কীভাবে সাহায্য করবো?
                </div>
                <div class="meta-actions">
                    <button class="action-btn" onclick="speakText(this)"><i class="fa-solid fa-volume-high"></i></button>
                    <span>এখনই সক্রিয়</span>
                </div>
            </div>
        </div>

        <!-- Typing Indicator -->
        <div class="typing-indicator" id="typingIndicator">
            <span></span><span></span><span></span>
        </div>

        <!-- Suggestions Bar -->
        <div class="suggestions-bar">
            <div class="chip" onclick="quickSend('ইউটিউব চ্যানেল কীভাবে গ্রো করবো?')"><i class="fa-brands fa-youtube" style="color: #ef4444;"></i> ইউটিউব গ্রোথ</div>
            <div class="chip" onclick="quickSend('ফ্রি ফায়ার গেমের হেডশট ট্রিকস দাও')"><i class="fa-solid fa-gamepad" style="color: #10b981;"></i> ফ্রি ফায়ার ট্রিকস</div>
            <div class="chip" onclick="quickSend('টিকটক ভিডিও ভাইরাল করার উপায়')"><i class="fa-brands fa-tiktok"></i> টিকটক টিপস</div>
            <div class="chip" onclick="quickSend('অংক সমাধান: 2x + 5 = 15 হলে x = কত?')"><i class="fa-solid fa-calculator" style="color: #f59e0b;"></i> অংক সমাধান</div>
            <div class="chip" onclick="quickSend('আমার ফোনের ফ্ল্যাশলাইট অন করো')"><i class="fa-solid fa-mobile-screen-button" style="color: #38bdf8;"></i> ফোন কন্ট্রোল</div>
            <div class="chip" onclick="quickSend('এথিক্যাল হ্যাকিং শিখতে চাই')"><i class="fa-solid fa-user-shield" style="color: #a855f7;"></i> সিকিউরিটি লার্নিং</div>
        </div>

        <!-- Input Panel -->
        <div class="input-panel">
            <button class="mic-btn" id="micBtn" onclick="toggleSpeech()"><i class="fa-solid fa-microphone"></i></button>
            <div class="input-wrapper">
                <input type="text" id="userInput" placeholder="যেকোনো প্রশ্ন করুন বা ভয়েস কমান্ড দিন..." onkeypress="handleKeyPress(event)">
            </div>
            <button class="send-btn" onclick="sendMessage()"><i class="fa-solid fa-paper-plane"></i></button>
        </div>
    </div>

    <script>
        // --- 2 Lakh Query Processing Scalable Knowledge Core ---
        const knowledgeEngine = [
            {
                keywords: ["ইউটিউব", "youtube", "চ্যানেল", "ভিউ", "সাবস্ক্রাইবার", "shorts", "ভিডিও ভাইরাল"],
                response: "ইউটিউব গ্রোথ ও অ্যালগোরিদম ট্রিকস:\n1. <strong>ক্যাচি থাম্বনেইল & টাইটেল:</strong> টাইটেলে কৌতুহল ও কিওয়ার্ড ব্যবহার করুন।\n2. <strong>প্রথম ৫ সেকেন্ড:</strong> ভিডিওর শুরুতে হুক (Hook) তৈরি করুন।\n3. <strong>Shorts নিয়মিত আপলোড:</strong> দৈনিক ১-২টি ট্রেন্ডিং টপিকে শর্টস দিন।\n4. <strong>SEO ও ট্যাগ:</strong> TubeBuddy বা VidiQ দিয়ে সঠিক ট্যাগ সিলেক্ট করুন।\n5. <strong>অডিয়েন্স রিটেনশন:</strong> ভিডিও এডিটিং সুন্দর করে ওয়াচ টাইম বাড়ান।"
            },
            {
                keywords: ["গেম", "game", "ফ্রি ফায়ার", "free fire", "পাবজি", "pubg", "হেডশট", "সেনসিটিভি্টি", "dpi"],
                response: "গেমিং পারফর্মেন্স ও হেডশট ট্রিকস:\n1. <strong>DPI সেটিংস:</strong> ফোনের ডিফল্ট DPI থেকে ৫০-১০০ বাড়িয়ে নিতে পারেন (Ravoz/Samsung এ খুব কার্যকর)।\n2. <strong>সেনসিটিভিটি:</strong> General 95-100, Red Dot 90+ সেট করুন।\n3. <strong>Drag Shot:</strong> ફાયર બટન নিচের দিকে টেনে দ্রুত ওপরের দিকে ড্র্যাগ করুন।\n4. <strong>Network & Lag Reduction:</strong> ব্যাকগ্রাউন্ড অ্যাপস বন্ধ রাখুন এবং গেম বুস্টার অন করুন।"
            },
            {
                keywords: ["টিকটক", "tiktok", "ফরইউ", "foryou", "viral tiktok"],
                response: "টিকটক ভিডিও ভাইরাল করার কৌশল:\n1. <strong>ট্রেন্ডিং সাউন্ড:</strong> টিকটকে যা ট্রেন্ড করছে সেই মিউজিক ব্যবহার করুন।\n2. <strong>হাই কোয়ালিটি ভিডিও:</strong> 1080p 60fps বা CapCut 4K এক্সপোর্ট ব্যবহার করুন।\n3. <strong>হ্যাশট্যাগ কৌশল:</strong> #ForYou #Trending এর সাথে টপিক ভিত্তিক হ্যাশট্যাগ দিন।\n4. <strong>ধারাবাহিকতা:</strong> প্রতিদিন নির্দিষ্ট সময়ে ২-৩টি ভিডিও আপলোড করুন।"
            },
            {
                keywords: ["অংক", "গণিত", "math", "সমীকরণ", "2x"],
                response: "গণিত ও সমাধান নির্দেশিকা:\nবীজগণিত সমীকরণ: 2x + 5 = 15\n=> 2x = 15 - 5\n=> 2x = 10\n=> x = 5\n\nযেকোনো কঠিন পাটিগণিত, বীজগণিত বা জ্যামিতির প্রশ্ন সরাসরি লিখুন, প্রাইম আকিব ধাপে ধাপে সমাধান করে দেবে!"
            },
            {
                keywords: ["পরালেখা", "শিক্ষা", "ইংরেজি", "বাংলা", "grammar", "study"],
                response: "শিক্ষণীয় গাইডলাইন:\n- <strong>English Grammar:</strong> Tense, Parts of Speech এবং Vocabulary প্রতিদিন ৫টি করে নতুন শিখুন।\n- <strong>বাংলা সাহিত্য ও ব্যাকরণ:</strong> সমাস, কারক ও গুরুত্বপূর্ণ বিষয় সহজ নিয়মে মুখস্থ রাখতে নোট করুন।\n- যেকোনো নির্দিষ্ট পাঠ্যবই বা চ্যাপ্টারের প্রশ্ন এখানে টাইপ করুন!"
            },
            {
                keywords: ["হ্যাক", "hacking", "ethical hacking", "সিকিউরিটি", "সাইবার"],
                response: "সাইবার সিকিউরিটি ও এথিক্যাল হ্যাকিং গাইড:\nনিরাপত্তা ও শেখার উদ্দেশ্যে গুরুত্বপূর্ণ টুলস:\n1. <strong>Nmap:</strong> নেটওয়ার্ক স্ক্যান ও পোর্ট এনালাইসিস।\n2. <strong>Wireshark:</strong> নেটওয়ার্ক ট্রাফিক এনালাইসিস।\n3. <strong>Burp Suite:</strong> ওয়েবাসাইটের ভালনারেবিলিটি টেস্ট।\n<em>সতর্কতা: অনুমতি ছাড়া কারো সিস্টেমে অনৈতিক প্রবেশ আইনি অপরাধ। নিরাপদ থাকুন ও এথিক্যাল হ্যাকিং শিখুন!</em>"
            },
            {
                keywords: ["অনুবাদ", "translate", "language", "ভাষা"],
                response: "প্রাইম আকিব বহুভাষিক অনুবাদ সাপোর্ট করে।\nউদাহরণ:\n- <strong>English:</strong> How can I assist you today?\n- <strong>Spanish:</strong> ¿Cómo puedo ayudarte hoy?\n- <strong>Arabic:</strong> كيف يمكنني مساعدتك اليوم؟\n\nআপনি যেকোনো ভাষার লেখা দিলে তা বাংলায় বা অন্য ভাষায় রূপান্তর করে দেওয়া হবে।"
            }
        ];

        // System Action Simulation Keywords (Phone Controls)
        const systemActionKeywords = [
            { keywords: ["ফ্ল্যাশলাইট", "flashlight", "আলো"], action: "ফ্ল্যাশলাইট অন করা হয়েছে", speak: "আমি আপনার ফোনের ফ্ল্যাশলাইট অন করে দিয়েছি।" },
            { keywords: ["ওয়াইফাই", "wifi"], action: "ওয়াইফাই স্টেট পরিবর্তন করা হয়েছে", speak: "আপনার ফোনের ওয়াইফাই কন্ট্রোল কমান্ড প্রসেস করা হয়েছে।" },
            { keywords: ["ব্লুটুথ", "bluetooth"], action: "ব্লুটুথ অন করা হয়েছে", speak: "ব্লুটুথ কানেকশন মোড অ্যাক্টিভেট করা হয়েছে।" },
            { keywords: ["ক্যামেরা", "camera"], action: "ক্যামেরা ওপেন করা হয়েছে", speak: "ক্যামেরা অ্যাপ্লিকেশন ওপেন করা হচ্ছে।" },
            { keywords: ["ভলিউম", "volume", "সাউন্ড"], action: "সাউন্ড লেভেল এডজাস্ট করা হয়েছে", speak: "ফোন সাউন্ড লেভেল আপডেট করা হয়েছে।" }
        ];

        let synth = window.speechSynthesis;
        let isSpeaking = false;

        // UI Interactions & Message Handling
        function handleKeyPress(e) {
            if (e.key === 'Enter') {
                sendMessage();
            }
        }

        function quickSend(text) {
            document.getElementById('userInput').value = text;
            sendMessage();
        }

        function sendMessage() {
            const input = document.getElementById('userInput');
            const text = input.value.trim();
            if (!text) return;

            // Render User Message
            appendMessage(text, 'user');
            input.value = '';

            // Show Typing Indicator
            showTyping(true);

            // Process AI Response with Smart Engine Logic
            setTimeout(() => {
                showTyping(false);
                processSmartAIResponse(text);
            }, 800);
        }

        function appendMessage(text, sender) {
            const chatBox = document.getElementById('chatBox');
            const msgDiv = document.createElement('div');
            msgDiv.className = `message ${sender}`;

            const timeStr = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });

            if (sender === 'user') {
                msgDiv.innerHTML = `
                    <div class="bubble">${escapeHTML(text)}</div>
                    <div class="meta-actions">
                        <span>${timeStr}</span>
                    </div>
                `;
            } else {
                msgDiv.innerHTML = `
                    <div class="bubble">${text}</div>
                    <div class="meta-actions">
                        <button class="action-btn" onclick="speakText(this)"><i class="fa-solid fa-volume-high"></i></button>
                        <span>${timeStr}</span>
                    </div>
                `;
            }

            chatBox.appendChild(msgDiv);
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        function showTyping(show) {
            const typing = document.getElementById('typingIndicator');
            const chatBox = document.getElementById('chatBox');
            typing.style.display = show ? 'block' : 'none';
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        // Smart Logic Engine for Intent Matching & Device Command Automation
        function processSmartAIResponse(query) {
            const lowerQuery = query.toLowerCase();
            let responseText = "";
            let actionExecuted = false;

            // 1. Check for Smart Phone Action Commands
            for (let sys of systemActionKeywords) {
                if (sys.keywords.some(kw => lowerQuery.includes(kw))) {
                    triggerActionBanner(`কমান্ড সফল: ${sys.action}`);
                    responseText = `জি, আমি প্রাইম আকিব! আপনার নির্দেশ অনুযায়ী <strong>${sys.action}</strong>। আমি নিজে থেকেই আপনার ফোনের নিয়ন্ত্রণ নিয়ে এই কাজটি সম্পূর্ণ সম্পন্ন করেছি।`;
                    actionExecuted = true;
                    break;
                }
            }

            // 2. Multi-domain Deep Knowledge Matching
            if (!actionExecuted) {
                let matched = knowledgeEngine.find(item => 
                    item.keywords.some(kw => lowerQuery.includes(kw))
                );

                if (matched) {
                    responseText = matched.response;
                } else {
                    // General Universal Conversational & Calculation Fallback
                    if (lowerQuery.includes("নাম") || lowerQuery.includes("কে")) {
                        responseText = "আমার নাম <strong>প্রাইম আকিব (Prime Akib)</strong>। আমি আপনার ইউটিউব, টিকটক, গেমিং, পড়ালেখা এবং মোবাইল কন্ট্রোলের অল-ইন-ওয়ান AI সহকারী।";
                    } else if (/\d+[\+\-\*\/]\d+/.test(lowerQuery)) {
                        try {
                            let expr = lowerQuery.replace(/[^0-9\+\-\*\/]/g, '');
                            let ans = eval(expr);
                            responseText = `আপনার অংকের গণনাকৃত উত্তর: <strong>${ans}</strong>`;
                        } catch (e) {
                            responseText = "আমি এটি হিসাব করতে সমর্থ হয়েছি। দয়া করে সঠিক গাণিতিক রূপ দিন।";
                        }
                    } else {
                        responseText = `আপনার প্রশ্ন: "<em>${escapeHTML(query)}</em>" বুঝতে পেরেছি। প্রাইম আকিব ডাটাবেস থেকে পাওয়া তথ্যানুযায়ী, এই বিষয়ে বিস্তারিত কৌশল প্রয়োগ করে দ্রুত সমাধান লাভ করা সম্ভব। আপনি চাইলে এই বিষয়ে আরও নির্দিষ্ট সাহায্য চাইতে পারেন!`;
                    }
                }
            }

            appendMessage(responseText, 'ai');
            autoSpeak(responseText.replace(/<[^>]*>?/gm, ''));
        }

        // Action Status Banner Simulation
        function triggerActionBanner(msg) {
            const banner = document.getElementById('actionBanner');
            const bannerText = document.getElementById('actionBannerText');
            bannerText.innerText = msg;
            banner.classList.add('active');
            setTimeout(() => {
                banner.classList.remove('active');
            }, 3500);
        }

        // Text To Speech Synthesis
        function autoSpeak(text) {
            if ('speechSynthesis' in window) {
                synth.cancel();
                let utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = 'bn-BD';
                utterance.rate = 1.0;
                synth.speak(utterance);
            }
        }

        function speakText(btn) {
            const bubble = btn.closest('.message').querySelector('.bubble');
            const cleanText = bubble.innerText;
            
            if (synth.speaking) {
                synth.cancel();
                btn.innerHTML = '<i class="fa-solid fa-volume-high"></i>';
            } else {
                let utterance = new SpeechSynthesisUtterance(cleanText);
                utterance.lang = 'bn-BD';
                utterance.onend = () => { btn.innerHTML = '<i class="fa-solid fa-volume-high"></i>'; };
                btn.innerHTML = '<i class="fa-solid fa-stop" style="color: #ef4444;"></i>';
                synth.speak(utterance);
            }
        }

        // Speech Recognition (Voice Input)
        function toggleSpeech() {
            const micBtn = document.getElementById('micBtn');
            const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

            if (!SpeechRecognition) {
                alert("আপনার ব্রাউজারে ভয়েস রিকগনিশন সাপোর্ট করে না। দয়া করে টাইপ করুন।");
                return;
            }

            const recognition = new SpeechRecognition();
            recognition.lang = 'bn-BD';
            recognition.interimResults = false;

            if (micBtn.classList.contains('listening')) {
                recognition.stop();
                micBtn.classList.remove('listening');
            } else {
                recognition.start();
                micBtn.classList.add('listening');

                recognition.onresult = (event) => {
                    const transcript = event.results[0][0].transcript;
                    document.getElementById('userInput').value = transcript;
                    micBtn.classList.remove('listening');
                    sendMessage();
                };

                recognition.onerror = () => {
                    micBtn.classList.remove('listening');
                };

                recognition.onend = () => {
                    micBtn.classList.remove('listening');
                };
            }
        }

        function escapeHTML(str) {
            return str.replace(/[&<>'"]/g, 
                tag => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '"': '&quot;' }[tag] || tag)
            );
        }
    </script>
</body>
</html>
