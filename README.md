# CAMIL-Coordinates-Alignment-for-Mapping-Image-Lines
CAMIL (Coordinates Alignment for Mapping Image Lines) is an algorithm that extracts the 2D coordinates of open curves and lines present in an image.

This algorithm outperforms current pixel ordering/interpolation methods that do not perform well if there are free (that do not have a particular horizontal or vertical orientation) curves in the images.  

### Plot the comparisons of interpolation methods results
(from worst to best)
* 🔴 **Spline applied directly on white pixels retrieved from the binary image**:
  * ❌ the interpolation may fail since the pixels are ordered form the top left corner to the bottm right corner of the image, so the curve is restricted to have that direction (***top/left->bottom/right***).
  * ✅ it's the simplest way
* 🟡 **Spline applied with pixels extracted via open(half) contour**:
  * ❌ the interpolation may fail to capture the starting and ending point of the curve (since the order is based on an internal criterium of the cv2 findContours function). The criterium seems to take as starting point always the topmost point of the found contour, therefore the curve to map must satisfy ***top->down*** direction.
  * ✅ avoids back and forth lines that can be created with the previous method due to left/right, top/bottom simmetries in the curve
* 🟢 **CAMIL algorithm**:
  * ❌ the possible errors may be in start/end point position's precision, but that may be adjusted with the CAMIL's parameters according to your needs.
  * ✅ avoids many of the common errors of the previous two methods
  * ✅ it maps multiple separate curves (or it connects near ones based on yuor parameters selection)
  * ✅ a curve can follow ***any direction/orientation*** in the image plane.  
  * There's also last-resort methods (in case something fails) in the usual interpolation method, one that starts from open contour order (but is not guaranteed to works every time, indeed it's a last-resort) and one which is based on the pixel ordered as an image. They are used only if they don't have any issues. To look at the contour-camil method results put the `plot_camil_cont` flag to `True` when calling the plotting code.


In general the curves that will result in a non accurate interpolations have one of these issues:
* close loops
* long length (you may want to try to increase the number of iterations, it will slow the algotihm though)
* too many bumps, articulations or the line passes near other sections of the curve
* too much pixellated curve image
* curve has sparse points/it's not continous in the image (try to increase the max_distance, but precision may worsen) 

![image](https://github.com/MartinMolinaro/CAMIL-Coordinates-Alignment-for-Mapping-Image-Lines/assets/108731826/cee53673-0ae2-4fea-ad75-d8c4cc9d9a11)
![image](https://github.com/MartinMolinaro/CAMIL-Coordinates-Alignment-for-Mapping-Image-Lines/assets/108731826/58cc904b-ae8b-4e5a-b71b-c49b06edf924)
![image](https://github.com/MartinMolinaro/CAMIL-Coordinates-Alignment-for-Mapping-Image-Lines/assets/108731826/9b129994-21a4-4964-afc5-13eafe13f6a4)

