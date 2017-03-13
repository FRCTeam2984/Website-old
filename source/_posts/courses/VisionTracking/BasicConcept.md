;;;
 "title" : "",
 "layout" : "page",
 "navigation" : {
   "next": "/first/courses/VisionTracking/GameApplication/",
   "previous": false,
   "home": "/first/courses/VisionTracking/"
 }
;;;

Conceptual Tracking
===
To start out let's lay out the bits of information that we have to start with. We know the following:
 - The dimensions of the target
 - The field of vision of the camera
 - The target is retro reflective

The first thing we will want to do is acquire a useful image. In this case we will illuminate the retro reflective tape to create two very bright targets. This is the image which the camera will see.

![Full color image of vision target](/first/images/courses/VisionTracking/target.png)
Now to find only the vision targets we first must filter out the green targets, this can be done in a variety of ways but by far the easiest one is applying a threshold over the image. Applying a threshold consists of selecting a range for each of the image channels, then if a pixel falls within the values it gets set to white, when not it gets set to black. In particular this is called a binary threshold. We want to filter out the retro reflective tape which is very bright, therefor we will want to covert the image to hsv to facilitate the creation of a threshold.

![Hue, Saturation, and Value channels of the target](/first/images/courses/VisionTracking/targetHSV.png)
As we can see from the three images the tape has a very distinct hue and value but the saturation varies greatly. As a result it is of advantage to us to use only the hue and value channels in our threshold rather than trying to include saturation. When we apply the threshold to the image we get the following image which is either black or white. This is called a binary image.

![Hue, Saturation, and Value channels of the target](/first/images/courses/VisionTracking/targetBinary.png)
Now with the binary image we can find the contours in the image and determine their height and width.
