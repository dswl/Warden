## Inspiration
Artificial intelligence, machine learning, and computer vision have always been large topics of interest for me, so when I was brainstorming possible projects for Bitcamp this year, I recalled an idea I've always been interested in but never put to the test. 

With increasing tensions across the globe, feelings of unease and anxiety loom large. While large corporations and governments will surely have the best security systems money can buy, most are left in the dust. Even simple security systems with the most basic of cameras and alarms can quickly run up hundreds of dollars. The need for a security system that has a low cost of installation, low cost of maintenance, and low cost of energy has never been higher.

While a security system is generally not what comes to mind when most think of putting cutting-edge technology to the test, there is much more to be uncovered. 

## What it does
My project revolves around the usage of cameras to detect and identify faces. It periodically takes images and detects faces in them. When a new face is detected, a new directory is made and the image, along with its landmarks, is saved. By the time the program is done running, it will have a folder for every unique face, along with every picture that was taken of that face inside the folder. This allows those who value their privacy and security to have a system that protects them without spending large amounts of money.

## How I built it
I built Warden using Google Colab to host and execute my code.
I used Python for most of the computation and used a bit of JS to snag pictures from the browser.
The machine learning model I used was from Google's mediapipe, and allowed me to detect facial landmarks.
For numerical computation, I used scipy, math, numpy, and opencv.
Given the set of landmarks on a face, here is my general process for developing each face's unique matrix ID:
1. Select landmarks most representative of one's face
2. Grab x and y coordinates of each landmark, convert to np array
3. Calculate center of gravity and normalize to origin (some points will in in negative quadrants, now it doesn't matter where you are on the camera, it will detect you as the same)
3. Calculate the scale between two of the landmarks (choose two that are farther apart, maybe left cheekbone and right cheekbone) and normalize to scale (proportions will be preserved because we have normalized to the origin and we scale each point the same, and now no matter how far or close you are to the camera, you will be detected as the same)
4. Now we will calculate what I named Ax and Ay. We will take the x of every landmark and multiply it with every other x to create the Ax matrix. Do the same for Ay (with y coordinates instead)
(For example, if we detected 3 landmarks whose x-values ended up to be [1 2 3], then our matrix would be [[1 2 3] [2 4 6] [3 6 9]])
5. To see if these A matrices correspond to the same face, we want to calculate the Frobenius normalization of the difference between the two Ax matrices and add it to the Frobenius normalization of the difference between the two Ay matrices. If this value is under a set threshold, it is the same face. Otherwise, it is not.

## Challenges I ran into
One of the largest challenges I had was just getting the webcam to work with Colab. Colab has some resources for snagging webcam images, but they were not great, and resorted to saving the image first, which was very inefficient for my means.
Another issue was false negatives, which I was able to solve using normalization functions.
There were also tons of minor bugs and tweaking I had to do every time I made a change that would impact what my thresholds would be. 

## Accomplishments that I'm proud of
I'm proud of creating a project that works and is able to identify faces. Of course, there is lots of room for improvement, but I'm glad I was able to identify faces, group them by their owners, and have a decently low error rate.
I am also happy that I was able to attend many of the workshops that taught me things like ML.

## What we learned
I learned a lot about using mediapipe, as it was my first time actually using the software (I actually learned about it from one of the sign-language detection workshops). I am very glad I was able to use a powerful API without paying (lol). I also learned a lot about ML and training from workshops. I learned how to identify who's face was who's. 

## What's next for Facial Recognition Security System
In the future, this technology could be adopted into systems like security cameras or check-in cameras. There are better methods for normalization and identification, so implementing those would be a good next step as well. 

Feel free to let me know how well the code works for you, in my experience moving the camera or rapidly moving your face drastically decreases it's efficiency, but that's to be expected: there's only so much that can be done with one camera.
I do have a few ideas about how to actually detect depth with one camera: such as employing a slight wiggle function or flashing white light at the user and reading brightness levels. However, the best and most efficient facial recognition algorithms rely on multiple cameras or a (usually IR) depth sensor.
