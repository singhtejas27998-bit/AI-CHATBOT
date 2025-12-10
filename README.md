from flask import Flask, render_template_string, request, jsonify
import datetime

app = Flask(__name__)

# ---------- Simple intent-based response logic ----------

def get_bot_response(user_message: str) -> str:
    msg = user_message.lower().strip()

    if msg in ("hi", "hello", "hey", "hii", "hallo"):
        return "Hello! 👋 I’m your AI assistant. How can I help you today?"

    if "time" in msg:
        now = datetime.datetime.now().strftime("%H:%M:%S")
        return f"Right now the time is {now}."

    if "date" in msg or "day" in msg:
        today = datetime.datetime.now().strftime("%d-%m-%Y (%A)")
        return f"Today is {today}."

    if "your name" in msg:
        return "I’m a simple AI chatbot assistant written in Python 🐍."

    if "help" in msg:
        return (
            "I can tell you the time, date, reply to greetings, "
            "and answer simple questions. Try asking: 'what's the time?'"
        )

    # Default fallback
    return (
        "I'm not sure about that yet 🤔. "
        "Try asking me about the time, date, or just say 'hello'."
    )


# ---------- Simple HTML UI using template_string ----------

CHAT_HTML = """
<!doctype html>
<html>
<head>
    <title>AI Chatbot Assistant</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f4f4;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .chat-container {
            width: 400px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }
        .chat-header {
            background: #4a6cf7;
            color: white;
            padding: 10px 15px;
            font-weight: bold;
        }
        .chat-messages {
            flex: 1;
            padding: 10px;
            overflow-y: auto;
            font-size: 14px;
        }
        .message {
            margin-bottom: 8px;
            max-width: 80%;
            padding: 6px 10px;
            border-radius: 12px;
            clear: both;
        }
        .user {
            background: #e1f5fe;
            margin-left: auto;
        }
        .bot {
            background: #eeeeee;
            margin-right: auto;
        }
        .chat-input-area {
            display: flex;
            border-top: 1px solid #ddd;
        }
        .chat-input-area input {
            flex: 1;
            border: none;
            padding: 10px;
            font-size: 14px;
            outline: none;
        }
        .chat-input-area button {
            border: none;
            padding: 0 15px;
            cursor: pointer;
            background: #4a6cf7;
            color: white;
            font-weight: bold;
        }
    </style>
</head>
<body>
<div class="chat-container">
    <div class="chat-header">AI Chatbot Assistant</div>
    <div id="messages" class="chat-messages">
        <div class="message bot">
            Hello! 👋 I’m your AI assistant. Ask me something!
        </div>
    </div>
    <div class="chat-input-area">
        <input id="userInput" type="text" placeholder="Type your message..." onkeydown="if(event.key==='Enter'){sendMessage();}">
        <button onclick="sendMessage()">Send</button>
    </div>
</div>

<script>
    async function sendMessage() {
        const input = document.getElementById('userInput');
        const text = input.value.trim();
        if (!text) return;

        addMessage(text, 'user');
        input.value = '';

        const res = await fetch('/chat', {
            method: 'POST',
            headers: {'Content-Type': 'application/json'},
            body: JSON.stringify({message: text})
        });

        const data = await res.json();
        addMessage(data.reply, 'bot');
    }

    function addMessage(text, sender) {
        const messages = document.getElementById('messages');
        const div = document.createElement('div');
        div.className = 'message ' + sender;
        div.innerText = text;
        messages.appendChild(div);
        messages.scrollTop = messages.scrollHeight;
    }
</script>
</body>
</html>
"""

@app.route("/")
def index():
    return render_template_string(CHAT_HTML)

@app.route("/chat", methods=["POST"])
def chat():
    data = request.get_json()
    user_message = data.get("message", "")
    bot_reply = get_bot_response(user_message)
    return jsonify({"reply": bot_reply})

if __name__ == "__main__":
    app.run(debug=True)
    
