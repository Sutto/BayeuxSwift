# FayeSwift

[![Join the chat at https://gitter.im/hamin/FayeSwift](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/hamin/FayeSwift?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
A simple Swift client library for the [Faye](http://faye.jcoglan.com/) publish-subscribe messaging server. FayeObjC is implemented atop the [Starscream](https://github.com/daltoniam/starscream) Swift web socket library and will work on both Mac (pending Xcode 6 Swift update) and iPhone projects.

It was heavily inspired by the Objective-C client found here: [FayeObjc](https://github.com/pcrawfor/FayeObjC)

## Example

### Installation


### Initializing Client

You can open a connection to your faye server. Note that `client` is probably best as a property, so your delegate can stick around. You can initiate a client with a subscription to a specific channel.

```swift
client = FayeClient(aFayeURLString: "ws://localhost:5222/faye", channel: "/cool")
client.delegate = self
client.connectToServer()
```

You can then also subscribe to additional channels either with block handlers like so:

```swift
let channelBlock:ChannelSubscriptionBlock = {(messageDict) -> Void in
  let text: AnyObject? = messageDict["text"]
  println("Here is the Block message: \(text)")
}
client.subscribeToChannel("/awesome", block: channelBlock)
```

or without them letting the delegate handle them like so:

```swift
self.client.subscribeToChannel("/delegates_still_rock")
```

After you are connected, there are some optional delegate methods that we can implement.

### connectedToServer

connectedToServer is called as soon as the client connects to the Faye server.

```swift
func connectedToServer() {
   println("Connected to Faye server")
}
```

### connectionFailed

connectionFailed is called when a cleint fails to connect to Faye server either initially or on a retry.

```swift
func connectionFailed() {
   println("Failed to connect to Faye server!")
}
```

### disconnectedFromServer

disconnectedFromServer is called as soon as the client is disconnected from the server..

```swift
func disconnectedFromServer() {
   println("Disconnected from Faye server")
}
```

### didSubscribeToChannel

didSubscribeToChannel is called when the subscribes to a Faye channel.

```swift
func didSubscribeToChannel(channel: String) {
   println("subscribed to channel \(channel)")
}
```

### didUnsubscribeFromChannel

didUnsubscribeFromChannel is called when the client unsubscribes to a Faye channel.

```swift
func didUnsubscribeFromChannel(channel: String) {
   println("UNsubscribed from channel \(channel)")
}
```

### subscriptionFailedWithError

The subscriptionFailedWithError method is called when the client fails to subscribe to a Faye channel.

```swift
func subscriptionFailedWithError(error: String) {
   println("SUBSCRIPTION FAILED!!!!")
}
```

### messageReceived

The messageReceived is called when the client receives a message from any Faye channel that it is subscribed to.	

```swift
func messageReceived(messageDict: NSDictionary, channel: String) {
   let text: AnyObject? = messageDict["text"]
   println("Here is the message: \(text)")
   
   self.client.unsubscribeFromChannel(channel)
}
```

The delegate methods give you a simple way to handle data from the server, but how do you publish data to a Faye channel?


### sendMessage

You can call sendMessage to send a dictionary object to a channel

```swift
client.sendMessage(["text": textField.text], channel: "/cool")
```

## Requirements

FayeSwift requires at least iOS8 / tvOS9.

## TODOs

- [ ] Cocoapods Integration
- [ ] Complete Docs
- [ ] Add Unit Tests
- [ ] Rethink use of optionals (?)
- [ ] Add block handlers (?)
- [ ] Support for a long-polling transport (?)

## License

BayeuxSwift is licensed under the MIT License and is a derivative of the [FayeSwift]() project.

FayeSwift is licensed under the MIT License.
