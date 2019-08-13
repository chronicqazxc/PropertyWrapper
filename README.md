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
        print("----\(type)----\nHealth: \(health)\nAttack: \(attack)\nSupplyment: \(supplyment)\n")
    }
}

struct Hero: Character {
    @Range(0...100) var health: Int = 100
    @Range(0...100) var attack: Int = 20
    @Range(0...0) var supplyment: Int = 0
    var type: String {
        return "Hero"
    }
    mutating func upgrade() {
        print("\(type) upgraged")
        health = health + 5
        attack = attack + 5
        status()
    }
}

struct Villian: Character {
    @Range(0...60) var health: Int = 60
    @Range(0...20) var attack: Int = 20
    @Range(0...0) var supplyment: Int = 0
    var type: String {
        return "Villian"
    }
}

struct Wizard: Character {
    @Range(0...100) var health: Int = 100
    @Range(0...50) var attack: Int = 50
    @Range(0...20) var supplyment: Int = 5
    var type: String {
        return "Wizard"
    }

    mutating func upgrade() {
        health = health + 5
        supplyment = supplyment + 5
    }
}
```

## Play game
```swift
var hero = Hero()
var villian = Villian()
var wizard = Wizard()
hero.gotHit(by: villian)
hero.gotSupplyment(by: wizard)
hero.upgrade()
hero.upgrade()
hero.upgrade()
hero.upgrade()
hero.upgrade()
```

## Log
```shell
Hero got hit by Villian
----Hero----
Health: 80
Attack: 20
Supplyment: 0

Hero got supplyment by Wizard
----Hero----
Health: 85
Attack: 20
Supplyment: 0

Hero upgraged
----Hero----
Health: 90
Attack: 25
Supplyment: 0

Hero upgraged
----Hero----
Health: 95
Attack: 30
Supplyment: 0

Hero upgraged
----Hero----
Health: 100
Attack: 35
Supplyment: 0

Hero upgraged
----Hero----
Health: 100
Attack: 40
Supplyment: 0

Hero upgraged
----Hero----
Health: 100
Attack: 45
Supplyment: 0
```

## Conclusion

## Author
[Wayne Hsiao](mailto:chronicqazxc@gmail.com)
