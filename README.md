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
   a. Sound vs Radio waves?
   b. Basic terminologies
   c. The science of 1's and 0's
   d. Sending and receiving a bit
   e. Sending the payload and receiving it?
   f. Need for Preamble and Syncword
   g. Energy consumption considerations
   h. Channel usage considerations; Channel hopping / Frequency hopping (FH)
   i. Covering the distance
   j. Spread specturm vs narrow band
   h. Spread specturm in practise : chirp spread specturm (CSS)
