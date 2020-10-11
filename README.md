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
   1. Sending and receiving a bit
   1. Sending the payload and receiving it?
   1. Need for Preamble and Syncword
   1. Energy consumption considerations
   1. Channel usage considerations; Channel hopping / Frequency hopping (FH)
   1. Covering the distance; Spread specturm vs narrow band
   1. Spread specturm in practise : chirp spread specturm (CSS)

Chapter 1 : Physical Layer in practise
======================================
Physical layer of OSI stack deals with getting data from point A to point B. This is were the software meets the physical hardware. In our case, our physical layer will use sound waves to propogate information though the air onto another system. Our computer's speaker and microphone will be used as the transmitter and receiver for this purpose. In a radio system, they use antenna to the same effect.
  1. Sound vs Radio waves <br>
There are several reasons why we are implementing this stack using sound waves instead of radio waves. The main reason is that radio equipment and its use is tightly controlled in all countries and you often need to acquire R&D license even for hobby projects. Radio hardware is usually costly and with all its wires and connections is a messy thing to deal with. Since we are here to learn concepts lets skip the messy area. <br>
Sound and Radio waves belong on the same spectrum (although far away form each other) and therefore have a lot in common. The method of encoding and decoding data with sound waves and that with radio waves are essential same.<br>
