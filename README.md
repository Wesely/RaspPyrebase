# RaspPyrebase
Running Pyrebase on Django with Raseberry Pi 3 - Raspbian and PYTHON3


## Install Pyrebase
a Python wrapper for the Firebase API.
Install *https://github.com/thisbejim/Pyrebase*
```
$ pip install pyrebase
```

## Create a Firebase Realtime Database
Firebase privides realtime database that we can use it to sync data on all of our divices and web.

- Create, choose *realtime database* at https://firebase.google.com/
- Change `database -> rules` to public, if not, you need to Login to access Firebase later.
```json
{
    "rules": {
        ".read": "true",
        ".write": "true"
  }
}
```


# Starting SSH service on Raspbian/Raspberry Pi
https://goo.gl/voyZ4D

# Testing
Running this in Python3
```python
import pyrebase

config = {
  "apiKey": "SUPER KEY", ERROR!! replace this to ypur apiKey
  "authDomain": "project.firebaseapp.com",
  "databaseURL": "https://project.firebaseio.com",
  "storageBucket": "project.appspot.com"
}

firebase = pyrebase.initialize_app(config)
db = firebase.database()
gpio = db.child("GPIO").get()
# print(gpio.val())

def stream_handler(message):
    '''
    message["event"] : put
    message["path"] : /GPIO_#
    message["data"] : True/False
    '''
    if message["data"]==True and message['path']=='/GPIO_22':
        print ('GPIO_22 is True!!')
    else:
        print ('=口="')

my_stream = db.child("GPIO").stream(stream_handler)
my_stream.close()
```


## Raspberry Pi 3 LED Control
Try this.
``` python
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
# GPIO.setup(5,GPIO.OUTPUT) 
# ^^^^ Error !! GPIO.OUTPUT seems an old syntax
GPIO.setup(5,GPIO.OUT) # using OUT is correct
GPIO.output(5,GPIO.HIGH)
time.sleep(2000)
GPIO.output(5,GPIO.LOW)
```

## Combining GPIO and Firebase:
Just replace the `stream_handler`
```
def stream_handler(message):
    if message["data"]==True and message['path']=='/GPIO_5':
        print ('GPIO_5 is True!!')
        GPIO.output(5,GPIO.HIGH)
    else:
        print ('=口="')
        GPIO.output(5,GPIO.LOW)
```
