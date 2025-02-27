<p align="left" >
<a href="https://github.com/RoberWare/pytwinkle/graphs/contributors"><img src="https://img.shields.io/github/contributors/RoberWare/pytwinkle" alt="Github contributors"/></a>
<!-- <a href="https://github.com/RoberWare/pytwinkle"><img src="https://img.shields.io/github/release-pre/RoberWare/pytwinkle" alt="Github release"/></a>
<a href="https://github.com/RoberWare/pytwinkle/stargazers"><img src="https://img.shields.io/github/stars/RoberWare/pytwinkle" alt="Github stars"/></a> -->
</p>

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

#### Description
Twinkle sip client, ported to a python module. This version has been forked from the original to add DTMF support and to modify the exposed SIP Authorization Name to play nicely with Twilio. 
Without this modification, you'll likely get this error in your Twilio console: `Error Code 32106,Authorization name and user ID does not match.`

#### Tested environments

|                         |                                         |
|-------------------------|-----------------------------------------|
| **Hardware**            | Rpi 3 B                                 | 
| **Operating systems**   | RaspiOS                                 |
| **Python versions**     | Python 3.x                              |
| **Distros**             | Raspbian 10                             |
| **Languages**           | English                                 |

#### Instalation
```Shell
sudo apt-get install twinkle
sudo pip3 install pytwinkle
```
To install from this source, please run
```Shell
git clone https://github.com/MReschenberg/pytwinkle-twilio
cd pytwinkle-twilio
pip install .
```

#### Example
```Python
from pytwinkle import Twinkle

def callback(event, *args):
    if event=="registration_succeeded":
        uri, expires = args
        print("registratiom succeeded, uri: %s, expires in %s seconds"%(uri, expires))
        # The module keeps the session, you havent to register
        mTP.message("name@domain", "Hello")
        mTP.call("name@domain")

    if event=="new_msg":
        msg=args[0]
        print("new_msg!: "+str(msg))
    
    if event=="incoming_call":
        call=args[0]
        print("call: "+str(call))

    if event=="cancelled_call":
        line=args[0]
        print("call cancelled, line: %s"%(line))
        
    if event=="answered_call":
        call=args[0]
        print("answered: %s"%(str(call)))
        
    if event=="ended_call":
        line=args[0]
        print("call ended, line: %s"%(line))
    
    if event == "dtmf_received":
        line=args[0]
        key = args[1]
        print("dtmf received, line: %s key: %s"%(line, key))
        
mTP = Twinkle(callback)  
mTP.set_account("name","domain","password")
mTP.run()
```

 - Callbacks
 
| *Event*                  |  *Description*               | *Returns *                             |
|--------------------------|------------------------------|----------------------------------------|
| "registration_succeeded" | When the registration suceed | Line number and seconds to expire (*)  |
| "new_msg"                | New message receives         | msg={'from':uri, 'to':uri 'msg':msg}   |
| "incoming_call"          | Incoming call                | call={'from':uri, 'to':uri}            |
| "cancelled_call"         | Cancelled call               | Line number                            |
| "answered_call"          | Answered call                | call={'msg':msg, 'code':num, 'to':uri} |
| "ended_call"             | Ended call                   | Line number                            |
| "dtmf_recieved"          | Client recieved DTMF         | DTMF key                               |

(*) doesnt matter the seconds to expire, the program keep the session active

 - Functions
   - call		Call someone
   - answer		Answer an incoming call
   - answerbye	Answer an incoming call or end a call
   - reject		Reject an incoming call
   - bye		    End a call
   - hold		Put a call on-hold
   - retrieve	Retrieve a held call
   - redial		Repeat last call
   - register	Register your phone at a registrar
   - deregister	De-register your phone at a registrar
   - message		Send an instant message
   - recieve DTMF  Recieve and react to DTMF

 - Not supported at the moment
   - redirect	Redirect an incoming call
   - transfer	Transfer a standing call
   - conference	Join 2 calls in a 3-way conference
   - mute		Mute a line
   - dtmf		Send DTMF
   - fetch_reg	Fetch registrations from registrar
   - options		Get capabilities of another SIP endpoint
   - line		Toggle between phone lines
   - dnd		Do not disturb
   - auto_answer	Auto answer
   - user		Show users / set active user
   - presence	Publish your presence state
   - quit		Quit


#### Dependencies
- System dependencies
  - python3
  - twinkle
- Python dependencies
    - multiprocessing
    
#### Mentions
  @LubosD - https://github.com/LubosD/twinkle
  
#### Developer
Roberto Lama Rodríguez - roberlama@gmail.com
 
