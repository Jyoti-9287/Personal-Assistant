# Personal-Assistant
import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser
from webbrowser import Chrome
import os
import random
import wolframalpha
import wikipedia

engine=pyttsx3.init('sapi5')
voices=engine.getProperty('voices')
#print(voices[1].id)
engine.setProperty('voice',voices[1].id)

def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        text="Good Morning!"

    elif hour>=12 and hour<18:
        text="Good AfterNoon!"

    else:
        text="Good Evening!"
    print(text,"\nI am Jarvis. Sir, Please tell me how may I help you",)
    speak(text)
    speak("I am Jarvis. Sir, Please tell me how may I help you")

def takeCommand():
    print("speak something")
    speak("speak something")
    r=sr.Recognizer()
    with sr.Microphone() as source:
        # r.pause_threshold=.6
        # r.energy_threshold=1
        audio=r.listen(source)
        print("It's done. Stop Speaking")
        speak("It's done. Stop Speaking")
    try:
        query=r.recognize_google(audio,language="en-in")
        print(f"User Said: {query}\n")
        speak("Recognized")
    except Exception as e:
        print(e,end='')
        print("Say that again please\n")
        speak("Say that again please")
        return "None"
    return query

if __name__=="__main__":
    wishMe()
    while True:
        query=takeCommand().lower()
        # if 'wikipdeia' in query:
        #     speak("searching.......")
        #     query=query.replace('wikipdeia','')
        #     result=wikipedia.summary(query,sentences=1)
        #     print(result)
        #     speak(result)
        if 'play music' in query:
            music_dir='C:\\Users\\Dell\\Desktop\\Pooja Mixing Point\\DJ Dinesh Pooja\\My Chhoice'
            songs=os.listdir(music_dir)
            print(songs)
            rand=random.randint(0,len(songs)-1)
            os.startfile(os.path.join(music_dir,songs[rand]))
        
        elif 'the time' in query:
            t=datetime.datetime.now()
            print("Sir, the time is", t)
            speak(f"Sir, the time is {t}")
        elif 'open code' in query:
            path='C:\\Users\\Dell\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\Visual Studio Code'
            os.startfile(path)
        elif query=='quit' or query=='exit' or query=='stop' or query=='None':
            print("thank you for your time")
            speak("thank you for your time")
            break
        elif 'open youtube' in query:
            webbrowser.open("youtube.com")
            speak("Opened ")
        else:
                
                try:
                    app_id="9RK26T-QJ3PYX29TG"
                    client=wolframalpha.Client(app_id)
                    res = client.query(query)
                    answer=next(res.results).text
                    print("wolframalpha: ",answer,"\n")
                    speak(answer)
                    
                except:
                    answer=wikipedia.summary(query,sentences=2)
                    print("wikipedia: ", answer,"\n")
                    speak(answer)

            
