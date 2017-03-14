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

![The binary image of the target](/first/images/courses/VisionTracking/targetBinary.png)
Now with the binary image we can find the contours in the image and determine their height and width. We do this by finding the bounding rectangles of the two areas of white in the image. We can get away with rectangles because the real life shapes are rectangles and our algorithm does not rely on their accuracy being high.

![The vision targets with bounding rectangles drawn](/first/images/courses/VisionTracking/targetMinAreaRectLabled.png)
Now that we know the location and sizes of the two rectangles we can proceed to find the distance of the target and the angle of the target with respect to the camera. That is the angle perceived by the camera not the yaw of the peg.

![A camera looking at a vision target at distance d](/first/images/courses/VisionTracking/cameraDistance.svg)
To calculate the distance d in the figure above we simply have to relate the two sides of the right triangle with the angle θ. To do this we can use the tangent function, solving for the distance yields <span class="nobr">**d = h / tan(θ)**</span>. However we still have to calculate theta as we are dealing with pixels not with angles. To do this we can set up a proportion between the number of vertical pixels of the camera and its field of vision. So <span class="nobr">**θ = T<sub>h</sub> / C<sub>h</sub> * C<sub>vFOV</sub>**</span> where θ is the angle T<sub>h</sub> is the height of the target in pixels, C<sub>h</sub> is the vertical camera resolution and C<sub>vFOV</sub> is the
vertical field of vision of the camera in degrees. It can be better to determine the FOV empirically rather than going off of a specification. This same process can be applied to calculating the rotation with respect to the camera.

![A camera looking at a vision target at angle θ](/first/images/courses/VisionTracking/cameraRotation.svg)
To calculate the rotation of the target relative to the camera as in the image above a simple proportion can be used. <span class="nobr">**θ = (2T<sub>x</sub>-C<sub>w</sub>) / C<sub>w</sub> * C<sub>hFOV</sub>/2**</span> The equation is derived from getting a percent to either the left or right and then multiplying it by half of the horizontal field of view. T<sub>x</sub> is the targets x location in pixels C<sub>w</sub> is the camera width in pixels and C<sub>hFOV</sub> is the camera horizontal FOV.

Congratulations! Now we have calculated the distance and the angle of the vision targets from only an image given by the camera.
