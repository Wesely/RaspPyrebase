# RaspPyrebase
Running Pyrebase on Django with Raseberry Pi 3 - Raspbian and PYTHON3


- Create an Firebase project

- Use realtime database, change database -> rules to :

```
{
    "rules": {
    ".read": "true",
    ".write": "true"
  }
}

```


Starting SSH service on Raspbian/Raspberry Pi
https://goo.gl/voyZ4D

# Testing
Running this in Python3
```
import pyrebase

config = {
  "apiKey": "AIzaSyCpAaeOr0LqZCkeU5ZzN9xC7SeLzJRVY-M",
  "authDomain": "raspberryexperiment.firebaseapp.com",
  "databaseURL": "https://raspberryexperiment.firebaseio.com",
  "storageBucket": "raspberryexperiment.appspot.com"
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
