# PropertyWrapper

PropertyWrapper was introduced in [swift-evolution](https://github.com/apple/swift-evolution/blob/master/proposals/0258-property-wrappers.md) by proposal [SE-0258](https://github.com/apple/swift-evolution/blob/master/proposals/0258-property-wrappers.md) and included in Swfit5.1, you can find more detail in this great article [NSHipster PropertyWrapper](https://nshipster.com/propertywrapper/).

## Usage
Add `@propertyWrapper` infront of the type you want to define as a property wrapper and implement property `var wrappedValue` for every property wrappers.

## Concept
As the name says, everytime when the wrapped property valued has been changed the wrapper will be trigged and return/set the value in `var wrappedValue` for the wrapped property.

## Usage
There are so many posiblilites to take advantage of property wrapper to remove boilerplate code, following are two examples to demostrate how to properly use the property wrapper.

|Examples|
|--|
|[Log](./record.md)|
|[Constraint values](./constraint_value.md)|

## Author
[Wayne Hsiao](mailto:chronicqazxc@gmail.com)
