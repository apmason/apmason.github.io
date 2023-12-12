---
layout: post
title:  "VisionOS Design Patterns"
date:   2023-12-03 14:02:19 -0800
categories: jekyll update
---
With the advent of SwiftUI and now visionOS, new paradigms for app development are being formed. Below is a pattern that I believe will become quite standard. 

{% highlight swift %}
@State private var appState = AppState()
    
@State private var immersionStyle: ImmersionStyle = .mixed

var body: some SwiftUI.Scene {
    WindowGroup(id: "MainMenu") {
        ContentView()
            // modifiers
    }
    .windowStyle(.plain)
    .windowResizability(.contentSize)
    
    // Present a mixed immersive space in expanded mode.
    ImmersiveSpace(id: "Track") {
        TrackBuildingView()
            .environment(appState)
    }
    .immersionStyle(selection: $immersionStyle, in: .mixed)
}
{% endhighlight %}

The `.mixed` parameter to the `.immersionStyle(selection:in:)` modifier is important. This is how you will achieve full "AR" in a visionOS app. The other important options are `.progressive`, which is where the edges of your vision are your room, and the center is more like true VR, and `.full`, which is full VR.

This example comes from Apple's `SwiftSplash` [example](https://developer.apple.com/documentation/visionos/swift-splash) example.

I will admit, I have mixed feeling about this becoming the new paradigm. During the UIKit era of app development, so many design patterns were created to maximize testability, separation of concerns, and dependency injection. Singletons were viewed as a necessary, yet regrettable, evil. And now... singletons are THE default paradigm. And I've also found that `EnvironmentObjects` make testing harder. Has anyone found a solution to this, or run into this? Let me know on [X](https://x.com/mason_ap).
