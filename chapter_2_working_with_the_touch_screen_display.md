### Chapter 2: 

#Working with the Touch Screen Display

Now that we've completed our first Android app, let's explore a device feature that has become particularly popular with mobile phones and tablets—multitouch. Virtually all Android devices ship today with a capacitive touch screen panel. It's a device we've gotten so accustomed to that we hardly "see" it as the hardware sensor that it is.

User interaction (UI) with Android touch screens differs somewhat from that of a mouse on a traditional computer display. First of all, we don't have one omnipresent mouse pointer for interacting with UI elements via rollovers, clicks, right-clicks, and double-clicks. In fact, we don't have a rollover or a physical "click" on the touch screen panel at all, hence UI interactions often require adjustments for the touch screen. Typically the Android device uses audiovisual cues such as click sounds or small device vibrations for user feedback.

There are a number of advantages to the multitouch screen interface to point out. First and foremost, the capacitive touch screen panel affords us more than one mouse pointer. We can work with two, five, even ten fingers on the Android, although more than three are rarely used. [Multitouch][1] allows us a variety of distinct finger gestures compared to the mouse, which we can only use to interact with the UI elements and other components displayed on the screen. The two most common multitouch gestures are the pinch and rotate gestures, typically used for scaling and rotating objects on the screen. 

In this chapter, we'll get started by learning to use the mouse callback methods available in Processing for Android. Then we'll dive into the different color modes Processing has to offer, an essential topic that we need to address to work with graphics and images throughout the book. Building on the basic drawing sketch <!-- linkend="code.basic.drawing" -->, we'll use the mouse speed to manipulate the hues of the ellipses we draw.

Finally, we'll dedicate the second part of the chapter to the multitouch features of the Android touch screen and create a sketch that showcases the most common gestures, including the tap, double-tap, long press, flick, pinch, and rotate gestures. In the sketch we'll develop, we'll manipulate the scale, position, rotation, and color of a rectangle using multitouch gestures.

To make working with multitouch gestures easy, we'll use the [Ketai library for Processing,][2] which greatly simplifies the process. We'll work with Ketai throughout the book, as it also simplifies working with sensors, cameras, location, and networking—all the hardware features that are typically difficult to work with. We'll download and install the library step by step and take a quick look at the main Ketai classes.

Let's take a look at how the touch screen panel works.

[1]: http://en.wikipedia.org/wiki/Multi-touch
[2]: http://ketai.org

###Introducing the Android Touch Screen

The capacitive touch screen panel of an Android device consists of a glass insulator coated with a transparent conductor. When we interact with the touch screen surface, our fingertips act as electrical conductors—not very good ones, but good enough to be detected. A touch on the screen surface distorts the electrostatic field, causing a change in its electric capacitance, which can be located relative to the screen surface. The horizontal and vertical position of the fingertip relative to the screen is then made available to us through the Android OS; it is updated only when we touch or move a fingertip across the screen.

The apps we write in Processing have a flexible screen orientation by default, which means our app switches orientation automatically from portrait to landscape depending on how we are holding the phone or tablet–this is detected by the accelerometer sensor we'll get to know in <!-- linkend="sec.display.accelerometer.values" -->. We can lock the orientation using [Processing's ```orientation()``` method using either the ```PORTRAIT``` or the ```LANDSCAPE``` parameter.][3]

<!-- 
2.1 REVISION
Changed previous link (footnote 3 - old link is dead) from
http://wiki.processing.org/index.php?title=Android#Screen.2C_Orientation.2C_and_the_size.28.29_command to http://android.processing.org/reference/environment/orientation.html
-->

For compatibility, Processing uses the constants ```mouseX``` and ```mouseY``` when it's running in Android mode, corresponding in this case to the position of a user's fingertip relative to the upper left corner of the device touch screen rather than the position of the mouse cursor on a desktop screen. This allows us to use the same code across modes. When using ```mouseX``` in Android mode, we refer to the horizontal position of the fingertip on the touch screen panel, and when we use ```mouseY```, we refer the fingertip's vertical position. Both are measured relative to the [coordinate system's][4] origin in the upper left corner of the touch screen. Moving the finger to the right on the screen will increase ```mouseX``` values; moving the finger down will increase ```mouseY```.

In Android mode, we can also use the following mouse methods, which are available in all Processing modes. The Android touch screen gestures correspond to the following mouse events:

* [```mousePressed()```][5] This callback method is called every time a finger touches the screen panel. It corresponds to a mouse-pressed event on the desktop when the mouse button is pressed down.
* [```mouseReleased()```][6] This callback method is called every time a finger lifts off the touch screen surface, but only if its position has changed since first touching the panel. It corresponds to a mouse-up event on the desktop.
* [```mouseDragged()```][7] This callback method is called every time a new finger position is detected by the touch screen panel compared to the previously detected position. It corresponds to a mouse-dragged event on the desktop when the mouse moves while the button is pressed.

All three methods respond only to one finger's touch. When you use more than one finger on the multitouch surface, the finger that triggers callback events is the first one that touches the screen panel&emdash;the second, third, or more are ignored. If you hold down one finger on the screen surface, add another one on, and remove the first, then the second finger one will now be first in line and take over mouse events. We will work with multiple fingers and multitouch gestures in just a bit in <!--ref linkend="sec.multi.touch" -->.

Let's put the mouse callback methods to the test with a simple sketch that prints the mouse position and events into the Processing console. We'll need ```draw()``` to indicate that this sketch is running and listening to the mouse continuously. Then we add our callback methods and have each print a brief text string indicating which mouse method has been called at what finger position.

Create a new Android sketch by choosing  File  &mapsto;  New  from the Processing menu. If your new sketch window is not yet in Android mode, switch it to Android using the drop-down menu in the upper right corner. Add a few lines of code to the sketch window:

```
void draw()
			{
				// no display output, so nothing to do here
			}
			
			void mousePressed ()
			{
				println("PRESSED x:" + mouseX + " y: " + mouseY);
			}
			
			void mouseReleased ()
			{
				println("RELEASED x:" + mouseX + " y: " + mouseY);
			}
			
			void mouseDragged ()
			{
				println("DRAGGED x:" + mouseX + " y: " + mouseY);
			}
```

Let's go ahead and test the touch screen panel of an Android device.

[3]: http://android.processing.org/reference/environment/orientation.html
[4]: http://processing.org/learning/drawing/
[5]: http://processing.org/reference/mousePressed_.html
[6]: http://processing.org/reference/mouseReleased_.html
[7]: http://processing.org/reference/mouseDragged_.html

###Run the App

With your Android device connected to your desktop via a USB cable, run the sketch on the device by pressing the "Run on Device" button in the sketch window. When the sketch is installed and launched on the device, we don't need to pay attention to the screen output of the touch screen panel, but keep an eye on the Processing console at the bottom of the sketch window.

Hold your device in one hand and get ready to touch the screen surface with the other. Take a look at the console and tap the screen. In the console, you'll see output similar to this:

```
PRESSED x:123 y:214
```

Lift your finger and see what happens. If you see no additional mouse event, don't be surprised. Although we might expect a ```RELEASED``` here, we shouldn't get this event if we just tap the screen and lift the finger. The ```mouseX``` and ```mouseY``` constants always store and maintain the last mouse position. To get a mouse-released event, touch the screen, move your finger a bit, and release. Now you should see something like this:

```
PRESSED x:125 y:208
DRAGGED x:128 y:210
DRAGGED x:130 y:209
RELEASED x:130 y:209
```

Because we touched the screen, we first trigger a ```mousePressed()``` event. By moving the finger slightly while touching the surface, we trigger ```mouseDragged()``` until we stop moving. Finally, we get a ```mouseReleased()``` event because we've updated our position since we pressed or touched the screen.

Now that we can now work with the mouse callback methods, we're ready to take a look at the color support that Processing provides, which is one of its strengths. Knowing how to control color values is a fundamental skill that we'll frequently return to as we work with graphics and images throughout the book. We'll come back to the Android touch screen and its multitouch features later in this chapter. 

###Using Colors

Any geometric primitive we draw on the screen uses a particular ```fill()``` and ```stroke()``` color. If we don't say otherwise, Processing will default to a black stroke and a white fill color. We can use the ```fill()``` and ```stroke()``` methods to change default values, and we can also use grayscale, RGB, HSB, or hexadecimal color in the Android apps we create. The ```background()``` method uses color in the same way, with the exception that it cannot set a value for opacity, formally known as the alpha value. 
