- 👋 Hi, I’m @kingPanther927
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
kingPanther927/kingPanther927 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import openai
import speech_recognition as sr
import pyttsx3

# Initialize OpenAI with your API key
openai.api_key = 'your_openai_api_key'

# Initialize the speech engine
engine = pyttsx3.init()

def speak(text):
    engine.say(text)
    engine.runAndWait()

def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio = recognizer.listen(source)
        try:
            query = recognizer.recognize_google(audio, language='en-US')
            print(f"You said: {query}")
            return query
        except sr.UnknownValueError:
            print("Sorry, I did not get that.")
            return ""
        except sr.RequestError:
            print("Could not request results; check your network connection.")
            return ""

def chat_with_gpt(prompt):
    response = openai.Completion.create(
        engine="text-davinci-003",  # Use a suitable engine
        prompt=prompt,
        max_tokens=100,
        n=1,
        stop=None,
        temperature=0.7,
    )
    return response.choices[0].text.strip()

def main():
    speak("Hello, I am Jarvis. How can I assist you today?")
    
    while True:
        query = listen()
        if query:
            if "exit" in query.lower() or "stop" in query.lower():
                speak("Goodbye!")
                break
            
            response = chat_with_gpt(query)
            print("Jarvis:", response)
            speak(response)

if __name__ == "__main__":
    main()
