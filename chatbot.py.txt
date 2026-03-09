# RULE-BASED CHATBOT

import datetime

# ---------- Text Processing ----------
def preprocess(text):
    """
    Normalize user input:
    - lowercase
    - remove extra spaces
    """
    return text.lower().strip()


# ---------- Intent Detection ----------
def detect_intent(user_input):
    
    greetings = ["hello", "hi", "hey", "good morning", "good evening"]
    exit_words = ["bye", "exit", "quit", "goodbye"]
    help_words = ["help", "what can you do", "options"]

    # Greeting Intent
    for word in greetings:
        if word in user_input:
            return "greeting"

    # Exit Intent
    for word in exit_words:
        if word in user_input:
            return "exit"

    # Help Intent
    for word in help_words:
        if word in user_input:
            return "help"

    # Time Intent
    if "time" in user_input:
        return "time"

    # Date Intent
    if "date" in user_input:
        return "date"

    # Name Intent
    if "my name is" in user_input:
        return "store_name"

    if "your name" in user_input:
        return "bot_name"

    return "unknown"


# ---------- Response Generator ----------
def generate_response(intent, user_input, memory):

    if intent == "greeting":
        return "Hello! 👋 How can I assist you today?"

    elif intent == "help":
        return (
            "I can:\n"
            "- Greet you\n"
            "- Tell date/time\n"
            "- Remember your name\n"
            "- Chat using simple rules\n"
            "Type 'bye' to exit."
        )

    elif intent == "time":
        current_time = datetime.datetime.now().strftime("%H:%M:%S")
        return f"The current time is {current_time}"

    elif intent == "date":
        today = datetime.date.today()
        return f"Today's date is {today}"

    elif intent == "store_name":
        name = user_input.replace("my name is", "").strip().title()
        memory["user_name"] = name
        return f"Nice to meet you, {name}!"

    elif intent == "bot_name":
        return "I am a Rule-Based AI Chatbot."

    elif intent == "exit":
        return "Goodbye! Have a great day! 👋"

    else:
        return "I'm not sure how to respond to that. Try asking for help."


# ---------- Chat Loop ----------
def chatbot():
    print("🤖 Chatbot Started (type 'bye' to exit)")
    
    memory = {}

    while True:
        user_input = input("You: ")

        cleaned_input = preprocess(user_input)

        intent = detect_intent(cleaned_input)

        response = generate_response(intent, cleaned_input, memory)

        print("Bot:", response)

        if intent == "exit":
            break


# ---------- Run Chatbot ----------
if __name__ == "__main__":
    chatbot()