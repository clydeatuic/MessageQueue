# MessageQueue
Advance Message Queueing Protocol

### Setup
1. Install python with pip
  * To install pip on windows, download [get-pip.py](https://github.com/clydeatuic/MessageQueue/blob/master/get-pip.py) to a folder on your computer. Open a command prompt window and navigate to the folder containing ```get-pip.py```. Then run ```> python get-pip.py```
  > Still got an issue? try [this](https://github.com/BurntSushi/nfldb/wiki/Python-&-pip-Windows-installation)
2. Install pika ```> pip install pika```
3. Create [AMQP account](https://www.cloudamqp.com/) and copy AMQP instance [URL](https://customer.cloudamqp.com/instance)

### Demo Codes
* Default Exchange
```python
# amqp_01_producer.py
# producer code
import pika

url = '[your AMQP instance URL here]'
params = pika.URLParameters(url)
connection = pika.BlockingConnection(params)
channel = connection.channel()

msg = 'Hello UICians!'
channel.basic_publish(exchange='', routing_key='hello', body=msg)
```

```python
# amqp_01_consumer.py
# consumer code

import pika

url = '[your AMQP instance URL here]'
params = pika.URLParameters(url)
connection = pika.BlockingConnection(params)
channel = connection.channel()
channel.queue_declare(queue='hello')

def callback (ch, method, properties, body):
	print " [x] Received %r" %(body)

channel.basic_consume(callback, queue='hello', no_ack=True)			
channel.start_consuming()
```

* Fanout Exchange
* Direct Exchange
* Topic Exchange