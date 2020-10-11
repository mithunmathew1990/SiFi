# SiFi


Introduction
============
My plan here is to create a wireless stack which uses sound waves to carry information wirelessly.

I also want to turn this into a tutorial for radio systems. Therefore I will do my best to have the stack closely resemble an RF system.

- Sound waves (free to use) will be used instead of radio waves (licensed/restricted) as we can tinker freely
- Each OSI layer will be explained in optimal detail
- Each layer of OSI and each feature will be implemented as distinctly as possible
- Stack will be built in such as fashion, that replacing the physical layer with a radio system will be very easy
- Stack will explain physical layer implementation as if it is a system that uses a wireless radio (RF)

*This projects expects that you are already familiar with the OSI model and some basic computer networking knowledge.


Regarding License
=================
In order to ensure that the project remains completely free and unencumbered by anyone's copyright monopoly, it is advisable that you as a major contributor, explicitly dedicate your code-base contributions to the public domain. If you are contributing code to this project outside of "The Unlicense" license, please ensure that the license details are also brought in.


Table of contents
=================
1. Physical Layer in practise
   1. Sound vs Radio waves?
   1. Basic terminologies
   1. The science of 1's and 0's
   1. Need for Preamble and Syncword
   1. Sending and receiving a bit
   1. Sending the payload and receiving it?
   1. Energy consumption considerations
   1. Frequency Shift Keying (FSK)
   1. Chirp spread specturm (CSS)
   1. Channel usage considerations; Channel hopping / Frequency hopping (FH)
   1. Covering the distance; Spread specturm vs narrow band

Chapter 1 : Physical Layer in practise
======================================
Physical layer of OSI stack deals with getting data from point A to point B. This is were the software meets the physical hardware. In our case, our physical layer will use sound waves to propogate information though the air onto another system. Our computer's speaker and microphone will be used as the transmitter and receiver for this purpose. In a radio system, they use antenna to the same effect.
  1. Sound vs Radio waves <br>
There are several reasons why we are implementing this stack using sound waves instead of radio waves. The main reason is that radio equipment and its use is tightly controlled in all countries and you often need to acquire R&D license even for hobby projects. Radio hardware is usually costly and with all its wires and connections is a messy thing to deal with. Since we are here to learn concepts lets skip the messy area. <br>
Sound and Radio waves belong on the same spectrum (although far away form each other) and therefore have a lot in common. The method of encoding and decoding data with sound waves and that with radio waves are essential same.<br>
  1. Basic terminologies <br>
When it comes to a wireless system there are several terminologies that you ought to be familiar with viz. <br>
**Payload** : The data that we wish to transmit. If we wish to send the plain-text "Apple" from A to B then "Apple" is our payload. This term will evolve as we go. <br>
**Frame** : Lets just say that it is a sequence of data that makes some sense to us. Like how "Apple" made sense before. This term will also evolve as we go. <br>
**Carrier** : For a basic defenition lets think of this like a donkey which carries wood, here donkey is our carrier and wood is what is carried. In our case sound waves will carry our data, so sound waves can be considered as our carrier.<br>
**CRC** : Cyclic Redundancy Check is a famous error detection code<br>
**Baud Rate** : Number of bits we will be transmitting per second<br>
**0b11001100** : 0b at starting indicates that its a binary value<br>
**0xCC** : 0x at starting indicates that its a hex value<br>
**noise** : Since we are using sound waves, noise means noise in its literal sense. In RF systems though, it means unwanted signals. See the similarity?
<br>
**Wireless domain specific terms :**<br>
**Carrier Frequency** : Since we are using sound waves to carry data, the sound wave used for this purpose will have a frequency and this value is carrier frequency.<br>
**number of Channels** : The number of distinct carrier frequencies that we use in our system<br>
**channel** : Each individual carrier frequency can send and receive data and is therefore considered as one channel.<br>
**channel spacing** : The gap in frequency between each of the channels is called channel spacing<br>
  1. The science of 1's and 0's <br>
Since are intereseted in sending digital data, we have to develop a means of sending our binary digits 1 and 0 using our carrier.<br>
One of the easiest ways in which this can be done is by a technique called **On-Off Keying (OOK)**. As the name suggests, the idea is really simple, carrier wave is emitted for bit value 1 and is not emitted for bit value 0.<br>
e.g. Sending 10101010 **frame** would be equivalent to CW_ON CW_OFF CW_ON CW_OFF CW_ON CW_OFF CW_ON CW_OFF (Carrier ON  Carrier OFF ...). As you can see from the example the CW will be ON/OFF for a certain duration for each bit and this time period will utimately determine the number of bits we can transmit per second, which is nothing but our **Baud Rate**.<br>
Q: What if our data is 0b00000000?<br>
Yes we have a problem here as OOK will generate CW_OFF CW_OFF CW_OFF CW_OFF CW_OFF CW_OFF CW_OFF CW_OFF. Since the carrier wave if OFF by default, no data is transmitted and the reciever never gets our intended data 0b00000000.<br>
  1. Need for preamble, syncword and payload size<br>
Lets solve our problem first. How can we transmit 0b00000000? It is obvious now that we have to transmit at least 1 bit value which is 0b1 so that the receiver end knows the start of frame. In this case we always start our transmissions with 0b1 and our new frame is thus 0b100000000. Great now the receiver can figure out where the frame starts, but where does it end? Receiver can know this in two ways, the frame inself carries few bits which will tell how many data bytes are present or the sender and receiver has agreed to a **payload size**, which would be 1 byte in our case. Lets say that in our case we have told the receiver that we will always transmit 1 byte payload at a time. Now the receiver now knows where the frame starts and it can figure out where it would end by reverse calculating the time needed to deliver the payload.<br>
Problem solved! The receiver can now recieve any data byte we wish to send. But now there is a new problem. If we keep the system ON, Ambient noises would sometimes result in an accidental 0b1 and the receiver now thinks we have transmitted a payload and thus reads a junk payload from noise.<br>
Ok, we can solve this problem by increasing the number of 0b1 or maybe even by using a 0b10101010 pattern. This pattern is called **preamble** and indicates the start of frame. Since picking up a 0b10101010 pattern out from noise is highly unlikely, our receiver will stop reading junk data out from noise.<br>
I hate to be the bearer of bad news, but sadly this is not enough. Practically, the sender and receiver will be two different systems each aged differently under different circumstances. Thier clock frequencies, value of carrier frequency, ... would be a little off from each other. Thus by the time the receiver catches on the the 0b10101010 pattern we could potentially miss a few bits. This could result in the payload getting dropped or few data bits of our payload being mistaken for preamble. To solve this we can add a pattern just after preamble and right before payload called a **syncword**. Lets consider this as 0xCC or 0b11001100. Our new frame thus becomes 0b10101010 11001100 00000000. Cool, now even if we drop a few bits from preamble we can still figure out where the payload starts exactly.<br>
_* Normally, number of preamble bits that must be received is preset when we configure the system_
