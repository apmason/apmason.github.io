---
layout: post
title:  "VisionOS Space Toggling"
date:   2023-12-10 14:02:19 -0800
categories: jekyll update
---

I am converting my multiplayer iOS pong app to visionOS. Here is a fun trick that might come in handy for some developers. Apple recommends starting your app in a Window, and then if needed open up a full immersive space. How would this be done?

Here is a button that user clicks on when they want to join a friends server. Elsewhere in the code they enter the `serverCode`.

{% highlight swift %}
Button("Join") {
    guard let codeDigits = Int(viewModel.serverCode) else { return }
    Task {
        do {
            let info = try await viewModel.joinServer(codeDigits)
            try await viewModel.connectToServer(info)
            
            switch await openImmersiveSpace(id: "ImmersiveSpace") {
            case .opened:
                // If we opened the immersive space, close the window
                dismissWindow(id: "MainMenu")
            case .error, .userCancelled:
                print("Error opening me space")
                // TODO: Present error upon closing the space.
            @unknown default:
                break
            }
        
        } catch {
            // Bubble up error to user
        }
    }
}
{% endhighlight %}

If we succesfully connected to the server, we open the immersive space to play the game, and then you dismiss the current window. Your immersive view would be declared like this:

{% highlight swift %}
@main
@MainActor
struct PongApp: App {
    
    @State private var immersionStyle: ImmersionStyle = .mixed
    
    @State private var appState = AppState()
    
    init() {
        PaddleSystem.registerSystem()
    }
    
    let size: Float = 10
    
    var body: some Scene {
        WindowGroup(id: "MainMenu") {
            VisionMainMenuView()
                .environment(appState)
        }.windowStyle(.volumetric)

        ImmersiveSpace(id: "ImmersiveSpace") {
            VisionGameView()
                .environment(appState)
        }
        .immersionStyle(selection: $immersionStyle, in: .mixed)
    }
}
{% endhighlight %}