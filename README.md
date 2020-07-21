# RealDash-CAN-MQTT-Node-Red-2-way
A Node-Red flow to convert data between RealDash CAN format and MQTT

## Why did I create this?
I was looking for a way to use cheap, readily available ESP8266 boards and firmware to transmit engine sensor data to RealDash.  I own an antique vehicle that does not have OBD-II or an ECU to plug into.  This is one way to solve the problem of getting sensor data from a classic car engine into a modern digital dashboard.

Since all of the available open-source firmware for ESP8266 seems to understand MQTT, I chose to write a translator using Node-Red.

## 2-way support
This Node-red flow is 2-way meaning that it supports transmitting information back to MQTT to control relays, lights, etc.

## My use case
I own a 1962 GM 4106 bus conversion (eg, converted to an RV) powered by a Detroit Diesel 6v96TA engine.  I want engine and other sensor data available to me on my dash.  I want to add sensors without needing to run additional wires and install expensive gauges.  I want to be able to control certain "house systems" from my dash, eg lights and HVAC.  This solution should allow me to expand the system over time as I upgrade certain aspects of my bus.  I can send a signal from a RealDash switch to turn on MQTT-controlled lights or relays.  I can display information from any MQTT sensor on RealDash.

## YouTube video and demo
TODO, I will post a video on my YouTube channel called "This Old Bus" soon. https://www.youtube.com/channel/UC0WtcGAtbh6DgBAW5ckFtgQ/featured?view_as=subscriber

## ESPhome circuit
I'm using the ESPhome firmware to program my ESP8266 boards.  You can use whatever you want.  If you want to see the circuit I'm using, look for the related project on my github page.

## Instructions
This project is driven by 2 files: The Node-red flow, and corresponding RealDash XML file. 
1.  Import the flow into a Node-Red flow.  Configure it to talk to your MQTT broker.
2.  Configure RealDash with a RealDash CAN data source.  Use network, and point it to the IP address of your Node-Red server.  Load the supplied XML configuration file into this new connection.  If you want, you can reconfigure the flow to use bluetooth, but you're on your own getting that to work on your hardware.  I'm using wifi in this example.
3.  The XML file is the "glue" that both systems use for configuration.  There are "pub" and "sub" keys for publish and subscribe added to the various XML ID's.  RealDash ignores these, but the node-red flow uses them to know where to store and send the data.  The XML file gets pasted into the node titled "realdash can xml".  
4.  You will need to edit the flow to contain the MQTT topics (sensors) you wish to subscribe to, and connect them to the function that does the work and sends it on to RealDash, and edit the XML file accordingly AND paste it into the node described above.  You could potentially get lazy and subscribe to all topics, or all topics within a certain group - but I have not done that for performance reasons.  

The job of the code is basically to send and receive data, figure out where in the bit-packed 8-byte data stream it gets read from or written to, and send it along. The "work" of all this has been simplified down to the XML config file, which you need to understand to work with realdash anyways.

## Limitations
1.  All ID's within a realdash frame should be the same data type (eg, bit or byte).
2.  Currently, only byte and bit data types are supported.  I will add Text when I have a need for it.
3.  Little-endian is the only data type supported.
4.  I've tested the code with up to 2-byte ID's and single bit values, although theoretically more should work.
5.  If you send bigger data than the realdash ID is configured for, it'll get truncated.
6.  Designed to send integers to realdash.  If you need decimals, use the "submult" key to cause the code to multiply by 10 or 100, and tell realdash to divide using the "conversion" key.  This trick will let you get decimal values.

## Notes
I'm not a javascript programmer. I don't do all the fancy terse javascript-y stuff.  I come from C++ and PHP.  If you don't like my code, fix it and send me a pull request.  Contributors welcome, and thanks!










