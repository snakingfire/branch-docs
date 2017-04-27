### URI Schemes

While we're still on the dashboard, let's come up with a URI scheme for your app. This can be anything you want (so long as it doesn't contain any "special" characters), and is used for opening your app in situations where Universal Links can't.

Once you have a URI scheme to use, enter it on the dashboard, and click save. Once the dashboard has updated, switch to Xcode, and:

  - Click the project file (it's the item at the top of the file list)
  - Select the `Info` tab
  - Expand the `URL Types` section
  - Click the `+` symbol
  - In the `URL Schemes` box, paste the URI Scheme you decided on above

  ![image](/uri_scheme.gif)

### Universal Links

#### Dashboard setup

Universal Links are crucial for opening your app, so getting them set up is one of the most important parts of integrating with Branch.

To configure Universal Links, first go to the [`Certificates, Identifiers & Profiles`](https://developer.apple.com/account/ios/identifier/bundle) page of Apple's Developer website. Once on the website, search for the `Bundle Identifier` that you set for your project, in Xcode. Once you've found it, expand the entry, and copy the `Prefix` value.

With the `Prefix` for your app, head over to the [link settings](https://dashboard.branch.io/settings/link) page of your dashboard. Here, set the `Apple App Prefix` to the value you just copied, and update the  `Bundle Identifier` field to the id you set for your project. Don't forget to click save!

  ![image](/dash_ul.gif)

#### Xcode setup

Once you've updated your dashboard settings, you need to switch back to Xcode to configure the `Entitlements` with your Branch app. Before switch, though, make sure to note down the Branch `<something>.app.link` domain that your Branch app is using.

In Xcode, click your project file (top of the list) and select the `Capabilities` tab. Expand the `Associated Domains` section, turn it `on`, and add two entries:

 - `applinks:something.app.link`
 - `applinks:something-alternate.app.link`

 replacing `something` with your Branch domain.

  ![image](/xcode_ul.gif)
