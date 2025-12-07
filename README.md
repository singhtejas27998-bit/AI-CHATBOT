import random
import textwrap

# -----------------------------
# Simple AI Chatbot Assistant
# -----------------------------

BOT_NAME = "Assistant"

def wrap(text, width=80):
    return "\n".join(textwrap.wrap(text, width=width))

def get_small_talk_response(user):
    if any(word in user for word in ["hi", "hello", "hey"]):
        return random.choice([
            "Hi! 👋 How can I help you today?",
            "Hello! What can I do for you?",
            "Hey! Need any help?"
        ])

    if "how are you" in user:
        return random.choice([
            "I'm just code, but I'm running fine 😄 How about you?",
            "Doing great and ready to help! How are you?"
        ])

    if "your name" in user:
        return f"My name is {BOT_NAME}. I'm your AI assistant 🤖."

    return None

def get_help_response(user):
    if "help" in user or "what can you do" in user:
        return (
            "I can answer general questions, explain concepts, help with basic programming, "
            "and chat with you. Try asking:\n"
            "- \"Explain AI in simple words\"\n"
            "- \"Help me with a Python if-else example\"\n"
            "- \"Give me tips to study better\""
        )
    return None

def get_programming_response(user):
    if "python" in user and ("example" in user or "code" in user):
        return (
            "Here is a simple Python if-else example:\n\n"
            "x = 10\n"
            "if x > 5:\n"
            "    print(\"x is greater than 5\")\n"
            "else:\n"
            "    print(\"x is 5 or less\")"
        )

    if "reverse a string" in user and "python" in user:
        return (
            "To reverse a string in Python:\n\n"
            "s = \"hello\"\n"
            "reversed_s = s[::-1]\n"
            "print(reversed_s)  # olleh"
        )

    return None

def get_study_response(user):
    if "study" in user or "exam" in user or "learn" in user:
        return (
            "Here are some study tips:\n"
            "1) Set small goals (e.g. 25–30 minutes focused study).\n"
            "2) Remove distractions (mobile, notifications).\n"
            "3) After studying, write a short summary in your own words.\n"
            "4) Practice questions instead of only reading.\n"
            "5) Teach the topic to someone else (or pretend to)."
        )
    return None

def fallback_response(user):
    return (
        "I'm not fully sure about that yet 😅\n"
        "Try asking me to explain a concept, help with some code, "
        "or give tips about studying or productivity."
    )

def get_response(user_input: str) -> str:
    user = user_input.lower().strip()

    # 1. Small talk
    resp = get_small_talk_response(user)
    if resp:
        return resp

    # 2. Help / capabilities
    resp = get_help_response(user)
    if resp:
        return resp

    # 3. Programming-related
    resp = get_programming_response(user)
    if resp:
        return resp

    # 4. Study / exam tips
    resp = get_study_response(user)
    if resp:
        return resp

    # 5. Fallback
    return fallback_response(user)

def main():
    print(f"{BOT_NAME}: Hi! I'm your AI chatbot assistant. Type 'quit' to exit.\n")

    while True:
        user_input = input("You: ")
        if user_input.strip().lower() in ["quit", "exit", "bye"]:
            print(f"{BOT_NAME}: Bye! Have a great day 👋")
            break

        bot_reply = get_response(user_input)
        print(f"{BOT_NAME}: {wrap(bot_reply)}\n")

if __name__ == "__main__":
    main()
# AI-CHATBOT
