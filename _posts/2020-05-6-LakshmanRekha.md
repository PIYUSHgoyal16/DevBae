---
layout: post
author: "Piyush Goyal"
author_url: https://github.com/PIYUSHgoyal16
title: "LakshmanRekha"
subtitle: "A mobile-based automated home quarantine management application using continuous face recognition."
bg_url: "https://user-images.githubusercontent.com/43112419/83101600-4e912c00-a0d0-11ea-87c1-0161ff00d284.jpg"
tags: [home]
---

Pneumonia of unknown cause was detected in Wuhan, China, and was first reported to the WHO Country Office in China on 31 December 2019. Soon it was realized that this was due to the new strain of coronavirus. Coronavirus disease (COVID-19) is a highly infectious disease, and we have over 2.4 million confirmed cases worldwide, with around 170 thousand fatalities. Despite strict measures implemented by many countries including India, the spread of the virus has only been delayed if not stopped completely. In the face of the Covid-19 outbreak, it is crucial to learn about the disease and make attempts to escalate it further. The only strong response against this invisible enemy currently is to follow strict social distancing and home quarantine properly.

__Table of Contents__


1. **[Introduction](#understanding-git)**
2. **[Scope and Utility](#install-setup)**
3. **[Software Requirements](#software)**
4. **[Deliverables and Methodology](#method)**
5. **[How to Use?](#use)**



<h2 id="understanding-git"> Introduction </h2>

We propose a mobile software app Lakshmanrekha for home quarantine management system, which would help authorities (health worker/ police) to monitor the individuals who are required to be in self/ home quarantine. The proposed app is inspired by the face biometric-based continuous user authentication mechanism which actively verifies an individual by collecting biometric data through mobile build-in sensors and lock the device once a change in user identity is detected. In specific, the proposed software tool continuously matches the quarantined location and the location from where the user uploaded the biometric data and if it detects user identity is changed, it will be notified to government officials. In this way, every quarantined user of the app will be fenced within some (say 300 m) radius of his place of quarantine, and local administration can efficiently monitor and detect any geofencing breach.

<h2 id="install-setup">Scope and Utility</h2>
Lakshmanrekha would help Govt. officials to track whether people are following home quarantine or not. Our app verifies the identity of the person and also using the current location of the user to identify whether the user is in their home or not. The authorities are alerted immediately whenever a user breaks their quarantine, thus it serves as an efficient way to keep track of the population. This system can be implemented in every state, and the working supervised locally to ensure proper quarantine is followed. 
With the State of the Art Face recognition Algorithms being used in the backend, our solution is robust and fast. The app also uses the latest android API for its functioning and thus consumes a minimal amount of battery. The app is also supported to run on almost every android device in the market thus the scope of our solution is large.

<h2 id="software">Software Requirements</h2>
The requirements for the app are minimal considering we are targeting the whole of India with this solution. Devices running Android 5.1 and above are supported. The app uses bare minimum system RAM and hence devices having and more than 512MB RAM would be sufficient. Active and continuous internet access is required. Continuous access to the location is also required. The android device should also be having at least 1 front camera.

<h2 id="method">Deliverables and Methodology</h2>
The deliverables consist of two software tools: user mobile application, and server mobile application. User apps will continuously run in the background even when mobile is not in use.

<h3>Server</h3>
The Server used by the app consists of two parts:
<ol>
<li>API made using Flask.</li>
<li>Storage</li>
</ol>
 
The Android App captures these four below-mentioned data and makes a POST request on the API:
<ul>
<li>User-ID</li>
<li>Current Picture Clicked by Camera</li>
<li>Current Location of the User Using GPS</li>
<li>Current System Time</li>
</ul>

The received User-ID helps in retrieving the original image and original location of the user (Collected at the time of registration). Then the following three things are checked, before sending the alert message:
<ol>
<li>Match face detected in the original image and the current image</li>
<li>Compute the distance between the original location and current location</li>
<li>Compute the difference between received time and server’s system time</li>
</ol>

Comparing the Faces present in the original and current image, the face_recognition library, is used. This library is Built using dlib’s state-of-the-art face recognition built with deep learning. The model has an accuracy of 99.38% on the Labeled Faces in the Wild benchmark. The working of The API can be better understood using the flowchart present below.

![image alt ><](https://user-images.githubusercontent.com/43112419/83099062-d7f13000-a0c9-11ea-9881-07d8e8c27e9c.png)

The storage is used to store the images received by the API. The server used for this purpose is python anywhere.

<h3>App</h3>

The app uses the WorkManager class from the Android SDK to run periodic tasks in the background roughly every 15 minutes. The exact time varies according to the system resources available and so as to make optimal use of the memory and battery available. The periodic background task captures a photo of the user in the background without any user interaction, this photo is then sent to the server through the form of a POST request where the photo is matched against the reference image of the user to verify the authenticity of the user. The app also sends the current location of the user along with the photo to the server, so as to determine whether the user has violated their home quarantine. The app asks for all the required permits when opened for the first time, and it makes sure that the service keeps running in the background by explicitly asking the user to run the app on startup, as there are many android manufacturers, the app has considered the following android OS variants, to give the app special permissions.
<ul>
<li>MiUI</li>
<li>LeTV</li>
<li>Huawei</li>
<li>ColorOS</li>
<li>OPPO</li>
<li>IQOO</li>
<li>VIVO</li>
<li>Samsung</li>
<li>HTC</li>
<li>ASUS</li>
<li>Oneplus (Oxygen OS)</li>
</ul>

By making the user allow these special permissions, the app makes sure that it is not killed by the OS.
The user has to upload his reference image first to the database, at the same moment, their current location is also stored which would be treated as the home location, this reference image of the user and the reference location is then matched roughly ever 15 minutes to determine whether the user follows the quarantine or not. Currently, there is a button through which the user can explicitly send a post request to the server, as it may not be feasible to wait every 15 minutes for it to happen in the background. The result obtained from the server is displayed in the form of push notification.

<h2 id="use">How to Use?</h2>
<ol>
<li>After installing the app, open the app, it will ask for a few permissions at the first run, allow all the permissions.</li>
<li>Allow permission for the app to display over other apps. </li>
<li>Keep the app in the protected list so it does not get killed by OS</li>
<li>Upload the reference image for the first time to the database, your current location will also be stored at this instant and would serve as your home location. </li>
<li>After uploading ref image, click on the send post button to capture an image in the background and verify, if the verification is a success then the result is displayed on the screen along with notification.</li>
</ol>


