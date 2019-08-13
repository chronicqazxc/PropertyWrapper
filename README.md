# PropertyWrapper
![](https://img.shields.io/badge/Swift-v5.1-blue)
## PropertyWrapper
Implement `wrappedValue`, `init(initialValue value: T)`.
```swift
@propertyWrapper
struct Range<T: Comparable> {
    private var value: T
    private var range: ClosedRange<T>

    init(initialValue value: T, _ range: ClosedRange<T>) {
        self.value = value
        self.range = range
    }

    var wrappedValue: T {
        set {
            value = min(max(range.lowerBound, newValue), range.upperBound)
        }
        get {
            value
        }
    }
}
```

## Characters
```swift
protocol Character {
    var health: Int { set get }
    var attack: Int { get }
    var supplyment: Int { get }
    var type: String { get }
    var level: Int { get }
    mutating func upgrade()
    func status()
}

extension Character {
    mutating func gotSupplyment(by character: Character) {
        print("\(type) got supplyment by \(character.type)")
        self.health = self.health + character.supplyment
        status()
    }

    mutating func gotHit(by character: Character) {
        guard character.health > 0 else {
            return
        }
        print("\(type) got hit by \(character.type)")
        self.health = self.health - character.attack
        status()
    }

    mutating func upgrade() { }

    func status() {
        let message =
        """
        ----Status----
        Level: \(level)
        Health: \(health)
        Attack: \(attack)
        Supplyment: \(supplyment)

        """
        print(message)
    }
}

struct Hero: Character {
    @Range(0...100) var health: Int = 100
    @Range(0...100) var attack: Int = 20
    @Range(0...0) var supplyment: Int = 0
    @Range(1...100) var level: Int = 1
    var type: String {
        return "Hero"
    }
    mutating func upgrade() {
        print("\(type) upgraged")
        level = level + 1
        health = health + 5
        attack = attack + 5
        status()
    }
}

struct Villian: Character {
    @Range(0...60) var health: Int = 60
    @Range(1...20) var attack: Int = 1
    @Range(0...0) var supplyment: Int = 0
    @Range(1...20) var level: Int = Int.random(in: 1...20) {
        didSet {
            switch level {
            case 1...5:
                attack = Int.random(in: 1...5)
            case 6...10:
                attack = Int.random(in: 6...10)
            case 11...15:
                attack = Int.random(in: 11...15)
            case 16...20:
                attack = Int.random(in: 16...20)
            default:
                attack = 1
            }
        }
    }
    var type: String {
        return "Villian"
    }
}

struct Wizard: Character {
    @Range(0...100) var health: Int = 100
    @Range(0...50) var attack: Int = 50
    @Range(0...20) var supplyment: Int = 10
    @Range(1...100) var level: Int = 1
    var type: String {
        return "Wizard"
    }

    mutating func upgrade() {
        level = level + 1
        health = health + 5
        supplyment = supplyment + 5
    }
}
```

## Play game
```swift
print("Game start!")
var hero = Hero()
var villian = Villian()
villian.level = 10
var wizard = Wizard()
hero.status()
hero.gotHit(by: villian)
hero.gotSupplyment(by: wizard)
hero.upgrade()
hero.upgrade()
hero.upgrade()
villian.gotHit(by: hero)
villian.gotHit(by: hero)
print("You win!")
```

## Log
```shell
Game start!
----Status----
Level: 1
Health: 100
Attack: 20
Supplyment: 0

Hero got hit by Villian
----Status----
Level: 1
Health: 90
Attack: 20
Supplyment: 0

Hero got supplyment by Wizard
----Status----
Level: 1
Health: 100
Attack: 20
Supplyment: 0

Hero upgraged
----Status----
Level: 2
Health: 100
Attack: 25
Supplyment: 0

Hero upgraged
----Status----
Level: 3
Health: 100
Attack: 30
Supplyment: 0

Hero upgraged
----Status----
Level: 4
Health: 100
Attack: 35
Supplyment: 0

Villian got hit by Hero
----Status----
Level: 10
Health: 25
Attack: 10
Supplyment: 0

Villian got hit by Hero
----Status----
Level: 10
Health: 0
Attack: 10
Supplyment: 0

You win!
```

## Conclusion

## Author
[Wayne Hsiao](mailto:chronicqazxc@gmail.com)
