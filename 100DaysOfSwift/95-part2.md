# 100DaysOfSwift Day 95 Part 2 - 2022-02-15

Today I learned how to create holes in images and combine them with effects.  I also learned how to got practice with collisions and bitMaskCategories.  I got more practice with SKActions and scheduling stuff in a queue.  It was a fun game to build.

## Video

https://user-images.githubusercontent.com/9620015/154198230-97b70532-8801-47e5-a6ff-a512909157db.mov

## Code

https://github.com/phptuts/100DaysOfSwift/blob/master/Project29/Project29/BuildingNode.swift

https://github.com/phptuts/100DaysOfSwift/blob/master/Project29/Project29/GameScene.swift



## Collision Detection

We always put the lower number categoryBitMask as the first item because our lowest number is banana.  It makes it easier to write logic for it hitting a building or a player.

```swift
    func didBegin(_ contact: SKPhysicsContact) {
        let firstBody: SKPhysicsBody
        let secondBody: SKPhysicsBody
        
        if contact.bodyA.categoryBitMask < contact.bodyB.categoryBitMask {
            firstBody = contact.bodyA
            secondBody = contact.bodyB
        } else {
            firstBody = contact.bodyB
            secondBody = contact.bodyA
        }
        
        guard let firstNode = firstBody.node else { return }
        guard let secondNode = secondBody.node else { return }
        
        if firstNode.name == "banana" && secondNode.name == "building" {
            bananaHit(building: secondNode, atPoint: contact.contactPoint)
        }
        
        if firstNode.name == "banana" && secondNode.name == "player1" {
            destroy(player: player1)
        }
        
        if firstNode.name == "banana" && secondNode.name == "player2" {
            destroy(player: player2)
        }
    }

```

## Making banana blowup

We create an explosion effect with an emitter.  
Then remove tha banana from the scene and call building.hit function which will remove a part of the building.

```swift
func bananaHit(building: SKNode, atPoint contactPoint: CGPoint) {
    guard let building = building as? BuildingNode else { return }
    let buildingLocation = convert(contactPoint, to: building)
    building.hit(at: buildingLocation)

    if let explosion = SKEmitterNode(fileNamed: "hitBuilding") {
        explosion.position = contactPoint
        addChild(explosion)
    }

    banana.name = ""
    banana.removeFromParent()
    banana = nil
    changePlayer()
}
```

## Eating the building

We draw a circle at the point that the banana hits the building.  We then to the blendMode to clear to remove part of the building use that to redraw the image.

```swift
func hit(at point: CGPoint) {
    let convertedPoint = CGPoint(x: point.x + size.width / 2, y: abs(point.y - (size.height  / 2)))

    let renderer = UIGraphicsImageRenderer(size: size)
    let img = renderer.image { ctx in
        currentImage.draw(at: .zero)
        ctx.cgContext.addEllipse(in: CGRect(x: convertedPoint.x - 32, y: convertedPoint.y - 32, width: 64, height: 64))
        ctx.cgContext.setBlendMode(.clear)
        ctx.cgContext.drawPath(using: .fill)
    }

    texture = SKTexture(image: img)
    currentImage = img
    configurePhysics()

}
```

## Hitting the player

When it hits the player create an explosion with the emitter and make the player that was hit disappear.  Then use the dispatch queue to wait 2 second to restart the game.  Use a SKTransition to make a cool effect for starting the scene.

```swift
func destroy(player: SKSpriteNode) {
    if let explosion = SKEmitterNode(fileNamed: "hitPlayer") {
        explosion.position = player.position
        addChild(explosion)
    }


    player.removeFromParent()
    banana.removeFromParent()

    DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
        let newGame = GameScene(size: self.size)
        newGame.viewController = self.viewController
        self.viewController?.currentGame = newGame

        self.changePlayer()
        newGame.currentPlayer = self.currentPlayer

        let transition = SKTransition.doorway(withDuration: 1.5)
        self.view?.presentScene(newGame, transition: transition)
    }

}
```

## Cleaning up stray bananas

If the banana is off the screne in 1000 pixels in either way direction remove it and change players.

```swift
    override func update(_ currentTime: TimeInterval) {
        guard  banana != nil else { return }
        
        if abs(banana.position.y) > 1000 {
            banana.removeFromParent()
            banana = nil
            changePlayer()
        }
    }
```
