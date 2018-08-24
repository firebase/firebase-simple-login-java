# Status: Archived
This repository has been archived and is no longer maintained.

![status: inactive](https://img.shields.io/badge/status-inactive-red.svg)

# Firebase Simple Login - Android Client

Firebase Simple Login is a simple, easy-to-use authentication service built on
top of [Firebase Custom Login](https://www.firebase.com/docs/security/custom-login.html),
allowing you to authenticate users without any server code.

Enable authentication via a number of third-party providers, anonymous login, or email / password authentication without having to manually store authentication credentials or run a server.

## Deprecation Warning!

Firebase Simple Login for Android is now a part of the core Firebase Android library. As a result,
this standalone Simple Login client is being deprecated. We encourage everyone to upgrade to the
latest version of the [Firebase Android Client](https://www.firebase.com/docs/android/) to get the
latest and greatest features. If you are still using this deprecated Simple Login client,
[you can find documentation for it here](./docs/v1).

You can read more about this change [on our blog](https://www.firebase.com/blog/2014-10-03-major-updates-to-firebase-user-auth.html) and see the [updated login documentation](https://www.firebase.com/docs/android/guide/user-auth.html)
on our website. The updated documentation includes migration plans, but if you have any other
questions, [please reach out to us](https://firebase.google.com/support/).

## Installation

To install in your application, [download from the Firebase CDN](https://www.firebase.com/docs/downloads.html),
or install as a Maven dependency:

```xml
<dependency>
  <groupId>com.firebase</groupId>
  <artifactId>firebase-simple-login</artifactId>
  <version>[1.4.0,)</version>
</dependency>
```

See [https://www.firebase.com/docs/security/simple-login-java-overview.html](https://www.firebase.com/docs/security/simple-login-java-overview.html)
for complete documentation and API reference.

## Configuration

The Firebase Simple Login Java Client supports email & password, Facebook, Google,
Twitter, and anonymous authentication methods. Before adding to
your application, you'll need to first enable these auth. providers in your app.

To get started, visit the Simple Login tab in Firebase Forge, at
`https://<YOUR-FIREBASE>.firebaseio.com`. There you may enable / disable auth.
providers, setup OAuth credentials, and configure valid OAuth request origins.

## Usage

Start monitoring user authentication state in your application by instantiating
the Firebase Simple Login client with a Firebase reference, and callback.

```java
Firebase ref = new Firebase("https://SampleChat.firebaseIO.com");
SimpleLogin authClient = new SimpleLogin(ref, getApplicationContext());
```

Note that instantiating the Simple Login constructor with a valid Android context
is required in order for the client to seamlessly persist and load sessions across
application restarts.

Once you have instantiated the client library, check the user's authentication
status:

```java
authClient.checkAuthStatus(new SimpleLoginAuthenticatedHandler() {
  public void authenticated(FirebaseSimpleLoginError error, FirebaseSimpleLoginUser user) {
    if (error != null) {
      // Oh no! There was an error performing the check
    } else if (user == null) {
      // No user is logged in
    } else {
      // There is a logged in user
    }
  }
});
```

In addition, you can monitor the user's authentication state with respect to your Firebase by observing events at a special location: .info/authenticated

```java
Firebase authRef = ref.getRoot().child(".info/authenticated");
authRef.addValueEventListener(new ValueEventListener() {
  public void onDataChange(DataSnapshot snap) {
    boolean isAuthenticated = snap.getValue(Boolean.class);
  }
  public void onCancelled() {}
});
```

If the user is logged out, try authenticating using the provider of your choice:

```java
authClient.loginWithEmail("email@example.com", "very secret", new SimpleLoginAuthenticatedHandler() {
  public void authenticated(FirebaseSimpleLoginError error, FirebaseSimpleLoginUser user) {
    if(error != null) {
      // There was an error logging into this account
    }
    else {
      // We are now logged in!
    }
  }
});
```

## Testing / Compiling From Source

Interested in manually debugging from source, or submitting a pull request?
Don't forget to read the [Contribution Guidelines](https://github.com/firebase/firebase-simple-login-java/blob/master/CONTRIBUTING.md).

License
-------
[The MIT License](http://firebase.mit-license.org)

Copyright © 2014 Firebase <opensource@firebase.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
