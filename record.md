![](https://img.shields.io/badge/Swift-v5.1-blue)
## PropertyWrapper
```swift
@propertyWrapper
struct Log<T> {
    private(set) var logs = [String]()
    private(set) var format: String?

    init(wrappedValue value: T? = nil, format: String? = nil) {
        self.wrappedValue = value
        self.format = format
    }

    var wrappedValue: T? {
        didSet {
            if let format = format {
                if let value = wrappedValue {
                    logs.append(String(format: format, "\(Date())", "\(value)"))
                } else {
                    logs.append(String(format: format, "\(Date())", "nil"))
                }
            } else {
                if let value = wrappedValue {
                    logs.append("\(Date()) - \(value)")
                } else {
                    logs.append("\(Date()) - nil")
                }

            }
        }
    }
}
```

## Model
```swift
struct User: CustomStringConvertible {
    var description: String {
        return "\(username)"
    }
    var username: String, password: String
}

struct System {
    @Log(format: "%@ - %@ logged in.")
    static private(set) var user: User

    static func login(by user: User) {
        System.user = user
    }

    static func inspect() {
        print("Access history\n-------")
        for log in _user.logs {
            print(log)
        }
    }
}
```

## Demo
```swift
System.login(by: User(username: "David", password: "1234"))
DispatchQueue.main.asyncAfter(deadline: DispatchTime.now() + 2) {
    System.login(by: User(username: "John", password: "1234"))
    System.inspect()
}

```
## Result
```shell
Access history
-------
2019-08-20 11:23:52 +0000 - David logged in.
2019-08-20 11:23:54 +0000 - John logged in.
```

Author: [Wayne Hsiao](mailto:chronicqazxc@gmail.com)
