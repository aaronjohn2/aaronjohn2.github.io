---
layout: post
title:  High-quality Bass From Small Speakers
date:   2015-12-01
tags: Physics
author: "Aaron John"
---

**A project by David Mohn and Aaron John.** In this report we discuss the route we took in designing and creating a small form factor speaker with a high amount of bass, without sacrificing sound quality.

<div style="text-align:center;">
<iframe width="420" height="315" src="https://www.youtube.com/embed/Z0m_2z4FU4s?autoplay=1&amp;loop=1&amp;rel=0&amp;showinfo=0&amp;playlist=Z0m_2z4FU4s" frameborder="0" allowfullscreen></iframe>
<br>
Our final product.
</div>


## 1.Abstract
In this report we will be discussing the route we took in designing and creating a small form factor speaker with a high amount of bass without sacrificing sound quality. To do this, we did a lot of research on different types of enclosure designs and made a decision on what would suit our application the best. What we ended up choosing was the transmission line(T-line) enclosure, also known as a quarter wave enclosure. The reason we chose this enclosure over the other options is because of the T-lines excellent low frequency response as well as keeping the midrange and highs in check. After we chose the type of enclosure, we researched on what driver we would need to use. To choose the right driver, we had to look at many different options and as to which one had the correct specification to suit our needs. Once we had chosen the driver and had the idea of how we wanted our enclosure, we took the design principles of a T-line enclosure and created a 3d model of our enclosure in autodesk inventor. After designing the enclosure in autodesk, we then 3d printed the enclosures of our design. Finally we assembled all the parts of the speaker.

## 2.Introduction
Speakers operate by moving a mass of air in such a way that it creates audible sound. Typically, speakers convert varying frequency alternating current into sound. All speakers are either passively powered  (passive speaker), or actively powered (active speaker). Passive speakers do not have a built-in amplifier and must be connected to an amplifier using a regular speaker wire. Most speakers that are commercially available are passive. Active speakers are internally amplified speakers. They feature built-in amplifiers for high and/or low frequencies. Active speakers often require heavy enclosures, making them larger, less mobile and functional as passive speakers. They are sometimes less reliable since they have built-in electronic components that require external power sources[1].

Speakers are categorized depending on a number of characteristics, including the types of drivers and enclosure used in their construction. The common types of speaker systems are tweeter, woofer, midrange, subwoofer, midbass, and fullrange. The tweeter is a speaker that is made to cover only high frequencies going from around 2,000 Hz all the way up to the limit of audible frequencies at 20,000Hz. Tweeters are typically smaller speakers and made of very light weight materials because of the fast movement they have to achieve to reach these high frequencies. Another very common speaker is the woofer. The woofer is used to cover midrange and low frequencies from around 2,000Hz down to around 60 Hz. They are usually larger and made of stiff materials so they can achieve the low frequencies that are required from the speaker.  The next common type of speaker is a midrange. This is a speaker that fits in between the tweeter and the woofer. It is used to cover the midrange frequencies from around 2,000 Hz down to around 200 Hz. These are similar to woofers, but they are typically much smaller and and made of lighter materials to get the desired frequency response. They are normally used only in very high end speaker systems. Then the fourth type is the subwoofer. It is used to cover all the low frequencies from 80Hz, all the way down to the limit of human hearing at 20 Hz. Subwoofers are always very large to achieve the surface area needed to create such low frequencies. They typically require a lot more power than other types of  speakers because of their size and weight.  The fifth type is the midbass. Like a midrange, these are typically used only in high end speakers. They fit in between a midrange and the subwoofer in the frequency spectrum. They play frequencies from around 200 Hz down to around 30-40 Hz. The last type is the fullrange speaker. These are the most used speaker type because of their low cost and their versatility. They usually cover nearly all of the frequency spectrum and are used in phones, small desktop speakers, TVs, radios, and even computers. They are used in many kinds of systems but they usually only have one downfall which is their low frequency performance[2].

In addition to the type of speakers, the enclosure used in the construction is an integral part of the speaker performance. The most commonly used enclosure design is a sealed enclosure since it is easy to design and is low in cost.  The second type is the Standard ported enclosure which consists of a chamber with a port of a certain length that exist outside the enclosure to help improve low end response. These are typically used in larger speakers like tower speakers or subwoofer. The last type of design is the transmission line enclosure. These are one of the more complex enclosures to design but they tend to offer the best results. They greatly improve low end response of a speaker as well as make sure that there are no unwanted peaks in audio levels throughout the spectrum.

<div class="imgcap">
<img src="/assets/physics/image4.jpg">
</div>


The most basic transmission line enclosures consist of a long tube that is tuned to the length of a quarter of a wavelength of a certain frequency, usually around 30-40 Hz to help with low frequencies. Because of how efficient transmission line enclosures are in improving low frequency response, we decided to attempt to get the most bass and low frequency response we can from a small speaker. A reason why someone may want something like this is because of the great low end response and the small form factor that lets the speakers be more versatile[3-5].

## 3.Approach
To start our project, we first had to do some research on what makes a good speaker. To do this, we researched different kinds of enclosures, drivers, and crossover systems. To make a final decision on what kind of enclosure we would choose and what kind of drivers we would use, we had to look at all the variables and pick what would suit our desired application the best. The first decision we made was for the type of enclosure. We ended up choosing the transmission line (t-line) also known as a quarter wave enclosure. The reason we ended up choosing this enclosure type is because of the low end response it offers and the fact that it also can help flatten out the midrange response to give more accurate sound reproduction. The transmission line operates on the principle of an acoustic standing wave at a quarter of the drivers resonant frequency. The transmission line enclosure at its bare essentials is basically a tapering resonant tube. The factors that change the enclosures acoustic response are the length of the tube and the rate at which it tapers either in on itself or out[6-9]. When coming up with the design of the enclosure, the variables can be looked at like a circuit. Like in electronics, when dealing with acoustics you need to deal with an acoustic impedance and resistance as well as other properties of the driver such as mechanical compliance and acoustic mass. To calculate the enclosure properties, you use the same mathematical theory that is used in electronics engineering to calculate damping and transient response of a series RCL (resistor, capacitor, inductor) circuit and substitute the acoustic equivalents[10].

<div class="imgcap">
<img src="/assets/physics/image5.png" height="300">
<img src="/assets/physics/image1.jpg">
</div>


[7] R would be the driver acoustic resistance, C would be the drivers acoustic compliance, and L would be the drivers acoustic mass. The enclosures acoustic impedance is also included in the calculation of the “voltage” source in the acoustic circuit (figure 1.2) is the pressure source that is created from the driver at certain frequencies. This “voltage” source will be treated as an AC(alternating current) source at the drivers resonant frequency. What needs to be calculated is the “current” of the circuit which would be the drivers cone velocity in this case[10]. Once this is calculated, you can then calculate the rest of the properties needed for the enclosure.   

<div class="imgcap">
<img src="/assets/physics/image2.jpg">
<img src="/assets/physics/image3.png" height="400">
</div>


[7] For the next step in our goal to create a speaker, we had to find a driver that would work well with a transmission line enclosure as well as being small. To pick the driver, we had to look at the driver's size, resonant frequency, frequency response, and moving mass. The driver needed to be small so that it would fit in our application. The frequency response as well as the resonant frequency are the two most important variables when it comes to choosing the driver. The reason these are important is because they dictate the level of accuracy a driver will have in sound reproduction. The Frequency response is the range of different frequencies that a driver can cover. This is usually depicted as a graph where the x axis is frequency and the y axis is the sensitivity and for the most part, a flatter response means a more realistic sound reproduction. The frequency response is also used to choose drivers to match together in a multi-driver system. The resonant frequency is the point at which the driver is most sensitive and resonates. It is important to know what the drivers resonant frequency is because you can design the enclosure to fix the jump in sensitivity at that frequency to make a better sounding speaker with a flat response. What we ended up choosing was a 3 inch aurasound driver that fit all of our needs[11]. After we chose the enclosure type and the driver, we had to design the enclosure. We used the principles of a transmission line discussed above to get design elements we need to create a transmission line. At its bare minimum, the transmission line is just a tapering tube that has a specific length and rate of taper. While this simplified design would work, it would not suit our needs. To make this type of enclosure fit into a small form factor like we want, we had to bend the enclosure. By making the tapering tube of the enclosure follow a series of folded curves, we are able to compress the design into a smaller form factor without compromising on the length of the transmission line.

<div class="imgcap">
<img src="/assets/physics/imageEnclosure.png" height="715" width="820">
</div>

Next, we took the data we got from the calculations and used those to design a 3d model of the enclosure in autodesk inventor, which we then converted to an stl file to be 3d printed. The material we used to 3d print the enclosures out of is a plastic called PLA. It is a dense and rigid thermoplastic derived from corn oil that is strong enough to support a lot of stress. During the 3d printing process, we had an issue with the 3d printer where the print would fail and break at a certain point in the printing process. To fix this problem, we had to figure out what was causing the failure. To do this we had to look through all the settings on the 3d printer to figure out what would be causing the problem. After looking through the settings, there was no obvious cause as with other 3d prints, there were no issues like what we saw with the enclosure. After trying many different things, we found out that the cause of the failure was simply because of one setting that is not easily seen. The setting that controls how the printer sees the build plate had been changed so that the printer would see the build plate as being slightly convex instead of flat. The reason this caused issues even though it was only off by a very small amount is because the printhead had no room at certain points to lay down material. This caused the material to clog up the print head which kept it from printing certain layers. This made the 3d print very weak and caused the first two attempts to fail early on. With that problem solved, we were able to continue with the 3d printing process. Next we assembled the parts of the enclosure together using a high temperature hot glue. Before sealing the enclosure we installed the drivers, terminals, and ran the cable that connected the speaker to the terminals so that it could be powered by an external amp to verify that everything is working.     	

## 4.Discussion & Results
When designing the enclosure, there were a few different ways we could have built a transmission line speaker. At its most basic, a transmission line enclosure is a tapering tube of a given length that is determined based on the driver being used. Because of this we can bend and curve the tube to get the desired length in a more compact design. With the design freedom that 3d printing gives you, we created a design with 3 curves inside with no sharp edges. Whereas this design would be much more difficult to create with a wooden enclosure manually.

Once both enclosures where glued together and the wires and drivers were installed, we went ahead and tested out the speakers by connecting them to an amplifier and playing some music through them. This was to test out how well our enclosures would work and if they ended functioning as we had originally planned for them. In the end they did sound quite good and had a decent low end response, though not as great as we had been expecting. This was likely due to the density of the material the enclosure was made of and the way it was 3d printed. It was printed with black PLA plastic at a layer height of 0.25 millimeters with a nozzle diameter of 0.4 millimeters. It was also printed at only an 8% infill, which means that the inside of the solid parts of the 3d print were mostly hollow with a grid pattern inside for strength. Both enclosures were 3d printed out of 4 separate parts. 2 pieces for the main enclosure and another 2 to cover the open side of the enclosure. Because the material was lightweight and not very dense, i beleive that led to the enclosure vibrating and creating some destructive interference within it that prevented it from giving the best possible low end response from our chosen driver. Overall, this project was an interesting experience where we learned about how speakers function and how they are designed.

## 5.Summary
Our goal with this project was to get the most bass and low end response possible from a small speaker. We chose to use the transmission line enclosure for this project because of its efficient reproduction of low frequencies while maintaining a flat response. Secondly, we ended up choosing a 3 inch aurasound drivers because of its small size and good frequency response. Next we designed the enclosure in autodesk inventor and then 3d printed the enclosure in a plastic material.  Finally, we assembled the 3d printed enclosure, driver, and other parts into the final design. 	


## 6.References

[1] Briere, Danny and Hurley, Pat. “The Essentials of Speakers.” Dummies. Dummies, n.d. Web. 22 November 2015. <http://www.dummies.com/how-to/content/the-essentials-of-speakers.html>

[2] Fostex. “Speaker Components.” Fostex. Fostex, n.d. Web. 22 November 2015. <http://www.fostexinternational.com/docs/speaker_components/enclosures.shtml>

[3] Lucas, Anthony. “Sealed Vs. Ported Enclosures.” Eminence. Bass Gear Magazine, n.d. Web. 22 November 2015. <http://www.eminence.com/2011/06/sealed-vs-ported-enclosures/>

[4] LoudspeakerBuilder. “Types of Audio Speaker Enclosures.” members shaw. members shaw, n.d. Web. 22 November 2015. <http://www.members.shaw.ca/loudspeakerbuilder.ca/types.html>

[5] Sabolish, Michael. “Speaker Cabinets.” Duncan’s amps. Duncan’s amps, n.d. Web. 22 November 2015. <http://www.duncanamps.com/technical/speaker_cab.html>

[6] Risch, Jon. “Classic TL Design.”  t-line speakers. t-line speakers, n.d. Web. 22 November 2015. <http://www.t-linespeakers.org/design/classic.html>

[7] King, Martin J. “Quarter Wavelength Loudspeaker Design.” quarter-wave. quarter-wave,
17 July 2002. Web. 22 November 2015. <http://www.quarter-wave.com/>

[8] Weems, David B. “Experiments With Tapered Pipes.” Janbatist. Janbatist, n.d. Web. 22 November 2015.
<http://janbatist.narod.ru/Sound/4SubwofersRouporsFaziki/ExperimentswithTaperedQuarterWavePipes.htm>

[9] Neurochrome Audio. “Designing MLTL.” Neurochrome. Neurochrome, n.d. Web. 22 November 2015. <http://www.neurochrome.com/designing-an-mltl/>

[10] Kuphaldt, Tony R. “Series R, L, and C.” All About Circuits. Design Science, n.d. Web. 22 November 2015. <http://www.allaboutcircuits.com/textbook/alternating-current/chpt-5/series-r-l-and-c/>

[11] Parts Express. “AuraSound NS3-193-8A 3" Extended Range Driver 8 Ohm.” Parts Express. Parts Express, n.d. Web. 22 November 2015. <http://www.parts-express.com/aurasound-ns3-193-8a-3-extended-range-driver-8-ohm--296-25>
