
# cocos2d-win32-window-close

Added win32 close button callback which is absent from the original engine.




##
In the original cocos2d-x engine there is no proper way to handle a onWindowCloseButtonPressed event on win32 platform.

On Pressing the Close button, the cocos2d application abruptly terminates without any callback events.

A onWindowCloseButtonPressed event is useful in case such as "asking if we want to quit" or "saving game data". 

We can use the AppDelegate destructor for the application terminate event, but important objects such as Director, UserDefault, etc. 
are already deleted and are not accessible at this point. 

In short, we need an event such as onWindowCloseButtonPressed which will trigger when the close button is pressed on the application window so we can put
our own application logic in the callback and gracefully close the app.

## Steps

The following trick does the job without changing much of the engine code.

#### The following files need to be changed:
- Classes/AppDelegate.h
- Classes/AppDelegate.cpp
- proj.win32/main.cpp	

#	  
	  
In AppDelegate.h add 2 new functions to the AppDelegate class declaration:
  - int run1();//We are creating a duplicate run function
  - void applicationShouldClose();//The callback function
(Function names can be whatever)
	  
In AppDelegate.cpp implement both functions:
  - applicationShouldClose() //Will contain our own close callback logic.
  - In run1() function paste the code from AppDelegate.cpp file in this repo;
  - There is also a "static void PVRFrameEnableControlWindow(bool)" function which is to be copied from AppDelegate.cpp file in this repo.
										                      
In main.cpp replace:  
"return Application::getInstance()->run();" line with  
"app.run1();"
					
#

Now if we press the window close button, AppDelegate::applicationShouldClose() function will be executed instead of closing the application.

To close the application we need to call Director::getInstance()->end(); explicitly.
