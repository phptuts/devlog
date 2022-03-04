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

## Getting a sprite from SKScene to Code

Use the childNode(withName: ) function.

```swift
guard let track = self.childNode(withName: "\(i)") as? SKSpriteNode else { return }
```

## Creating a sprite by hand

1. Use the SKSpriteNode Constructor
2. Postion the sprite node
3. Use addChild to add it to the scene

```swift
func createPlayer() {
        player = SKSpriteNode(imageNamed: "player")
        guard let playerPosition = tracksArray?.first?.position.x else {
            return
        }
        
        player?.position = CGPoint(x: playerPosition, y: self.size.height / 2)
        addChild(player!)
    }
```

## Seeing if a player touched a sprite

1. Get the first touch
2. Get the location of first touch within the context of the view
3. Then find all the nodes that were touches


Here I only want the nodes with the names "right", "left", "down"

```swift
 override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        if let touch = touches.first {
            let location = touch.location(in: self)
            let node = self.nodes(at: location).first(where: { $0 is SKSpriteNode && ["right", "up", "down"].contains($0.name) })
            
            if node?.name == "right" {
                moveToNextTrack()
            } else if node?.name == "up" {
                moveVertically(up: true)
            } else if node?.name == "down" {
                moveVertically(up: false)
            }
        }
    }
```

## SKAction

SkActions are how animations, sounds and other actions are performed in the game.

SkActions can be ran against any SKNode

1. Create the SKAction
2. Run the action

```swift
 func moveVertically(up: Bool) {
        if up {
            let moveAction = SKAction.moveBy(x: 0, y: 3, duration: 0.01)
            let repeatAction = SKAction.repeatForever(moveAction)
            player?.run(repeatAction)
        } else {
            let moveAction = SKAction.moveBy(x: 0, y: -3, duration: 0.01)
            let repeatAction = SKAction.repeatForever(moveAction)
            player?.run(repeatAction)

        }
    }
```

## End an action

```swift
 override func touchesEnded(_ touches: Set<UITouch>, with event: UIEvent?) {
        
        if !movingToTrack {
            player?.removeAllActions()
        }
    }
    
    override func touchesCancelled(_ touches: Set<UITouch>, with event: UIEvent?) {
        player?.removeAllActions()

    }
```

## Update 

Update is called everytime render the loop is execute.  

```swift
override func update(_ currentTime: TimeInterval) {
        // Called before each frame is rendered
    }
```

## Creating Random 

Use GKRandomSource when creating random element.  GK means Game Kit.  

### Random Number

Create a random number from 0 to 2

```swift
GKRandomSource.sharedRandom().nextInt(upperBound: 3)
```

### Random Boolean

```swift
 GKRandomSource.sharedRandom().nextBool()
```

## Create Physics Bodies

Physics Bodies allow for physics engine to affect the sprite.  They are create by putting a border around the sprite.  You can set it's velocity, gravity etc.

```swift
        enemySprite.physicsBody = SKPhysicsBody(edgeLoopFrom: enemySprite.path!)
        enemySprite.physicsBody?.velocity = up ? CGVector(dx: 0, dy: velocityArray[track]) : CGVector(dx: 0, dy: -velocityArray[track])

```

## Getting a list of SKNodes by Name

```swift
self.enumerateChildNodes(withName: "ENEMY") { (node:SKNode, nil) in
            if node.position.y < -150 || node.position.y > self.size.height + 150 {
                node.removeFromParent()
            }
        }
```

## Example of creating a repeated Action

This will run the code and then wait 2 seconds.

```swift
self.run(SKAction.repeatForever(SKAction.sequence([
            SKAction.run {
                [weak self] in
                self?.spawnEnemies()
            },
            SKAction.wait(forDuration: 2)
        ])))
```

## Removing SKNodes

Once an SKNode has left the game you want to remove the node.  In the game we are building we do that createNode but I think it would better to do it in the update function.

```swift
  sprite.physicsBody = SKPhysicsBody(rectangleOf: enemySprite.size)
```

## SKPhysicsBody

The give your sprites a physics simulation which is calculated on every frame rendering.

### Static vs Dynamic

A static physics body is not movable.  Think a wall in a maze
A dynamic physics body is movable.  You can toggle the property like so.

```swift
  sprite.physicsBody?.isDynamic = true
```

### Edge Constructor

If you use an edge constructor when creating a physics body it means that your physics body does not have a mass or take up any space.  This allows you to apply a velocity without gravity affecting it. 

```swift
        enemySprite.physicsBody = SKPhysicsBody(edgeLoopFrom: enemySprite.path!)
```

### Physics Body Declared with texture

This has the worst performance but is the most accurate with collisions.  It uses the actual image to draw a boundary around the image for collision detection.  The best performance is circleOfRadius

```swift
        let texture = SKTexture(imageNamed: "player")
        player = SKSpriteNode(texture: texture, size: texture.size())
        
        // Best performance
        // SKPhysicsBody(circleOfRadius: 32)
```

## Collision Detection Bitmasks

- Category Bitmask -> Define the category of object 
- Collision Bitmask -> Define which categories can collide with each other
- Contact Test Bitmask -> Define what collision we want to notified with in the game scene

![Screen Shot 2022-03-03 at 9 58 10 PM](https://user-images.githubusercontent.com/9620015/156708118-386e9d6a-b977-4d96-9010-8bf452b4382d.png)

