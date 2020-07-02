# fake-ev3-bluetooth #

----------------

## Description ##

Implement the bluetooth messaging module of EV3 pybricks on normal python. Based on PyBluez

In [the offical document of pybricks.messaging](https://pybricks.github.io/ev3-micropython/messaging.html), it says:

> An EV3 Brick can send information to another EV3 Brick using Bluetooth. 
  This page shows you how to connect multiple bricks and how to write scripts to send messages between them.  
> ...

Now you can also connect the EV3 brick and your computer, and let them send messages to each other.

----------------

## Installation ##

This project is based on PyBluez.
So you need to install it first.
```bash
pip install PyBluez
```

Then you just need to simply copy these files to your 'lib/' directory.

---------------

## Usage ##

It's almost like the usage of the offical module for micropython.
Here's a example from the offical document (comments modified).

> Server: 
```python
# Before running this program, make sure the client and server EV3 bricks or 
# PC are paired using Bluetooth, but do NOT connect them. The program  will
# take care of establishing the connection.

# The server must be started before the client!

from pybricks.messaging import BluetoothMailboxServer, TextMailbox

server = BluetoothMailboxServer()
mbox = TextMailbox('greeting', server)

# The server must be started before the client!
print('waiting for connection...')
server.wait_for_connection()
print('connected!')

# In this program, the server waits for the client to send the first message
# and then sends a reply.
mbox.wait()
print(mbox.read())
mbox.send('hello to you!')
```

> Client:
```python
from pybricks.messaging import BluetoothMailboxClient, TextMailbox

# This is the name of the remote EV3 or PC we are connecting to.
SERVER = 'ev3dev'

client = BluetoothMailboxClient()
mbox = TextMailbox('greeting', client)

print('establishing connection...')
client.connect(SERVER)
print('connected!')

mbox.send('hello!')
mbox.wait()
print(mbox.read())
```

> *Note:* When you run it on your computer, it may fail with an OSError like this:
```
Traceback (most recent call last):
  File "<Your Program>", line 1, in <module>
    pybricks.messaging.BluetoothMailboxServer()
  File "C:\Projects\fake-ev3-bluetooth\pybricks\messaging.py", line 251, in __init__
    super(ThreadingRFCOMMServer, self).__init__(
  File "C:\Projects\fake-ev3-bluetooth\pybricks\bluetooth.py", line 97, in __init__
    self.socket.bind(server_address)
  File "C:\Program Files\Python38\lib\site-packages\pybluez_win10-0.22-py3.8-win-amd64.egg\bluetooth\msbt.py", line 72, in bind
    bt.bind (self._sockfd, addr, port)
OSError
```
> The reason may be that you can't bind `BluetoothSocket` on port 1.
  To solve this problem, add this to both the client and server code before instancing the `BluetoothMailboxServer` or `BluetoothMailboxClient` class:
```python
import pybricks.messaging
pybricks.messaging.EV3_RFCOMM_CHANNEL = 20  # Modify the default port. Other port numbers should also work.
```

For more usage, you can read the [offical document](https://pybricks.github.io/ev3-micropython/messaging.html).

Sorry for my bad English.