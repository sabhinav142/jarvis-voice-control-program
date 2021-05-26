# jarvis-voice-control-program
import pyttsx3
import speech_recognition as sr
import engineio
import wikipedia
import datetime
import random
import webbrowser
import pyaudio
import os
from gtts import gTTS
import bs4
import pyautogui
import winshell
import pywhatkit

engineio = pyttsx3.init('sapi5')
voices = engineio.getProperty('voices')
#print(voices[1].id) #1 for girl voice
engineio.setProperty('voice',voices[1].id)

def speak(audio):
    engineio.say(audio)
    engineio.runAndWait()
def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Good Morning")
    elif hour>=12 and hour<17:
        speak("Good afternoon")
    else:
        speak("Good evening")
    speak("Hi i am your alexa please tell how may i help you")
def screenshot():
    img = pyautogui.screenshot()
    img.save("C:\\Users\\sabhinav11201\\Desktop\\jarvis scren shot\\.screen.jpg")
    speak("I have taken the screenshot")

def takeCommand():
    r= sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        audio = r.listen(source)
    try:
        print("Recognizing...")
        query = r.recognize_google(audio,language='en-in')
        print(f"user said: {query}\n")
    except Exception as e:
        #print(e)
        print("say it again")
        return "None"
    return  query
if __name__=="__main__":
    speak("Hi Shubhendu")
    wishMe()
    while True:
        query = takeCommand().lower()
        if 'wikipedia' in query:
            speak('searching wikipedia...')
            query = query.replace("wikipedia","")
            results = wikipedia.summary(query,sentences=10)
            speak("According to wikipedia")
            print(results)
            speak(results)
        elif 'open youtube' in query:
            speak("opening in youtube")
            webbrowser.open("youtube.com")
        elif 'open google' in query:
            webbrowser.open("google.com")
        elif 'the time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"sir the time is {strTime}")
        elif 'open excel' in query:
            speak("Opening Microsoft Excel")
            os.startfile('C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Microsoft Office 2013\\Excel 2013.lnk')
        elif 'open word'in query:
            speak("opening Microsoft word")
            os.startfile('C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Microsoft Office 2013\\Word 2013.lnk')
        elif 'open outlook'in query:
            speak("opening Microsoft outlook")
            os.startfile('C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Microsoft Office 2013\\outlook 2013')
        elif 'open gmail'in query:
            speak("opening gmail")
            webbrowser.open("www.gmail.com")
        elif 'search on chrome' in query:
            speak("what should i search sir")
            search = takeCommand()
            chromepath = 'C://Users//sabhinav11201//AppData//Local//Google//Chrome//Application//chrome.exe %s'
            webbrowser.get(chromepath).open_new_tab(search+'.com')
        elif 'search on google' in query:
            speak("what should i search")
            search = takeCommand()
            path = 'http://google.com/search?q=' + search
            webbrowser.get().open(path)
        elif 'search on youtube'in query:
            speak("what do you want to search")
            results = takeCommand()
            path = 'https://www.youtube.com/results?q=' + results
            webbrowser.get().open(path)
        elif 'search location'in query:
            speak("what do yo want to search")
            location = takeCommand()
            path = 'http://google.com/maps/place/'+ location + '/&amp;'
            webbrowser.get().open(path)

        elif 'search contact'in query:
            speak("which contact you want to search")
            search = takeCommand()
            path = 'https://www.linkedin.com/search/search/people/?keywords=' +search
            webbrowser.get().open(path)
        elif 'screenshot'in query:
            screenshot()
        elif 'shutdown'in query:
            speak("do you realy want to shutdown")
            reply = takeCommand()
            #shutdown = takeCommand()
            if 'no' in reply:
                exit()
            else:
                os.system("shutdown /s /t 1")
        elif 'logout' in query:
            speak("do you realy want to logout")
            reply = takeCommand()
            if 'no' in reply:
                exit()
            else:
                os.system("shutdown -l")
        elif 'restart'in query:
            speak("do you really want to restart")
            reply = takeCommand()
            if 'no' in reply:
                exit()
            else:
                os.system("shutdown /r /t 1")
        elif "write a note" in query:
            speak("What should i write, sir")
            note = takeCommand()
            file = open(r'C:\Users\sabhinav11201\Desktop\jarvis.txt', 'w')
            speak("Sir, Should i include date and time")
            snfm = takeCommand()
            if 'yes' in snfm:
                strTime = datetime.datetime.now().strftime("%m-%d-%Y %H:%I%p")
                file.write(strTime)
                file.write(" :- ")
                file.write(note)
            else:
                file.write(note)
        if 'play' in query:
            #speak("which song you want to play")
            play = takeCommand()
            song = query.replace('play','')
            speak('playing' +play)
            pywhatkit.playonyt(song)











