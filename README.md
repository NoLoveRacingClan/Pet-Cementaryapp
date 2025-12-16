# Pet-CementaryApp
This Project is the Code for a Pixel style Game called " Pet Cemetery" that I had build the game feature similar play style of its originator " Flappy Bird" only thing different is Our Character doesn't fly,  He jumps what does he jumps you may ask ? well the brink of puppy heaven with this game we focus our character with being on ground!  

***My Role***

Coder for The Group Young & The Reckless 

***Project Overview***

The Goal was to cater to our User Guides ( personal likes, Preferences, & Hobbies in one game and that being )

***Tools/Skills***

-Spritekit 

-Xcode

-Swift Ui

***Timeline***

First was setting up character and Phsyics Body of our characters once all that is good 
Second focusing on How to get the Graves to move & spawn them repeatedly & also  how to get the game to start when you tap the Screen
then Finally Working on collisions, dying, restarting game & Scoring !

***Contribution*** 

I was Coder and programmer for group 

***Photos of Game***

<img width="508" height="1083" alt="Screenshot 2025-12-15 at 4 58 41 PM" src="https://github.com/user-attachments/assets/cffdb082-ad38-4ed2-9d6f-14702a91ad51" />
<img width="247" height="504" alt="Screenshot 2025-12-16 at 1 31 47 PM" src="https://github.com/user-attachments/assets/df0cc443-a3a0-4dea-b507-dc46f98ebeca" />
<img width="222" height="506" alt="Screenshot 2025-12-16 at 1 33 03 PM" src="https://github.com/user-attachments/assets/6a42add5-24dd-40bd-87db-94281365b3d3" />
<img width="227" height="497" alt="Screenshot 2025-12-16 at 1 33 44 PM" src="https://github.com/user-attachments/assets/8bbba500-7602-43b0-9cfb-22fd674ee6f2" />
<img width="226" height="501" alt="Screenshot 2025-12-16 at 1 34 11 PM" src="https://github.com/user-attachments/assets/6af0a2d5-76bf-49a3-b794-7fca6d057fbb" />
<img width="234" height="495" alt="Screenshot 2025-12-16 at 1 37 17 PM" src="https://github.com/user-attachments/assets/f40907c5-32fd-48da-8fa9-61044e223d0e" />
<img width="214" height="408" alt="Screenshot 2025-12-16 at 2 00 44 PM" src="https://github.com/user-attachments/assets/e0496eb4-5f09-4daa-b3f4-84e32c1fab94" />
<img width="567" height="535" alt="Screenshot 2025-12-16 at 2 03 09 PM" src="https://github.com/user-attachments/assets/b0cb7ffb-2340-4d04-bc23-7b3fee6c0fc7" />

***The Code***


//

//  GameScene.swift

//  PetCementarydemo

//

//  Created by SwaViantae Phillips on 11/20/25.

//

import SpriteKit
import GameplayKit





struct PhysicsCategory {
    static let lucky : UInt32 = 0x1 << 1
    static let Ground : UInt32 = 0x1 << 2
    static let Grave : UInt32 = 0x1 << 3
    static let Score : UInt32 = 0x1 << 4

}


class GameScene: SKScene, SKPhysicsContactDelegate  {
    
    
    var Ground = SKSpriteNode()
    var lucky = SKSpriteNode()
    
    var gravePair = SKNode()
    
    var MoveAndRemove = SKAction()
    
    var gameStarted = Bool()
    
    var score = Int()
    let scoreLbl = SKLabelNode()
    
    var died = Bool()
    var restartBTN = SKSpriteNode()
    
    func restartScene(){
        
        self.removeAllChildren()
        self.removeAllActions()
        died = false
        gameStarted = false
        score = 0
        createScene()
        
    }
    
    func createScene(){
        self.physicsWorld.contactDelegate = self
        scoreLbl.position = CGPoint(x: self.frame.width / 2, y: self.frame.height / 2 + self.frame.height / 2.5)
        scoreLbl.text = "\(score)"
        scoreLbl.zPosition = 5
        self.addChild(scoreLbl)
        
        
        Ground = SKSpriteNode(imageNamed:"Ground")
        Ground.setScale(0.5)
        Ground.position = CGPoint (x: self.frame.width / 2, y: 0 + Ground.frame.height / 2)
        
        
        Ground.physicsBody?.categoryBitMask = PhysicsCategory.Ground
        Ground.physicsBody?.collisionBitMask = PhysicsCategory.lucky
        Ground.physicsBody?.contactTestBitMask = PhysicsCategory.lucky
        Ground.physicsBody?.affectedByGravity = false
        Ground.physicsBody?.isDynamic = false
        
        Ground.zPosition = 3
        
        self.addChild(Ground)
        
        
        
        lucky = SKSpriteNode(imageNamed: "lucky")
        lucky.size = CGSize(width: 60, height:70)
        lucky.position = CGPoint(x: self.frame.width / 2 - lucky.frame.width, y: self.frame.height / 2)
        
        lucky.physicsBody = SKPhysicsBody(circleOfRadius: lucky.frame.height / 2)
        lucky.physicsBody?.categoryBitMask = PhysicsCategory.lucky
        lucky.physicsBody?.collisionBitMask = PhysicsCategory.Ground | PhysicsCategory.Grave
        lucky.physicsBody?.contactTestBitMask = PhysicsCategory.Ground | PhysicsCategory.Grave | PhysicsCategory.Score
        lucky.physicsBody?.affectedByGravity = false
        lucky.physicsBody?.isDynamic = true
        
        lucky.zPosition = 2
        
        
        self.addChild(lucky)
        
        
        
        
    }
    
    override func didMove(to view: SKView) {
        
        createScene()
      
        
    }
    func createBTN(){
        
        restartBTN = SKSpriteNode(color: SKColor.red, size: CGSize(width: 200, height: 100) )
        restartBTN.position = CGPoint(x: self.frame.width / 2, y: self.frame.height / 2)
        restartBTN.zPosition = 6
        self.addChild(restartBTN)
    }
    
   func didBeginContact(contact: SKPhysicsContact) {
        let firstBody = contact.bodyA
        let secondBody = contact.bodyB
       
       if firstBody.categoryBitMask == PhysicsCategory.Score && secondBody.categoryBitMask == PhysicsCategory.lucky||firstBody.categoryBitMask == PhysicsCategory.lucky && secondBody.categoryBitMask == PhysicsCategory.Score {
           
           score+=1
           scoreLbl.text = "\(score)"
             
           
           
           
           
       }
       if firstBody.categoryBitMask == PhysicsCategory.lucky && secondBody.categoryBitMask == PhysicsCategory.Grave || firstBody.categoryBitMask == PhysicsCategory.Grave && secondBody.categoryBitMask == PhysicsCategory.lucky{
           
           died = true
           createBTN()
       }
       
       
    }
    
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        
        if gameStarted == false{
            
            gameStarted = true
            
            lucky.physicsBody?.affectedByGravity = true
            
            let spawn = SKAction.run({
                () in
                
                self.createGraves()
            })
            
            
            let delay = SKAction.wait(forDuration: 2.0)
            let spawnDelay = SKAction.sequence ([spawn, delay])
            _ = SKAction.repeatForever(spawnDelay)
            self.run(spawnDelay)
            
            _ = CGFloat(self.frame.width + gravePair.frame.width)
            let moveGrave = SKAction.move(by: CGVector(dx: 5, dy: 5),duration:20 )
            let removeGrave = SKAction.removeFromParent()
            MoveAndRemove = SKAction.sequence([moveGrave, removeGrave])
            
            
            
            
            
            lucky.physicsBody?.velocity = CGVectorMake(0,0)
            lucky.physicsBody?.applyImpulse(CGVectorMake(0,90))
            
        }
        else{
            
            if died == true{
                
                
            }
            else{
                lucky.physicsBody?.velocity = CGVectorMake(0,0)
                lucky.physicsBody?.applyImpulse(CGVectorMake(0, 90))
            }
            
        }
        
        for touch in touches {
            let location = touch.location(in: self)
            
            if died == true{
                
                if restartBTN.contains(location){restartScene()
                    
                }
                
                
            }
        
    }

    }
    
    func createGraves(){
        
        let scoreNode = SKSpriteNode()
        
        scoreNode.size = CGSize(width: 1, height: 200 )
        scoreNode.position = CGPoint(x: self.frame.width, y: self.frame.height / 2)
        scoreNode.physicsBody = SKPhysicsBody(rectangleOf: scoreNode.size)
        scoreNode.physicsBody?.affectedByGravity = false
        scoreNode.physicsBody?.isDynamic = false
        scoreNode.physicsBody?.categoryBitMask = PhysicsCategory.Score
        scoreNode.physicsBody?.collisionBitMask = 0
        scoreNode.physicsBody?.contactTestBitMask = PhysicsCategory.lucky
        scoreNode.color = SKColor.blue
        
        
        
        
        
        
        
        let gravePair = SKNode()

    
        let btmGrave = SKSpriteNode(imageNamed: "Grave")
        
        btmGrave.position = CGPoint(x: self.frame.width, y: self.frame.height / 2 - 350)
        
        btmGrave.setScale(0.5)
        
        gravePair.addChild(btmGrave)
        
        gravePair.zPosition = 1
        
        gravePair.addChild(scoreNode)
        
        gravePair.run(MoveAndRemove)
        
        
        self.addChild(gravePair)
        
    }
     
    
    
        override func update(_ currentTime: TimeInterval) {
            // Called before each frame is rendered
        }
    }

//
//  MainScreen.swift
//  PetCementarydemo
//
//  Created by SwaViantae Phillips on 12/8/25.
//

import SwiftUI
import SpriteKit

struct MainScreen: View {
    
//    var scene: SKScene {
//        let scene = GameScene()
//        scene.size = CGSize(width: 300, height: 400)
//        scene.scaleMode = .fill
//        return scene
//        
//    }
    
    var scene: SKScene {
        let scene = SKScene(fileNamed: "GameScene")!
        scene.scaleMode = .aspectFit
        return scene
    }
    var body: some View {
        SpriteView(scene: scene)
            .ignoresSafeArea()
    }
}

#Preview {
    MainScreen()
}
