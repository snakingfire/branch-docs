### Installing Cocoapods

The quickest way to get the Branch SDK is by using the dependency manager Cocoapods. However, before you can use Cocoapods to get the Branch SDK, you must first install Cocoapods itself. To do this:

- Open a terminal window
- Paste in the command: `sudo gem install cocoapods`

Once the gem has finished installing:

- Type `cd ` into the terminal window
- Drag and drop the `BuildAnAppAB` folder into the terminal window
- Press enter:

  ![image](/cd_gif.gif)

Once you have a terminal window in the same location as your app's code, we can install the Branch SDK.

### Adding the Branch Pod

To add the Branch SDK to your Xcode project, we need to initialize cocoapods. To do this, run `pod init` from the terminal window we set up previously. This will create a new file in your project folder called, descriptively, `Podfile`:

  ![image](/pod_init.gif)

This file contains all of the other code libraries, like the Branch SDK, that our project will use. To add Branch to this list of libraries, open the Podfile in a text editor, and add `pod 'Branch'` below `#Pods for ...`

  ![image](/pod_config.gif)

Once the Branch pod as been declared, close your text editor, and run `pod install` to install the SDK files, and generate a new Xcode workspace:

  ![image](/pod_install.gif)

### Using the new project

Because of the way that Xcode and Cocoapods interact to handle dependencies like the Branch SDK, you won't be able to continue using the `projectName.xcproject` to work on your app. Instead, you need to use the `projectName.xcworkspace` file the was generated in the above steps. To switch, simply quit Xcode and double click the `.xcworkspace` file.
