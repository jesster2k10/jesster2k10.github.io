---
title: Firebase Login/Sign up with Swift 3
date: 2016-11-29 00:00:00 Z
tags:
- login
- sign up
- swift
- swift 3
- ios
- firebase
- authentification
- databse
- parse
- xcode 8
- cocoapods
layout: post
comments: true
description: Learn how to implemement user login/sign up using Firebase on Swift 3
og_image: documentation/sample-image.jpg
---

If you're making any sort of app for iOS, I'm pretty sure at one point you thought making user accounts would be a great idea but; you might've tought it would be too hard. Luckily, [Firebase](https://firebase.google.com) by google makes this a hell load easier. Previously you would've had to have to purchase a server, setup a `mySQL` database, create a PHP file to connect to the database, use something like `AFNetworking` to connect to that server etc.

Now you could do this with literally a few lines of code.

### Prequsites

In order to begin you must of course create a Firebase account. You can do this by going to [here](https://firebase.google.com), it's really straight forward just sign in with your google account.

Once you've made an account, head to the console and create your project. 

{% include image.html path="firebase-tutorial/acc-creation/fir-tutorial1.png" path-detail="firebase-tutorial/acc-creation/fir-tutorial1.png" alt="Creating a project on firebase.google.com" %}

Click the create new project and give it a name. For this tutorial, i'll name it `Sample Project`. Name it whatever you wish, select your reigon and click create.

Follow the steps shown an add your app to the console. Enter your bundle identifier and a nickname for the app. Once you've got that click next and you should see a file named `GoogleService-Info.plist` was downloaded.

{% include image.html path="firebase-tutorial/acc-creation/fir-tutorial1.png" path-detail="firebase-tutorial/acc-creation/fir-tutorial1.png" alt="Creating a project on firebase.google.com" %}

We're done with the console for now, you can close it and open up your xcode project. 

### Installing the Pod

The best way to install Firebase alongside any other cocoa frameworks you may come across while creating you app, would be through [Cocoapods](https://cocoapods.org). Cocoapods is simple just a dependency manager for objective-c and swift. You can read more about it on their website [](https://cocoapods.org).

If you don't already have cocoapods installed, you can check out my other blog post on how to install cocoapods or check out the [installation guide](https://guides.cocoapods.org/using/getting-started.html#getting-started)

Go to the root directry of your project, open terminal and type `cd` and drag your project folder into the terminal window. 

You should see something like this now

{% highlight bash %}
Jesses-iMac:Sample Fir-Login jeffe$ 
{% endhighlight %}

Now you're in your projects directory. 
Run the command `pod init` in that directory. 

{% highlight bash %}
Jesses-iMac:Sample Fir-Login jeffe$ pod init
{% endhighlight %}

A new file called `Podfile` should be in the project directory. Open this file with Xcode. 

> Don't use TextEdit to edit this file as it'll cause compatability issues with the way it creates the "", quite hard to explain but just use xode.

{% highlight bash %}
Jesses-iMac:Sample Fir-Login jeffe$ pod init
Jesses-iMac:Sample Fir-Login jeffe$ open -a Xcode Podfile
{% endhighlight %}

Simply, add the following to 

{% highlight ruby %}
pod 'Firebase'
pod 'Firebase/Auth'
{% endhighlight %}

to your iOS target, which in this case is `target 'Sample Fir-Login' do` below the comment `Pods for Sample Fir-Login`.

You Podfile should now look like this bar a few name changes.

{% highlight ruby %}
# Uncomment the next line to define a global platform for your project
# platform :ios, '9.0'

target 'Sample Fir-Login' do
  # Comment the next line if you're not using Swift and don't want to use dynamic frameworks
  use_frameworks!

  # Pods for Sample Fir-Login
  pod 'Firebase'
  pod 'Firebase/Auth'

end

{% endhighlight %}

Save your Podfile by pressing `⌘S`. Once you've saved it head back to the terminal and enter

{% highlight bash %}
Jesses-iMac:Sample Fir-Login jeffe$ pod install
{% endhighlight %}

Your output should look something like this

{% highlight bash %}

Analyzing dependencies
Downloading dependencies
Installing Firebase (3.7.1)
Installing FirebaseAnalytics (3.4.4)
Installing FirebaseAuth (3.0.5)
Installing FirebaseCore (3.4.3)
Installing FirebaseInstanceID (1.0.8)
Installing GoogleInterchangeUtilities (1.2.2)
Installing GoogleNetworkingUtilities (1.2.2)
Installing GoogleSymbolUtilities (1.1.2)
Installing GoogleUtilities (1.3.2)
Generating Pods project
Integrating client project

[!] Please close any current Xcode sessions and use `Sample Fir-Login.xcworkspace` for this project from now on.

Sending stats
Pod installation complete! There are 2 dependencies from the Podfile and 9 total pods installed.

{% endhighlight %}

If you see something like that, you've installed the Firebase framework successfully and it's time to move on. 

> You can no longer use the `.xcodeproject` file as you'll receive errors if you do so. You must use the newly created `.xcworkspace` to open your project from now on.

### Integrating Firebase

Integrating Firebase is quite easy. Open up your `AppDelegate.swift` and `import Firebase` and `import FirebaseAuth` where all the other import statements are. Build your project and it's time to initalise firebase.

{% include image.html path="firebase-tutorial/acc-creation/fir-tutorial2.png" path-detail="firebase-tutorial/acc-creation/fir-tutorial2.png" alt="Copying firebase" %}

Copy the `GoogleService-Info.plist` file into your project directory. Remeber to select copy files if needed to ensure the file's in the directory and select the iOS target. Press finish and you should see the file amongs your other project files.

Now everything is in place, in your `AppDelegate.swift` file in the `application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool` function, add the following code:

{% highlight swift %}
FIRApp.configure()
{% endhighlight %}

Build your project again and it's time to add the authentification code.

### Final Steps
Open up the swift file where you want to add the authentification code, for me it's `ViewController.swift` for you it may be `LoginViewController.swift` or `SignUpViewController.swift', regardless the name; it'll still work.

The way my sample project is structured, when the user clicks the `Login/Sign up` `UIButton`, the function `loginSignUp()` is called and we preform the authentification here. 

There's a UISwitch that choses whether the user want's to preform a login/sign up and we can rename the cutton accordingly.

At the top of our class we need to add yet again `import Firebase` and `import FirebaseAuth`. 

{% include image.html path="firebase-tutorial/acc-creation/fir-tutorial3.png" path-detail="firebase-tutorial/acc-creation/fir-tutorial3.png" alt="Sample project" %}

As you can see, in the Sample Project, there's the two textfields, 3 labels and 3 buttons. Their functions are self explanitory. 

The textfields are where the user inputs their details, the labels on top of the textfields explain their function, the label at the top, tells us whether we're logging in or signing up. 

The two buttons allow us to chose if we're logging in or signing up and finally, the continue button logs us in.

We need to link these UI emelents to our class via an IBOutlet. Once we've done that, our variables in our class now are:

{% highlight swift %}
var isLogin:Bool! = false
    
@IBOutlet weak var wereGoingToLogin: UILabel!
@IBOutlet weak var emailField: UITextField!
@IBOutlet weak var passwordField: UITextField!

{% endhighlight %}

I've also added two new functions. These are only relevant to the sample project and may be irrelevant to you.

{% highlight swift %}

@IBAction func login(_ sender: Any) {
        isLogin = true
        wereGoingToLogin.text = "We're going to be logging in"
}
    
@IBAction func signUpBtn(_ sender: Any) {
        isLogin = false
        wereGoingToLogin.text = "We're going to be signing up"
}

{% endhighlight %}

These functions are called when the two top buttons are touched

Now, we have to deal with the continue button. I have created a function called loginSignUp to handle the firebase logging in or signing up, you could copy this code into your own project and just get rid of the `isLogin` variable.

So were gonna add the following code:

{% highlight swift %}
func loginSignUp () {
        
        //0
        if isLogin == false {
            //1
            FIRAuth.auth()?.createUser(withEmail: emailField.text!, password: passwordField.text!, completion: { (user, error) in
                //2
                if error == nil {
                    print("Successfull created user with email" + user!.email!)
                }
                    
                    //3
                else {
                    print("An error occoured" + (error?.localizedDescription)!)
                }
            })
        } 
           
        //4
        else if isLogin == true {
            //5
            FIRAuth.auth()?.signIn(withEmail: emailField.text!, password: passwordField.text!, completion: { (user, error) in
                
                if error == nil {
                    print("Successfull loggedin user with email" + user!.email!)
                }
                    
                
                else {
                    print("An error occoured" + (error?.localizedDescription)!)
                }
            })
        }
        
    }

 //6
 @IBAction func continueAc(_ sender: Any) {
     loginSignUp()
 }
{% endhighlight %}

**0** This just checks if the user is logging in or signing up with a simple boolean
**1** This is the code as shown in the Firebase docs we use to create a user. We're passing the values from the textfields.

**2** This just checks if there was no errors and prints a nice message in the log telling us that we've sucessfully created the user. The user variable gives us access to things such as their email, user id etc. This can also be accesed by `FIRAuth.auth()?.currentUser()`

**3** Same thing as part 2, checks if there was an error. If there was it prints it to the console.

**4** This just checks if the user is logging in or signing up with a simple boolean
**5** This is the code as shown in the Firebase docs we use to log in a user. We're passing the values from the textfields.

**6** This is the action linked to the continue button in our storyboard.

Now, if you run your project you should see everything is working. 

{% include image.html path="firebase-tutorial/acc-creation/fir-tutorial4.png" path-detail="firebase-tutorial/acc-creation/fir-tutorial4.png" alt="Sample project" %}

{% include image.html path="firebase-tutorial/acc-creation/fir-tutorial5.png" path-detail="firebase-tutorial/acc-creation/fir-tutorial4.png" alt="Sample project" %}

A new user will appear in the firebase console with the information that you've provided.

{% include image.html path="firebase-tutorial/acc-creation/fir-tutorial7.png" path-detail="firebase-tutorial/acc-creation/fir-tutorial5.png" alt="Sample project" %}

You should also see some stuff in the log

{% include image.html path="firebase-tutorial/acc-creation/fir-tutorial6.png" path-detail="firebase-tutorial/acc-creation/fir-tutorial6.png" alt="Sample project" %}

### Finishing off

That's how you can sucessfully authenticate a user using Firebase on iOS with Swift 3. It's quite easy as you can see. No that you can create and login a user, mabye it's time to expand. In a future tutorial i'll be showing you how to use Firebase database to store information about your users.

That's it for today. If you enjoyed that, a small paypal donation to [jesster2k10@gmail.com](mailto:jesster2k10@gmail.com) would be much appreciated. 

You can download the sample project used in the tutorial from [GitHub](https://github.com/jesster2k10/Firebase-Sample-authentification). 




<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = '//jesseonolememen.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                                