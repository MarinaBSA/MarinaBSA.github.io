---
layout: post
title:  "How to get rid of the storyboard"
date:   2020-07-26 17:23:00 +0200
categories: swift storyboard
---
> Level: Beginner <br/>
> Framework: UIKit <br/>
> Swift: 5.0 <br/>
> Xcode: 11.3.1 <br/>
> iOS: 13.2 <br/>

## The Motivation 

As a beginner in iOS Development learning UIKit, one of the things that get to my nerves is to use the storyboard. It seems like it has its own will, it just does what it wants every now and then.  
The whole drop and drag of stuff and opening multiple windows to connect your buttons to your code is just not my cup of tea. So I started to look for alternatives.  
Where can I find information on how to get rid of the  storyboard and do everything programmatically? And guess what, that was hard to find. Most tutorials and books operate using the storyboard. <br/> But I got you, no worries. 

[I can't believe you are still talking, show me the code.](#code)
#### What are we going to do?
The most simple app with a cyan background color.  
Its main purpose is to get you up and running without the storyboard.

#### What do I need to know?
I am assuming you know the basics of programming, the Swift language, and how to create and run an Xcode Project.

### Let's go

* Create a new Xcode project
	1. Choose `Single View App`
	2. Call it whatever you want
	3. Choose `Swift` as its Language and `Storyboard` in User Interface
	4. Choose the location you want  
	5. Run the app to see if everything is ok (optional)<br/><br/>

* Delete the storyboard and its references
	1. Delete the file `Main.storyboard`, I usually click "Remove Reference".
	2. In the `General` Page under the section `Deployment Info` delete 'Main' <br/>
	from `Main Interface` and tap enter afterward.
	![Deleting Main from Main Interface](/assets/how_to_get_rid_of_the_storyboard/tapping_app_uikit_delete_storyboard.png)
	3. In the `Info.plist` search for "main" and click on the 'minus' button on the row you found.
	![Deleting Main from Info.plist](/assets/how_to_get_rid_of_the_storyboard/tapping_app_uikit_delete_main_from_info.png)

What happened here?  
The app's main interface was set to be a file called Main that was our storyboard.<br/>
In the Info.plist were some reminiscents of that file so we deleted that too.

* Setup the RootViewController  
In `SceneDelegate.swift` we have to declare which ViewController is going to be the root which basically tells where to look 
for when the app starts.  
Who is this app's root? Who controls this app's main view? That must be the RootViewController. This is where the marathon
starts. And since we deleted the storyboard where we usually had a ViewController with an arrow pointing to it, which means "I am Root". (Could not think of link to 'I am Groot' from Guardians of the Galaxy, I'm sorry I failed you.)  
We need to determine who is root now.

But before we get there. *What is a scene?*  
This discussion is deeper than I want to and can go now. So I'll give you my simple explanation of a scene. *The same app can have different instances of itself open like a browser has tabs. All of those instances are called scenes.*  
[Apple Docs on Scenes](https://developer.apple.com/documentation/uikit/app_and_environment/scenes)


Don't miss the goal.  
Let's determine who our root view controller is in the function `scene` under `SceneDelegate.swift`.  
We have a `window` variable that gives us our current window and we also have a `scene` that is passed as a parameter to the function. Currently, this scene does not have a name.  


{% highlight swift %} guard let _ = (scene as? UIWindowScene) else { return }
 // The underscore means this constant does not have a name.
 // But if it has no name, how can we use it?
 // How to call a dog that does not have a name?
 // All this code does is to check if there is a scene (it is not nil) and 
 // if there isn't one then return right now from this function.
 // Because something must be utterly wrong, so don't continue.
 // It is a guard, which is guarding or "keeping an eye" on our scene 
 // but it does not keep it somewhere so we can use it. Let's change that.
{% endhighlight %}


{% highlight swift %} // Now we can access our scene through the constant named windowScene.
 guard let windowScene = (scene as? UIWindowScene) else { return }
{% endhighlight %}

{% highlight swift %} guard let windowScene = (scene as? UIWindowScene) else { return }
 // We don't have a window. 
 // We just have a variable declared to hold one.
 // Let's initialize our variable window with an instance of UIWindow.
 // If you write 'UIWindow(' and don't close the parenthesis then
 // Xcode on a lucky day will show you everything that can go in there, 
 // which usually helps me out a lot.
 // Choose the option with the argument windowScene 
 // and pass it our windowScene.
 window = UIWindow(windowScene: windowScene) {% endhighlight %}  
<br/>![Deleting Main from Info.plist](/assets/how_to_get_rid_of_the_storyboard/tapping_app_uikit_windowscene.png)

{% highlight swift %} guard let windowScene = (scene as? UIWindowScene) else { return }
 window = UIWindow(windowScene: windowScene)
 // We have a window now which is connected to our scene, remember our goal?
 // So tell our window who the rootViewController is by assigning it to an 
 // instance of the ViewController that came with the project's template
 // inside a file equally named (ViewController.swift).
 window?.rootViewController = ViewController()
 {% endhighlight %}


{% highlight swift %} guard let windowScene = (scene as? UIWindowScene) else { return }
 window = UIWindow(windowScene: windowScene)
 window?.rootViewController = ViewController()
 // Just one more step to go now. We have to make our window visible.
 window?.makeKeyAndVisible(){% endhighlight %}  

Done.    
### <a name="code">Code</a>
{% highlight swift %} func scene(_ scene: UIScene, willConnectTo session: UISceneSession, 
   options connectionOptions: UIScene.ConnectionOptions) {
   guard let windowScene = (scene as? UIWindowScene) else { return }
   window = UIWindow(windowScene: windowScene)
   window?.rootViewController = ViewController()
   window?.makeKeyAndVisible()
 }{% endhighlight %}


Now let's go change the background color of our rootViewController so we can see if we actually made it work.
We set out rootViewController to be an instance of the ViewController class, which is inside a file also called ViewController. So we must go there to change its background.
* Change the background color of our view
	1. Go to the file ViewController.swift

{% highlight swift %} override func viewDidLoad() {
   super.viewDidLoad()
   // Every ViewController has access to its view. 
   // This is one of the things a view controller does.
   // It manages the view.
   // And a view is mainly about looks. 
   // So the background color belongs to the view.
   view.backgroundColor = .cyan
   // The backgroundColor takes an UIColor. The class UIColor has
   // some predefined color variables we can use.
   // Since backgroundColor takes an UIColor, we can get away with '.cyan'
   // instead of 'UIColor.cyan'
 }{% endhighlight %}
----
### Finish

Congrats, you are done.
Run the app and if should see a cyan background color.

Now what?  
Soon, I'll show you how to change this app's background color by tapping on the screen.

[Code to this app on GitHub](https://github.com/MarinaBSA/blogApps/tree/master/tapping_app/tapping_app_uikit)

