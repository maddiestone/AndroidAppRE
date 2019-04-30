#### Application Fundamentals
## Components
Components are basic building blocks of Android. Each of these components are declared in the Manifest along with permissions.
Android framework provides 4 basic components to build an app:
### Activities
Essentially UI elements. (This includes Fragments and other UI stuff too)

### Services
Tasks that run without a UI element.

### Broadcast Receivers
A component that listens to System Events (A good example is something listening for  `android.provider.Telephony.SMS_RECEIVED` event)

### ContentProviders
Basically an interface to work on SQLLite DB.

### Intents
`Intent (and Intent Filters)` is Android's RPC mechanism and primary way for each component to talk to each othe
