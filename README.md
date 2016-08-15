PFUser-Digits
=============

A way to authenticate Parse Users using the Twitter Digits API

Make sure you setup your project with [Digits](https://docs.fabric.io/ios/digits/) and with [Parse](https://www.parse.com/docs/ios_guide#top/iOS)

## Installation

PFUser+Digits is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "PFUser+Digits"
```

Note: This pod has `Digits`, `Parse` and `Bolts/Tasks` as dependencies, if you are currently not using cocoapods to install any of these, you may be better off just copying the files to your project.

#Login with Digits

To use just call the function which will trigger the Digits sign-in flow and when succeded, will authenticate or create a new user on Parse.

You may call using blocks: 

```objc
[PFUser loginWithDigitsInBackground:^(PFUser *user, NSError *error) {
    if(!error){
      // do something with user
    }
}];
```
Or Using Bolts:

```objc
[[PFUser loginWithDigitsInBackground] continueWithBlock: ^id (BFTask *task) {
    if(!task.error){
      PFUser *user = task.result;
      // do something with user
    }
}];
```

#Link Existing Account

You can also link an existing account (anonymous or not) with Digits. This works in the same way as linking with Facebook and allows you to later on log back in using the Digits sign-in

```objc
[[PFUser currentUser] linkWithDigitsInBackground]; //returns BFTask*
```
or 
```objc
[[PFUser currentUser] linkWithDigitsInBackground:^(PFUser* user, NSError* error) {}]; //block callback
```

#Stored properties
After login or link theese digits properties are available for the current user:

```objc 
-(nullable NSString *)digitsId;
-(nullable NSString *)digitsEmail;
-(nullable NSString *)digitsPhone;
```

You may wish to copy the `digitsEmail` to the stored `email` property on PFUser.

Note: to get the email you must specify `DGTAccountFieldsEmail` in the configuration as below for instance)

#Logout
Even if you logout your Parse User the Digits session is maintained separately, so if you would like to logout of Digits together with Parse, make sure to do something like below when logging out:

```objc
[[PFUser logOutInBackground] continueWithSuccessBlock:^(BFTask* task) {
    [Digits sharedInstance] logout];
}];
```

#Customising

For all the previous examples you can pass an `DGTAuthenticationConfiguration` object. Use it to configure the appearance of the login screen or pass in the phone number to verify.
For more information view the [Official documentation](https://docs.fabric.io/ios/digits/theming.html)
For example:

```objc
DGTAppearance *appearance = [DGTAppearance new];
appearance.backgroundColor = [UIColor whiteColor];
appearance.accentColor = [UIColor defaultLightBlueColor];
appearance.logoImage = [UIImage imageNamed:@"app_icon"];

DGTAuthenticationConfiguration *configuration = [[DGTAuthenticationConfiguration alloc] initWithAccountFields:DGTAccountFieldsEmail];
configuration.appearance = appearance;
configuration.phoneNumber = [User currentUser].phone;
configuration.title = NSLocalizedString(@"phone_login_title", nil);
[PFUser loginWithDigitsInBackgroundWithConfiguration:configuration];
```


#Old Version
If you are looking for the old version of this repo, which required you to add extra cloud-code as well, it is still available under this branch: [old-version](https://github.com/felix-dumit/PFUser-Digits/tree/old-version)


#License
The MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
