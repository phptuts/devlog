# How to Create iOS 2D Games with SpriteKit and Swift

## What is sprite kit

SpriteKit is a game engine that is built and maintained by apple.  It's used for rendering and animating arbitrary textured images.  

## What is Textured Image

Textures and images are mostly the same things in our game because we deal with 2D Games.  A texture is usually an image that wraps around a 3D object.


## SKView

This what renders the game

## SKScene

This is the root node in the a game and is used to control all and handle events for the game

## SKSpriteNode

It's a textured image that is used to render a game.

## Diagram of nodes in a game

![Screen Shot 2022-02-22 at 9 30 09 PM](https://user-images.githubusercontent.com/9620015/155266821-fa4cff06-4b46-4927-b85f-edbed8323155.png)

## How to turn a App to SpriteKit

1\. Create a Spritekit Scene
2\. Create a class that inherits from SKScene
3\. Associate your custom Scene class with your SpriteKit Scene.  Spritekit Scene is like a storyboard
4\. In the view controller create the SKScene and assign it to the view.

```swift
import UIKit
import SpriteKit

class GameViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        if let view = self.view as? SKView {
            if let scene = SKScene(fileNamed: "GameScene") {
                scene.scaleMode = .aspectFill
                view.presentScene(scene)
            }
        }
    }
}
```

## Game Scene

So I dragged and dropped some images from the course into the game scene.  I had to adjust the z-index on the sky scene to be able to see everything.  I also set width to 896 and height to 414.

![Simulator Screen Shot - iPhone 11 - 2022-02-22 at 22 17 13](https://user-images.githubusercontent.com/9620015/155270214-1f17dcb8-31ce-4ec2-9206-b475defc083e.png)

## Animating Sprites Using the GameScene.sks 

- You must put a z position on all the sprites
- Animate grab a move from + icon on xcode
- Color Sprite can be turned into image by adding a texture

https://user-images.githubusercontent.com/9620015/155473434-d112ed8e-a93f-4ad0-820f-f9e0b0a3dee0.mp4


