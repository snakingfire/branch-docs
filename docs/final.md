
Congratulations! You've made it almost all the way through this guide. At this point, you should have:

1. Set up Cocoapods on your computer, and installed the Branch SDK
1. Created a Branch app, and linked it with your Xcode project
1. Configured Universal Links and a URI Scheme
1. Set up deep link routing to take users to an animal

The only thing left to do is test the integration out. Open your app, select an animal, click share, and send the link to iMessages. When you close the app and click the link, you should be taken directly the animal you originally viewed.

<!-- Go back to your Branch dashboard, and [create a new link](https://dashboard.branch.io/quick-links/qlc/config/). Name it anything you like, and then:

1. Click `Configure Options`
1. Select `+ More Data`
1. Set the `Key` to `animal`
1. Update the `Value`to either `cat` or `dog`
1. Click `Create now`

![image](/create_link.gif) -->

## Next steps

Now that you have a Branch integrated app, you can start expanding the list of animals that your app supports, or even switch to showing something completely different. However, with Branch integrated, you can easily route users, regardless of why type of app you create. Start thinking of a creative way you can make use of Branch, or [visit the docs](http://dev.branch.io/) to try out some of the more advanced things you can do with Branch.
