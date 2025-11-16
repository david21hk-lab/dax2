<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Media Content Upload & Share</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f2f5;
            margin: 0;
            padding: 20px;
            text-align: center;
        }
        .container {
            max-width: 700px;
            margin: auto;
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
        }
        h1 {
            color: #1a73e8;
            margin-bottom: 10px;
        }
        p {
            color: #555;
        }
        form {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin: 20px 0;
        }
        input[type="file"] {
            margin: 15px 0;
            padding: 10px;
            border: 2px dashed #ccc;
            border-radius: 8px;
            width: 100%;
            max-width: 400px;
        }
        button {
            background-color: #1a73e8;
            color: white;
            padding: 12px 30px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            transition: 0.3s;
        }
        button:hover {
            background-color: #0d47a1;
        }
        #preview {
            margin-top: 25px;
            padding: 15px;
            background: #f9f9f9;
            border-radius: 8px;
            display: none;
        }
        .media {
            max-width: 100%;
            margin: 10px 0;
            border-radius: 8px;
        }
        .share-link {
            background: #e8f5e9;
            padding: 10px;
            border-radius: 6px;
            margin-top: 15px;
            word-break: break-all;
        }
        .copy-btn {
            background: #4caf50;
            margin-left: 10px;
            padding: 6px 12px;
            font-size: 14px;
        }
        .copy-btn:hover {
            background: #388e3c;
        }
        .multiple {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 15px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📤 Media Share Karein</h1>
        <p>Ek ya zyada image, video, audio upload karein aur shareable link paayein!</p>
        
        <form id="uploadForm">
            <input type="file" id="mediaFile" accept="image/*,video/*,audio/*" multiple>
            <button type="submit">🚀 Upload & Share Karein</button>
        </form>

        <div id="preview"></div>
    </div>

    <script>
        const form = document.getElementById('uploadForm');
        const fileInput = document.getElementById('mediaFile');
        const preview = document.getElementById('preview');

        form.addEventListener('submit', function(e) {
            e.preventDefault();
            const files = fileInput.files;
            if (files.length === 0) return;

            preview.style.display = 'block';
            preview.innerHTML = '<h3>✅ Upload Hua!</h3><div class="multiple"></div>';

            const mediaContainer = preview.querySelector('.multiple');
            const shareLinks = [];

            Array.from(files).forEach((file, index) => {
                const reader = new FileReader();
                reader.onload = function(event) {
                    const url = event.target.result;
                    let mediaElement;

                    if (file.type.startsWith('image/')) {
                        mediaElement = `<img src="${url}" class="media" alt="${file.name}">`;
                    } else if (file.type.startsWith('video/')) {
                        mediaElement = `<video src="${url}" class="media" controls></video>`;
                    } else if (file.type.startsWith('audio/')) {
                        mediaElement = `<audio src="${url}" class="media" controls></audio>`;
                    }

                    const div = document.createElement('div');
                    div.innerHTML = `
                        ${mediaElement}
                        <p><strong>${file.name}</strong> (${(file.size/1024/1024).toFixed(2)} MB)</p>
                        <p><a href="${url}" download="${file.name}">⬇️ Download</a></p>
                    `;
                    mediaContainer.appendChild(div);

                    // Generate shareable link (simulated)
                    const fakeLink = `https://share.mediafake.in/file-${Date.now()}-${index}`;
                    shareLinks.push(`<div class="share-link">
                        <strong>${file.name}:</strong> 
                        <span>${fakeLink}</span>
                        <button class="copy-btn" onclick="copyToClipboard('${fakeLink}')">📋 Copy</button>
                    </div>`);
                    
                    if (shareLinks.length === files.length) {
                        preview.innerHTML += '<h3>🔗 Share Links:</h3>' + shareLinks.join('');
                    }
                };
                reader.readAsDataURL(file);
            });
        });

        function copyToClipboard(text) {
            navigator.clipboard.writeText(text).then(() => {
                alert('Link copy ho gaya! 📋');
            });
        }
    </script>
</body>
</html>
