<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text to Audio Converter</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { 
            background: linear-gradient(135deg, #ff758c, #ff7eb3);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            text-align: center;
            color: white;
            flex-direction: column;
            padding: 20px;
        }
        .container {
            background: white;
            color: black;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
            max-width: 500px;
            width: 100%;
            transition: 0.3s ease-in-out;
            text-align: center;
        }
        h1 { font-size: 28px; color: #333; font-weight: 600; }
        p { font-size: 14px; color: #666; }
        textarea {
            width: 100%;
            height: 120px;
            padding: 12px;
            font-size: 16px;
            border: 2px solid #ddd;
            border-radius: 8px;
            resize: none;
            margin-bottom: 15px;
            transition: border-color 0.3s ease;
        }
        textarea:focus { border-color: #ff758c; outline: none; }
        select, button {
            width: 100%;
            padding: 15px;
            font-size: 18px;
            margin-top: 10px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            transition: 0.3s ease-in-out;
        }
        select { border: 2px solid #ddd; background: white; }
        .convert-btn {
            background: #ff758c;
            color: white;
            font-weight: 700;
            font-size: 20px;
        }
        .convert-btn:hover { background: #ff5c7a; }
        .download-btn {
            background: #4CAF50;
            color: white;
            padding: 12px;
            text-decoration: none;
            border-radius: 5px;
            font-weight: 600;
            transition: 0.3s ease-in-out;
            display: block; 
            margin-top: 10px;
        }
        .download-btn:hover { background: #45a049; }
        .timeline {
            width: 100%;
            height: 8px;
            background: #ddd;
            border-radius: 5px;
            margin-top: 15px;
            position: relative;
        }
        .timeline-progress {
            width: 0%;
            height: 100%;
            background: #ff758c;
            border-radius: 5px;
            transition: width 0.1s linear;
        }
        .footer { margin-top: 10px; font-size: 14px; color: #555; }
    </style>
</head>
<body>

    <div class="container">
        <h1>🔊 Text to Audio Converter</h1>
        <p>Created by Vivek Ninave</p>
        <textarea id="text-input" placeholder="Enter your text here (Max: 500 words)..."></textarea>
        <select id="language-select">
            <option value="hi-IN">🇮🇳 Hindi</option>
            <option value="en-US">🇺🇸 English</option>
        </select>
        <button class="convert-btn" onclick="convertToAudio()">🔍 Convert to Audio</button>
        
        <div class="timeline" id="timeline">
            <div class="timeline-progress" id="timeline-progress"></div>
        </div>
        
        <audio id="audio-player" controls></audio>
        <a id="download-link" class="download-btn">⬇ Download MP3</a>
        <p class="footer">© 2024 Vivek Ninave | All Rights Reserved</p>
    </div>

    <script>
        function convertToAudio() {
            const text = document.getElementById('text-input').value;
            const language = document.getElementById('language-select').value;
            const audioPlayer = document.getElementById('audio-player');
            const downloadLink = document.getElementById('download-link');
            const timeline = document.getElementById('timeline');
            const timelineProgress = document.getElementById('timeline-progress');

            // Word Limit Check (500 Words)
            const wordCount = text.trim().split(/\s+/).length;
            if (wordCount > 500) {
                alert("⚠ Maximum 500 words allowed!");
                return;
            }

            if (text.trim() === "") {
                alert("कृपया कुछ टेक्स्ट डालें!");
                return;
            }

            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = language;

            speechSynthesis.speak(utterance);

            // Set Audio Player Source
            const audioUrl = URL.createObjectURL(new Blob([text], { type: "audio/mp3" }));
            audioPlayer.src = audioUrl;
            audioPlayer.style.display = "block";
            downloadLink.href = audioUrl;
            downloadLink.setAttribute("download", "speech.mp3");

            // Enable Timeline & Sync with Audio
            updateTimeline(audioPlayer);
        }

        function updateTimeline(audio) {
            const timelineProgress = document.getElementById('timeline-progress');
            audio.addEventListener('timeupdate', () => {
                const progress = (audio.currentTime / audio.duration) * 100;
                timelineProgress.style.width = progress + "%";
            });
        }
    </script>

</body>
</html>
