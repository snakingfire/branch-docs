### Initializing Branch

While we're still on the dashboard, let's come up with a URI scheme for your app. This can be anything you want (so long as it doesn't contain any "special" characters), and is used for opening your app in situations where Universal Links can't.

Once you have a URI scheme to use, enter it on the dashboard, and click save. Once the dashboard has updated, switch to Xcode, and:

  - Click the project file (it's the item at the top of the file list)
  - Select the `Info` tab
  - Expand the `URL Types` section
  - Click the `+` symbol
  - In the `URL Schemes` box, paste the URI Scheme you decided on above

  ![image](/uri_scheme.gif)

### Handling Links

### Deep Link routing
