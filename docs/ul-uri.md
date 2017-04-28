### URI Schemes

While we're still on the dashboard, let's come up with a URI scheme for your app. This can be anything you want (so long as it doesn't contain any "special" characters), and is used for opening your app in situations where Universal Links can't. For example, a URI scheme might look something like this:
```
super-cool-app://
```

Once you have a URI scheme to use, got to the [link settings page](http://dashboard.branch.io/link-settings) of your dashboard, enter the URI scheme you've devised (in the `iOS URI Scheme` box), and click save. Once the dashboard has updated, switch to Xcode, and:

  1. Click the project file (it's the item at the top of the file list)
  1. Select the `Info` tab
  1. Expand the `URL Types` section
  1. Click the `+` symbol
  1. In the `URL Schemes` box, paste the URI Scheme you decided on above

  > Note: Xcode only needs to portion of your URI scheme before the `://`, so only paste in the text part: your scheme won't work if you add the whole scheme.


  ![image](/uri_scheme.gif)

### Universal Links

#### Dashboard setup

Universal Links are crucial for opening your app, so getting them set up is one of the most important parts of integrating with Branch.

To configure Universal Links:

1. Go to the [`Certificates, Identifiers & Profiles`](https://developer.apple.com/account/ios/identifier/bundle) page of Apple's Developer website.
1. Search for the `Bundle Identifier` that you set for your project

> You can find this by going to Xcode, and select the blue project file with your apps name. The Bundle Id will be the second entry on the screen

3. Expand the entry, and copy the `Prefix` value.

> If you don't see your Bundle Id listed, it may not have registered yet. To give it a kick, and speed things up:
> 1. Open Xcode, and select your project file
> 2. Select the "Capabilities" tab
> 3. Scroll to the bottom and turn "Associated Domains" to "on"
> If you go back to the [developer website](https://developer.apple.com/account/ios/identifier/bundle), you should now see your Bundle Id listed

With the `Prefix` for your app, head over to the [link settings](https://dashboard.branch.io/settings/link) page of your dashboard. Here:

1. Set the `Apple App Prefix` to the value you just copied,
1. Update the `Bundle Identifier` field to the id you set for your project.

>Don't forget to click save!

  ![image](/dash_ul.gif)

#### Xcode setup

Once you've updated your dashboard settings, you need to switch back to Xcode to configure the `Entitlements` with your Branch app. Before switch, though, make sure to note down the Branch `<something>.app.link` domain that your Branch app is using. In Xcode:

1. Click your project file (top of the list)
1. Select the `Capabilities` tab
1. Expand the `Associated Domains` section

Set the `Associated Domains` to `on`, and add two entries:

 1. `applinks:something.app.link`
 1. `applinks:something-alternate.app.link`

 replacing `something` with your Branch domain.

  ![image](/xcode_ul.gif)

One last thing to note is that iOS doesn't update your app's Universal Link settings automatically, so before moving on, make sure to uninstall the app from your device, and re-install it with the new changes.

### Testing break

At this point, your Branch links should now be able to open your app. Give it a shot by [creating a new link](https://dashboard.branch.io/quick-links/qlc/config/), and clicking it from the native iMessage app. You should see your app immediately launched to its home screen.

### Troubleshooting

Did your link not open your app? As cliché as it sounds, try uninstalling the app and then turning your phone off and on again. If you continue to run into issues, double-check all of the values you added in the above steps to verify that they are correct. Still no luck? Then it is time to reach [reach out for help](slack://channel?team=T02BUTP4H&id=C1U7KNG0K).
