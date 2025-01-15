project/
│
├── static/               # Statik fayllar (CSS, JavaScript)
│   ├── styles.css        # Sayt dizayni
│   └── script.js         # JavaScript kodlari
│
├── templates/            # HTML shablonlari
│   └── index.html        # Asosiy sahifa
│
├── app.py                # Flask backend kodlari
└── requirements.txt      # Kutubxona talablari
<!DOCTYPE html>
<html lang="uz">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bot Bilan Suhbat</title>
    <link rel="stylesheet" href="/static/styles.css">
</head>
<body>
    <h1>Sun'iy Intellekt Bilan Suhbat</h1>
    <div class="chat-box" id="chatBox"></div>
    <input type="text" id="userInput" placeholder="Savolingizni kiriting...">
    <button onclick="sendMessage()">Yuborish</button>
    <script src="/static/script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 20px;
    text-align: center;
}

.chat-box {
    width: 50%;
    margin: 20px auto;
    padding: 10px;
    border: 1px solid #ccc;
    background-color: #f9f9f9;
    text-align: left;
    max-height: 300px;
    overflow-y: scroll;
}

.chat-box .message {
    margin: 5px 0;
}

input[type="text"] {
    width: 70%;
    padding: 10px;
    margin-top: 10px;
}

button {
    padding: 10px 20px;
    margin-top: 10px;
    cursor: pointer;
}
async function sendMessage() {
    let userInput = document.getElementById("userInput").value;
    let chatBox = document.getElementById("chatBox");

    if (userInput) {
        // Foydalanuvchi xabari
        chatBox.innerHTML += `<div class="message"><b>Siz:</b> ${userInput}</div>`;
        
        // Bot javobi
        try {
            let response = await fetch('/api/chat', {
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({message: userInput})
            });
            let data = await response.json();
            chatBox.innerHTML += `<div class="message"><b>Bot:</b> ${data.response}</div>`;
        } catch (error) {
            chatBox.innerHTML += `<div class="message"><b>Bot:</b> Xato yuz berdi!</div>`;
        }

        document.getElementById("userInput").value = ""; // Inputni tozalash
    }
}
from flask import Flask, request, jsonify, render_template

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/api/chat', methods=['POST'])
def chat():
    user_message = request.json.get('message')
    # Botning oddiy javobi
    ai_response = f"Siz so'radingiz: {user_message}. Men hali o'rganmoqdaman!"
    return jsonify({'response': ai_response})

if __name__ == '__main__':
    app.run(debug=True)
    Flask==2.2.3
    pip install -r requirements.txt
    python app.py
    
