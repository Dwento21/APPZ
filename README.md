import speech_recognition as sr
import pyttsx3
import datetime
import webbrowser
# Функція для розпізнавання голосу
def recognize_speech():    
recognizer = sr.Recognizer()
    with sr.Microphone() as source:        
    print("Скажіть щось:")
        recognizer.adjust_for_ambient_noise(source)       
        audio = recognizer.listen(source)
    try:        
    print("Розпізнано: " + recognizer.recognize_google(audio, language="uk-UA"))
        return recognizer.recognize_google(audio, language="uk-UA")    
        except sr.UnknownValueError:
        print("Не розпізнано")      
        return ""
    except sr.RequestError as e:        
    print("Помилка сервісу: {0}".format(e))
        return ""
# Функція для синтезу мовлення
def speak(text):    
engine = pyttsx3.init()
    engine.setProperty('rate', 150)  # швидкість голосу    
    engine.setProperty('volume', 100)  # гучність (від 0 до 1)
    engine.say(text)    engine.runAndWait()
# Основний цикл роботи помічника
while True:
    command = recognize_speech().lower()
    if "привіт" in command:        
    speak("Привіт, я ваш голосовий помічник!")
    elif "яка зараз дата" in command:   
    now = datetime.datetime.now()
        current_date = now.strftime("%d-%m-%Y")  
        speak(f"Сьогодні {current_date}")
    elif any(keyword in command for keyword in ["який зараз час", "скільки годин", "що за година", "яка зараз година",                                                "годинник скільки показує", "котра година зараз",
                                                "скажи, будь ласка, час", "котре число годин",                                                "підкажи, яка година", "скажи, будь ласка, скільки часу",
                                                "котра зараз година", "на годиннику яке число"]):      
        now = datetime.datetime.now()
        current_time = now.strftime("%H:%M")       
        speak(f"Зараз {current_time}")
    elif "відкрий телеграм" in command:        
    webbrowser.open("https://web.telegram.org/")
    elif "відкрий youtube" in command:       
    webbrowser.open("https://www.youtube.com/")
    elif "відкрий google" in command:        
    webbrowser.open("https://www.google.com/")
    elif any(keyword in command for keyword in ["пока", "закінчити команду", "до побачення"]):        
    speak("До зустрічі!")
        break
