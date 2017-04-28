### Initializing Branch

Now that we have all of the setup out of the way, we can start using the SDK. The first thing that we need to do is import the Branch SDK, so we can make use of it in your app. To do this, find the `import UIKit` line at the top of your `AppDelegate` file, and add a new line below it:
```swift
import Branch
```

Next, lets initialize a Branch session. To do this, open your AppDelegate file in Xcode, and find the application:didFinishLaunchingWithOptions section of code. On the line above `return true` add:

```swift
let branch = Branch.getInstance()
```

Then, one line below, add the Branch `initSession` call:

```swift
branch?.initSession(launchOptions: launchOptions, andRegisterDeepLinkHandler: {params, error in
    // Check if an error occurred while getting deep link data
    if error == nil {
        // params are the deep linked params associated with the link that the user clicked -> was re-directed to this app
        // params will be empty if no data found
        // ... insert custom logic here ...
        print("params: %@", params.description)
    }
})
```

This code initiates a call to Branch's API to retrieve any deep link data that might exist. Right now, all it's doing is printing the data out to the Xcode console and then continuing startup as normal. To get your links to route users to the correct place in your app, we need to add some custom routing logic.

### Handling Links

Before we start deep linking to all the various parts of your app, we need to make sure that your app is aware of "incoming" links and that it is set up to handle them correctly. To do this, add the following two functions to your `AppDelegate` file:

```swift
// Respond to URI scheme links
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    // pass the url to the handle deep link call
    Branch.getInstance().handleDeepLink(url);

    // do other deep link routing for the Facebook SDK, Pinterest SDK, etc
    return true
}

// Respond to Universal Links
func application(_ application: UIApplication, continue userActivity: NSUserActivity, restorationHandler: @escaping ([Any]?) -> Void) -> Bool {
    // pass the url to the handle deep link call
    Branch.getInstance().continue(userActivity)

    return true
}
```

### Deep Link routing

The last core component of deep linking we need to add to your app is the code that decides where to take the user, depending on the data inside of the Branch link they clicked.

To start off, let's add a variable to the `AppDelegate` to let us track and set the type of animal we will show to the user on startup. Below the line with `var window: UIWindow?`, add a new variable:
```swift
var selectedAnimal: String?
```

Now, go back the the `initSession` call you added earlier, and replace this block of code:
```swift
if error == nil {
    // params are the deep linked params associated with the link that the user clicked -> was re-directed to this app
    // params will be empty if no data found
    // ... insert custom logic here ...
    print("params: %@", params.description)
}
```
with this one:
```swift
if error == nil && params?["animal"] != nil {

    self.selectedAnimal = params?["animal"]! as! String?
    print("Clicked animal link for \(self.selectedAnimal!)!")

    let storyboard = UIStoryboard(name: "Main", bundle: nil)
    let mainController = storyboard.instantiateInitialViewController()! as UIViewController
    self.window!.rootViewController = mainController

    let destination = storyboard.instantiateViewController(withIdentifier: "animalDetails") as! DetailViewController
    destination.selectedAnimal = String(describing: self.selectedAnimal!)

    self.window!.rootViewController?.present(destination, animated: true, completion: nil)

}
```

This is quite a change, so let's go back over it and see what each part is doing.

To start off, the first line:
```swift
if error == nil && params?["+clicked_branch_link"] != nil && params?["animal"] != nil {
```
is checking to make sure that:

1. No errors occurred while we were retrieving the deep link data from Branch
2. The deep link data the was returned has an `animal`

Once we have verified that nothing went wrong, and that we have the data we need to route the user, we set the `selectedAnimal` variable to the `animal` in the deep link data:
```swift
self.selectedAnimal = params?["animal"]! as! String?
```

Now, with an animal to show to the user, we can start preparing the view components that we will show to the user. In order to do this, we need to set up the main view controller, and then get an instance of the DetailViewController we will show to the user:
```swift
let storyboard = UIStoryboard(name: "Main", bundle: nil)
let mainController = storyboard.instantiateInitialViewController()! as UIViewController
self.window!.rootViewController = mainController

let destination = storyboard.instantiateViewController(withIdentifier: "animalDetails") as! DetailViewController
```

Finally, we set the `selectedAnimal` property of our destination view to the `animal` we pulled from the returned deep link data, and present the view to the user.

```swift
destination.selectedAnimal = String(describing: self.selectedAnimal!)

self.window!.rootViewController?.present(destination, animated: true, completion: nil)
```

### Content Sharing

One of the most valuable link distribution channels that you as a developer have access to are your users. To make sure that you fully support this, it's important to add content sharing functionality to your app, to provide an easy way for your users to create and send links. Thankfully, with Branch, content sharing is a very easy process. All you need to do is:

1. Configure the link properties (eg Redirect URL's)
1. Add link data (eg the `animal` property)
1. Show the user the share sheet

To add sharing to your Branch app, open the `MainStoryboad` file, and add a new button, called "Share", somewhere on the DetailView:

1. `Control + Drag` the button into the `DetailViewController`
1. Change the `Connection` type to `action`
1. Name the connection something like `shareButton`

![image](/share_button.gif)

Inside of the `@IBAction func shareButton` code block, add the following:
```swift
let linkProperties: BranchLinkProperties = BranchLinkProperties()
linkProperties.feature = "sharing"

let branchUniversalObject: BranchUniversalObject = BranchUniversalObject(canonicalIdentifier: "item/\(selectedAnimal!)")
branchUniversalObject.addMetadataKey("animal", value: selectedAnimal!)
branchUniversalObject.canonicalIdentifier = selectedAnimal!
branchUniversalObject.title = animalType.text!
branchUniversalObject.contentDescription = "Check out my \(selectedAnimal!)!"

branchUniversalObject.showShareSheet(with: linkProperties,
    andShareText: "Here's my \(selectedAnimal!)!",
    from: self) { (activityType, completed) in
    if (completed) {
        print(String(format: "Completed sharing %@ to %@", self.selectedAnimal!, activityType!))
    } else {
        print("Link sharing cancelled")
    }
}
```

The first thing we are doing here is creating a `BranchLinkProperties` object:
```swift
let linkProperties: BranchLinkProperties = BranchLinkProperties()
linkProperties.feature = "sharing"
```
which stores the "control" information on the link. This is the object used to define analytics information (in this case the feature) as well as the control parameters, like the iOS fallback URL.

The next object we are building is the `BranchUniversalObject`:
```swift
let branchUniversalObject: BranchUniversalObject = BranchUniversalObject(canonicalIdentifier: "item/\(selectedAnimal!)")
branchUniversalObject.addMetadataKey("animal", value: selectedAnimal!)
branchUniversalObject.canonicalIdentifier = selectedAnimal!
branchUniversalObject.title = animalType.text!
branchUniversalObject.contentDescription = "Check out my \(selectedAnimal!)!"
```
This contains social media information, like the link image and content description, as well as custom data, like the `animal`.

The last part of the sharing flow is showing the `share sheet` to the user, so they can select the app they would like to send the link with:
```swift
branchUniversalObject.showShareSheet(with: linkProperties,
    andShareText: "Here's my \(selectedAnimal!)!",
    from: self) { (activityType, completed) in
    if (completed) {
        print(String(format: "Completed sharing %@ to %@", self.selectedAnimal!, activityType!))
    } else {
        print("Link sharing cancelled")
    }
}
```

When the user chooses an app, the message body will automatically be populated with the message (eg "Here's my cat!") with a Branch link appended.
