### Creating a new app

In order to start setting up our app to use Branch links, we need to create a Branch app to use with your Xcode project. To do this, head over to the [Branch Dashboard](http://dashboard.branch.io/) and click the app selector in the top left corner:

1. Select `Create new app`
1. Name it something descriptive

  ![image](/dash_app.gif)


### Setting up link redirects

With a new Branch app, we need to specify where your future users will be sent. To do this, head over to the `Link Settings` tab, and:

 1. Select `I have an iOS App`
 1. Enter an iOS redirect URL (eg `http://branch.io`)
 1. Add redirects for Android and Desktop (eg `http://branch.io`)
 1. Click save

  ![image](/dash_redirect.gif)

### Adding the Branch key to Xcode

Finally, before moving forward, we need to add the key of the newly created Branch app to your Xcode project. Head over to the [Account Settings](https://dashboard.branch.io/account-settings/app) page of your dashboard, and copy the `Branch Key`. Switch back to Xcode and:

1. Open the `info.plist` file
1. Hover over the last entry, and click the `+` button
1. Name the new entry `branch_key` and use the drop-down menu on the right to set the type to `dictionary`
1. Use the small carat on the left edge of the row to expand the `branch_key` entry
1. Hover over the `branch_key` entry, and click the `+` symbol again
1. Name the new entry `live`, and set the value to the Branch key you copied earlier.

  ![image](/add_key.gif)
