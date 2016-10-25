# Smart Gas Valve For Safety By NodeRED, MQTT v3.1

***

# Introduction
This instructables will cover the basic steps that you need to follow to get started with open sources such as Node-RED, MQTT v3.1. MQTT(Message Queueing Telemetry Transport) is a Machine-To-Machine(M2M) or Internet of Things (IoT) connectivity protocol that was designed to be extremely lightweight and useful when low battery power consumption and low network bandwidth is at a premium and connection quality could be unreliable. It was invented in 1999 by Dr Andy Stanford-Clark and Arlen Nipper and is now an Oasis Standard.

The different tutorials can cause a great deal of confusion, which is why I have tried to make the easiest setup possible. Specifically, this instructables will cover how to code the Node-RED on Raspberry Pi2 as a MQTT client by connecting to your home wireless network and how to send sensor data ( we are going to send sensor data to test, and the installation guides of Raspberry Pi2, Node-RED will not really be covered in this instructables).

The following are the materials we will be using :

* Raspberry pi2 (Pi-1,2,3 are Possible.): Installation guide || Download here
* Wifi dongle (If you have Pi-3, it's unnecessary.)
* Magnetic switch sensor
* Smartphone's portable battery
* Nod-RED: Use the version pre-installed in Raspbian Jessie image since November 2015. Installation guide
* MQTT v3.1


***
# Step 1: Step 1: Setting up the smart gas valve with Raspberry Pi
## Assembly steps
(1) Connect the Raspberry Pi2 with a magnetic switch sensor as shown above in the circuit diagram.

(2) Add a pull-up resistor(4.7 k ohm). If you use a really long wire, you could change approximately 1 k ohm. It depends on your environment.

(3) Connect the magnetic switch sensor with raspberry pi.

* COM --- Ground
* NO --- GPIO 18

(4) Assemble the magnetic switch sensor with a gas valve at home.

(The magnetic switch sensor should include an adhesive tape on the base.)

(5) Connect a portable battery with Raspberry Pi2.

(Use any portable battery to connect with same size connector cable on Raspberry Pi2. )


***
# Step 2: Programming NodeRED on Raspberry Pi2
## How to start Node-RED on web browser.
(1) Write down command shown below to a terminal window.
#
   `
   node-red-start
   `
#
(2) You can find an IP address as below.

'Once Node-RED has started, point a browser at http://169.254.170.40:1880'

(3) Open your web browser.

(4) Copy the ip address and paste on web browser.

(5) It will display a visual editor of Node-RED on web browser.

(6) You can start coding with visual editor on web browser.

(7) Try dragging & dropping any node from left hand side to right hand side. It's really easy to code.

( You can conveniently use the visual editor off line as well as on line. )

## Download the 'SmartGasValve_NodeRED.txt' file.
(1) Click number (1) at the right hand side corner shown in NodeRED on web browser.

(2) Click the Import button on the drop down menu.

(3) Open the Clipboard shown in the above 1st picture.

(4) Lastly, paste the given JSON format text of 'SmartGasValve_NodeRED.txt' in Import nodes editor.


***
# Step 3: Setting up MQTT v3.1 on Raspberry Pi2
## Setting up MQTT v3.1 on Raspberry Pi2
This message broker(Mosquitto) is supported by MQTT v3.1 and it is easily installed on the Raspberry Pi and somewhat less easy to configure. Next we step through installing and configuring the Mosquitto broker. We are going to install & test the MQTT “mosquitto” on terminal window.
#
`
curl -O http://repo.mosquitto.org/debian/mosquitto-repo.gpg.key
`
#
`
sudo apt-key add mosquitto-repo.gpg.key
`
#
`
rm mosquitto-repo.gpg.key
`
#
`
cd /etc/apt/sources.list.d/
`
#
`
sudo curl -O http://repo.mosquitto.org/debian/mosquitto-repo.list
`
#
`
sudo apt-get update
`

### Next install the broker and command line clients:

* mosquitto – the MQTT broker (or in other words, a server)
* mosquitto-clients – command line clients, very useful in debugging
* python-mosquitto – the Python language bindings

#

`
sudo apt-get install mosquitto mosquitto-clients python-mosquitto
`

#

As is the case with most packages from Debian, the broker is immediately started. Since we have to configure it first, stop it.

#

`
sudo /etc/init.d/mosquitto stop
`

#

Now that the MQTT broker is installed on the Pi we will add some basic security.

#

Create a config file:

#

`
cd /etc/mosquitto/conf.d/
`
`
sudo nano mosquitto.conf
`
**
Let's stop anonymous clients connecting to our broker by adding a few lines to your config file. To control client access to the broker we also need to define valid client names and passwords. Add the lines:
**
`
allow_anonymous false
`
#
`
password_file /etc/mosquitto/conf.d/passwd
`
#
`
require_certificate false
`
#
* Save and exit your editor (nano in this case).
* From the current /conf.d directory, create an empty password file:
#
`
sudo touch passwd
`
#
We will to use the mosquitto_passwd tool to create a password hash for user pi:
#
`
sudo mosquitto_passwd -c /etc/mosquitto/conf.d/passwd pi
`
#
You will be asked to enter your password twice. Enter the password you wish to use for the user you defined.
## Testing Mosquitto on Raspberry Pi
Now that Mosquitto is installed we can perform a local test to see if it is working:
Open three terminal windows. In one, make sure the Mosquitto broker is running:
#
`
mosquitto
`
#
* In the next terminal, run the command line subscriber:
#
`
mosquitto_sub -v -t 'topic/test'
`
#
You should see the first terminal window echo that a new client is connected. In the next terminal, run the command line publisher:
#
`
mosquitto_pub -t 'topic/test' -m 'helloWorld'
`
#
You should see another message in the first terminal window saying another client is connected. You should also see this message in the subscriber terminal:
#
`
topic/test helloWorld
`
#
We have shown that Mosquitto is configured correctly and we can both publish and subscribe to a topic. When you finish testing all, let's set up below that.
#
`
sudo /etc/init.d/mosquitto start
`
#

***
# Step 4: Checking your NodeRED codes with MQTT on Raspberry Pi2



***
# Step 5: Adding & Setting up E-Mail, Twitter, and Twilio




***
# Step 6: Adding autostart file for every boot.
