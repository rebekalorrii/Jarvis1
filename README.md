import subprocess
import sys

def instalar_modulo(nome_modulo):
    try:
        __import__(nome_modulo)  __
    except ImportError:
        print(f"O módulo {nome_modulo} não está instalado. Instalando...")
        subprocess.check_call([sys.executable, "-m", "pip", "install", nome_modulo])

instalar_modulo("speech_recognition")
instalar_modulo("pyttsx3")

import speech_recognition as sr
import pyttsx3
from datetime import datetime

texto_fala = pyttsx3.init()
texto_fala.setProperty("rate", 195) 
voices = texto_fala.getProperty("voices")
texto_fala.setProperty("voice", voices[0].id)  

def falar(audio):
    texto_fala.say(audio)
    texto_fala.runAndWait()

def hora_atual():
    agora = datetime.now()
    return agora.strftime("%H:%M")

def ouvir_comando():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Estou ouvindo...")
        recognizer.adjust_for_ambient_noise(source)
        try:
            audio = recognizer.listen(source)
            comando = recognizer.recognize_google(audio, language='pt-BR')
            print(f"Você disse: {comando}")
            return comando.lower()
        except sr.UnknownValueError:
            falar("Desculpe, não entendi o que você disse. Pode repetir?")
            return None
        except sr.RequestError:
            falar("Erro ao se conectar ao serviço de reconhecimento de voz.")
            return None

def jarvis():
    falar("Olá, como posso te ajudar?")
    while True:
        comando = ouvir_comando()
        if comando:
            if "hora" in comando:
                falar(f"Agora são {hora_atual()}.")
            elif "sair" in comando or "desligar" in comando:
                falar("Desligando o sistema. Até logo!")
                break
            else:
                falar("Desculpe, não fui programado para entender esse comando ainda.")

if __name__ == "__main__":  
    jarvis()

