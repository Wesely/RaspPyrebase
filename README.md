# RaspPyrebase
Running Pyrebase on Raseberry Pi 3 (with Raspbian and PYTHON3)


## Create a Firebase Realtime Database
Firebase privides realtime database that we can use it to sync data on all of our divices and web.

- Create, choose *realtime database* at https://firebase.google.com/
- Under `database -> rules` tab, set to public for this practice. 
```json
{
    "rules": {
        ".read": "true",
        ".write": "true"
  }
}
```
Firebase's Databases can be inport/export as JSON format, 

I've wrote a JSON mapping to Raspberry's GPIO: https://github.com/Wesely/RaspPyrebase/blob/master/res/firebase_structure.json

import this JSON as your database structure.

# Running on RaspBerry
## Install Pyrebase
a Python wrapper for the Firebase API.
Install *https://github.com/thisbejim/Pyrebase*
```
$ pip install pyrebase
```

- Access Raspberry via ssh? check this: https://goo.gl/voyZ4D

## Testing
Run Python3 on Raspberry
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


## Configuring GPIO
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
