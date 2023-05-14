# chat-bot
import speech_recognition as sr
import pyttsx3
import pywhatkit
import datetime
import wikipedia
import pyjokes
import tkinter as tk

listener = sr.Recognizer()
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)

# Define the GUI window
window = tk.Tk()
window.title("Friday")
window.geometry("400x400")

# Define the labels and buttons
title_label = tk.Label(window, text="Welcome to Friday!", font=("Arial", 18))
title_label.pack(pady=10)

command_label = tk.Label(window, text="Command:", font=("Arial", 14))
command_label.pack()

command_entry = tk.Entry(window, width=30, font=("Arial", 14))
command_entry.pack(pady=10)

output_label = tk.Label(window, text="", font=("Arial", 14))
output_label.pack(pady=10)

def talk(text):
    engine.say(text)
    engine.runAndWait()

def take_command():
    try:
        with sr.Microphone() as source:
            output_label.configure(text="listening...")
            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            if 'friday' in command:
                command = command.replace('friday', '')
            print(command)
    except:
        pass
    return command

def run_friday():
    command = command_entry.get()
    if 'play' in command:
        song = command.replace('play', '')
        output_label.configure(text='playing ' + song)
        pywhatkit.playonyt(song)
    elif 'what is the time' in command:
        time = datetime.datetime.now().strftime('%I:%M %p')
        output_label.configure(text='Current time is ' + time)
    elif 'who is' in command:
        person = command.replace('who is', '')
        info = wikipedia.summary(person, 1)
        output_label.configure(text=info)
        talk(info)
    elif 'date' in command:
        output_label.configure(text='sorry, I have a headache')
    elif 'are you single' in command:
        output_label.configure(text='I am in a relationship with wifi')
    elif 'joke' in command:
        talk(pyjokes.get_joke())
    else:
        output_label.configure(text='Please say the command again.')

def activate_voice():
    command_entry.delete(0, tk.END)
    command = take_command()
    command_entry.insert(0, command)
    run_friday()

# Define the "Run" button
run_button = tk.Button(window, text="Run", font=("Arial", 14), command=run_friday)
run_button.pack(pady=20)

# Define the microphone activation button
mic_button = tk.Button(window, text="Activate voice command", font=("Arial", 14), command=activate_voice)
mic_button.pack(pady=20)

# Start the GUI event loop
window.mainloop()
