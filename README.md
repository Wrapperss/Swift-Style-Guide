# Swift-Style-Guide
Wrappers' Swift Style Guide

**欢迎到我的[博客](https://wrapperss.github.io/2019/02/11/Wrappers-Swift-Style-Guide/)阅读，体验最佳**

> 本文参考[Airbnb's Swift Style Guide](https://github.com/airbnb/swift)
> 总结出我个人认为最合适的代码风格
> 感谢[Airbnb](https://www.airbnb.com/)
## Goals(目标)
* **让阅读和理解代码更加简单**
* **使代码更易维护**
* **减少小的编程错误**
* **减少代码的认知负担**
* **更注重代码的逻辑，而不是代码风格**

**PS：简洁不是我们最主要的追求。只有代码质量得到保证（例如：可读性等），我们才会适当追求简洁。**

## Xcode Formatting(Xcode样式)
* **每行中最多包含100个单词**
* **用4个空格来进行缩减**
* **在每一行末尾以及在文件的末尾不要留有无用的空格**

## Naming(命名)
### 类和协议使用大驼峰式大小写(也叫[帕斯卡命名法PascalCase](https://zh.wikipedia.org/wiki/%E5%B8%95%E6%96%AF%E5%8D%A1%E5%91%BD%E5%90%8D%E6%B3%95))
例如：
```swift
protocol SpaceThing {
    // ...
}

class Spacefleet: SpaceThing {
    
    enum Formation {
        // ...
    }
    
    class Spaceship {
        // ...
    }
    
    var ships: [Spaceship] = []
    static let worldName: String = "Earth"
    
    func addShip(_ ship: Spaceship) {
        // ...
    }
}

let myFleet = Spacefleet()
```
**例外：如果私有属性支持具有更高访问级别的同名属性或方法，则可以在其前面加下划线**
**Why?**
在某些场景里，对属性和方法用一个更具描述性的命名更加易读。
- 类型擦除

```swift
public final class AnyRequester<ModelType>: Requester {
    
    public init<T: Requester>(_ requester: T) where T.ModelType == ModelType {
        _executeRequest = requester.executeRequest
    }
    
    @discardableResult
    public func executeRequest(_ request: URLRequest,
                               onSuccess: @escaping (ModelType, Bool) -> Void,
                               onFailure: @escaping (Error) -> Void) -> URLSessionCancellable
    {
        return _executeRequest(request, session, parser, onSuccess, onFailure)
    }
    
    private let _executeRequest: (URLRequest, @escaping (ModelType, Bool) -> Void, @escaping (NSError) -> Void) -> URLSessionCancellable
}
```

- 用一个更具描述的命名更好

```swift
final class ExperiencesViewController: UIViewController {
    // We can't name this view since UIViewController has a view: UIView property.
    private lazy var _view = CustomView()
    
    loadView() {
    self.view = _view
    }
}
```
### 用类似`isSpaceship`。`hasSpacesuit `来命名布尔(booleans)类型
让人更加明确这是一个布尔(booleans)类型

### 首字母缩略词(如：URL)应该是所有字母保持大写，除非作为名称开头应该为小写
例如：
```swift
// WRONG
class UrlValidator {
    
    func isValidUrl(_ URL: URL) -> Bool {
        // ...
    }
    
    func isUrlReachable(_ URL: URL) -> Bool {
        // ...
    }
}

let URLValidator = UrlValidator().isValidUrl(/* some URL */)

// RIGHT
class URLValidator {
    
    func isValidURL(_ url: URL) -> Bool {
        // ...
    }
    
    func isURLReachable(_ url: URL) -> Bool {
        // ...
    }
}

let urlValidator = URLValidator().isValidURL(/* some URL */)
```
### 命名时应先写其普遍的部分，再写其特殊的部分
其中普遍的部分，要根据上下文来理解，但可以粗略的理解为‘当你搜索时，最能帮助你缩小范围的部分’。更重要的是你需要有条理的去管理你的命名。
例如：
```swift
// WRONG
let rightTitleMargin: CGFloat
let leftTitleMargin: CGFloat
let bodyRightMargin: CGFloat
let bodyLeftMargin: CGFloat

// RIGHT
let titleMarginRight: CGFloat
let titleMarginLeft: CGFloat
let bodyMarginRight: CGFloat
let bodyMarginLeft: CGFloat
```
### 如果名称不明确，则在名称中包含有关类型的提示
例如：
```swift
// WRONG
let title: String
let cancel: UIButton

// RIGHT
let titleText: String
let cancelButton: UIButton
```
### 处理Event的函数应该用过去时态(可忽略)
例如：
```swift

// WRONG
class ExperiencesViewController {

  private func handleBookButtonTap() {
    // ...
  }

  private func modelChanged() {
    // ...
  }
}

// RIGHT
class ExperiencesViewController {

  private func didTapBookButton() {
    // ...
  }

  private func modelDidChange() {
    // ...
  }
}
```
### 避免Objective-C风格的缩写前缀
例如:
```swift
// WRONG
class AIRAccount {
  // ...
}

// RIGHT
class Account {
  // ...
}
```
### 避免在非视图控制器的类中是使用Controller单词

## Style(风格)
### 当可以轻易推断出类型时，不显示的书写类型，保持代码简洁
例如：
```swift
// WRONG
let host: Host = Host()

// RIGHT
let host = Host()
```
```swift
enum Direction {
  case left
  case right
}

func someDirection() -> Direction {
  // WRONG
  return Direction.left

  // RIGHT
  return .left
}
```
### 只有在必须时(即：在消除歧义或语言要求时)，才使用self关键词
例如：
```swift
final class Listing {

  init(capacity: Int, allowsPets: Bool) {
    // WRONG
    self.capacity = capacity
    self.isFamilyFriendly = !allowsPets // `self.` not required here

    // RIGHT
    self.capacity = capacity
    isFamilyFriendly = !allowsPets
  }

  private let isFamilyFriendly: Bool
  private var capacity: Int

  private func increaseCapacity(by amount: Int) {
    // WRONG
    self.capacity += amount

    // RIGHT
    capacity += amount

    // WRONG
    self.save()

    // RIGHT
    save()
  }
}
```
### 在多行数组的最后一个元素后加上逗号
例如：
```swift
// WRONG
let rowContent = [
  listingUrgencyDatesRowContent(),
  listingUrgencyBookedRowContent(),
  listingUrgencyBookedShortRowContent()
]

// RIGHT
let rowContent = [
  listingUrgencyDatesRowContent(),
  listingUrgencyBookedRowContent(),
  listingUrgencyBookedShortRowContent(),
]
```
### 对元组的每个成员进行命名，增加可读性
例如：
```swift
// WRONG
func whatever() -> (Int, Int) {
  return (4, 4)
}
let thing = whatever()
print(thing.0)

// RIGHT
func whatever() -> (x: Int, y: Int) {
  return (x: 4, y: 4)
}

// THIS IS ALSO OKAY
func whatever2() -> (x: Int, y: Int) {
  let x = 4
  let y = 4
  return (x, y)
}

let coord = whatever()
coord.x
coord.y
```
### 对于`CGRect`，`CGPoint`，`NSRange`等使用构造函数，还是不`make`函数
例如：
```swift
// WRONG
let rect = CGRectMake(10, 10, 10, 10)

// RIGHT
let rect = CGRect(x: 0, y: 0, width: 10, height: 10)
```
### 使用现代的Swift扩展方法，而不是Objective-C的全局扩展
例如：
```swift
// WRONG
var rect = CGRectZero
var width = CGRectGetWidth(rect)

// RIGHT
var rect = CGRect.zero
var width = rect.width
```
### 冒号紧跟着前一个词，后面加一个空格
例如：
```swift
// WRONG
var something : Double = 0

// RIGHT
var something: Double = 0
```
```swift
// WRONG
class MyClass : SuperClass {
  // ...
}

// RIGHT
class MyClass: SuperClass {
  // ...
}
```
```swift
// WRONG
var dict = [KeyType:ValueType]()
var dict = [KeyType : ValueType]()

// RIGHT
var dict = [KeyType: ValueType]()
```
### 在返回箭头的两边都加上空格，增加可读性
例如：
```swift
// WRONG
func doSomething()->String {
  // ...
}

// RIGHT
func doSomething() -> String {
  // ...
}
```
```swift
// WRONG
func doSomething(completion: ()->Void) {
  // ...
}

// RIGHT
func doSomething(completion: () -> Void) {
  // ...
}
```
### 省略不必要的括号
例如：
```swift
// WRONG
if (userCount > 0) { ... }
switch (someValue) { ... }
let evens = userCounts.filter { (number) in number % 2 == 0 }
let squares = userCounts.map() { $0 * $0 }

// RIGHT
if userCount > 0 { ... }
switch someValue { ... }
let evens = userCounts.filter { number in number % 2 == 0 }
let squares = userCounts.map { $0 * $0 }
```
### 当所有参数都没有标记时，忽略case语句中的枚举关联值
例如：
```swift
// WRONG
if case .done(_) = result { ... }

switch animal {
case .dog(_, _, _):
  ...
}

// RIGHT
if case .done = result { ... }

switch animal {
case .dog:
  ...
}
```
## Functions(函数)
### 在函数定义时，忽略`Void`的返回类型
例如：
```swift
// WRONG
func doSomething() -> Void {
  ...
}

// RIGHT
func doSomething() {
  ...
}
```
## Closures(闭包)
### 在闭包申明中优先使用`Void`返回类型，而不是`()`
例如：
```swift
// WRONG
func method(completion: () -> ()) {
  ...
}

// RIGHT
func method(completion: () -> Void) {
  ...
}
```
### 不被使用到参数应该写作`_`
例如：
```swift
// WRONG
someAsyncThing() { argument1, argument2, argument3 in
  print(argument3)
}

// RIGHT
someAsyncThing() { _, _, argument3 in
  print(argument3)
}
```
### 闭包的结束括号应该和开始括号具有相同的缩进
例如：
```swift
// WRONG

match(pattern: pattern)
  .compactMap { range in
    return Command(string: contents, range: range)
}
  .compactMap { command in
    return command.expand()
}

values.forEach { value in
  print(value)
  }

// RIGHT

match(pattern: pattern)
  .compactMap { range in
    return Command(string: contents, range: range)
  }
  .compactMap { command in
    return command.expand()
  }

values.forEach { value in
  print(value)
}
```
### 单行闭包要在大括号左右留出适当空间
例如：
```swift
// WRONG
let evenSquares = numbers.filter {$0 % 2 == 0}.map {  $0 * $0  }

// RIGHT
let evenSquares = numbers.filter { $0 % 2 == 0 }.map { $0 * $0 }
```
## Operators(操作符)
### 中置操作符左右都需要留有一个空格
宁愿使用圆括号`()`，也不要用多个操作符对语句进行可视分组。
例如：
```swift
// WRONG
let capacity = 1+2
let capacity = currentCapacity   ?? 0
let mask = (UIAccessibilityTraitButton|UIAccessibilityTraitSelected)
let capacity=newCapacity
let latitude = region.center.latitude - region.span.latitudeDelta/2.0

// RIGHT
let capacity = 1 + 2
let capacity = currentCapacity ?? 0
let mask = (UIAccessibilityTraitButton | UIAccessibilityTraitSelected)
let capacity = newCapacity
let latitude = region.center.latitude - (region.span.latitudeDelta / 2.0)
```
## Patterns(模式)
### 尽可能在`init`时，初始化属性，而不要使用隐式解包。
例如：
```swift
// WRONG
class MyClass: NSObject {
    var someValue: Int!
    
    init() {
        super.init()
        someValue = 5
    }
}

// RIGHT
class MyClass: NSObject {
    var someValue: Int
    
    init() {
        someValue = 0
        super.init()
    }
}
```
### 避免在`init`方法中执行任何有意义或耗时的工作
避免执行打开数据库连接，发送网络请求，从硬盘中读取一个大的数据。如果你需要在对象生成之前完成一些事，你可以用类似`start()`的方法。
### 将属性观察器内复杂的代码，抽取到单独方法中
例如：
```swift
// WRONG
class TextField {
  var text: String? {
    didSet {
      guard oldValue != text else {
        return
      }

      // Do a bunch of text-related side-effects.
    }
  }
}

// RIGHT
class TextField {
  var text: String? {
    didSet { updateText(from: oldValue) }
  }

  private func updateText(from oldValue: String?) {
    guard oldValue != text else {
      return
    }

    // Do a bunch of text-related side-effects.
  }
}
```
### 将闭包中复杂的代码，抽取到单独的方法中
例如：
```swift
//WRONG
class MyClass {

  func request(completion: () -> Void) {
    API.request() { [weak self] response in
      if let strongSelf = self {
        // Processing and side effects
      }
      completion()
    }
  }
}

// RIGHT
class MyClass {

  func request(completion: () -> Void) {
    API.request() { [weak self] response in
      guard let strongSelf = self else { return }
      strongSelf.doSomething(strongSelf.property)
      completion()
    }
  }

  func doSomething(nonOptionalParameter: SomeClass) {
    // Processing and side effects
  }
}
```
### 在代码块的开头优先使用`guard`
当所有的保护语句在顶部组合起来，而不是和业务逻辑混合起来，这样更容易对代码进行推理。
### 访问控制应该尽可能的严格
除非你需要，否则public优先于open，privae优先于fileprivate
### 尽可能避免全局函数
例如：
```swift
// WRONG
func age(of person, bornAt timeInterval) -> Int {
  // ...
}

func jump(person: Person) {
  // ...
}

// RIGHT
class Person {
  var bornAt: TimeInterval

  var age: Int {
    // ...
  }

  func jump() {
    // ...
  }
}
```
### 如果常量是私有的，尽量放在文件的顶部
例如：
```swift
private let privateValue = "secret"

public class MyClass {

  public static let publicValue = "something"

  func doSomething() {
    print(privateValue)
    print(MyClass.publicValue)
  }
}
```
### 只有在有语义含义时，才使用`optionals`
### 尽量使用不可以变的值
在数组操作中用`map`和`compactMap`代替`appending`，用`filter`代替`removing`。  
例如：
```swift
// WRONG
var results = [SomeType]()
for element in input {
  let result = transform(element)
  results.append(result)
}

// RIGHT
let results = input.map { transform($0) }
```
```swift
// WRONG
var results = [SomeType]()
for element in input {
  if let result = transformThatReturnsAnOptional(element) {
    results.append(result)
  }
}

// RIGHT
let results = input.compactMap { transformThatReturnsAnOptional($0) }
```
### 默认的静态方法
如果一个方法需要被复写，应该使用`class`  
例如：
```swift
// WRONG
class Fruit {
  class func eatFruits(_ fruits: [Fruit]) { ... }
}

// RIGHT
class Fruit {
  static func eatFruits(_ fruits: [Fruit]) { ... }
}
```
### 尽量使用`final`关键词
如果一个`class`需要被继承，则不写`final`  
例如：
```swift
// WRONG
class SettingsRepository {
  // ...
}

// RIGHT
final class SettingsRepository {
  // ...
}
```
### 在`switch`一个`enum`时，永远不要使用`default`
`enum`要求遍历所有的`case`，如果使用`default`，在新加入一个`case`时，会造成不可以预见的错误。
```swift
// WRONG
switch anEnum {
case .a:
  // Do something
default:
  // Do something else.
}

// RIGHT
switch anEnum {
case .a:
  // Do something
case .b, .c:
  // Do something else.
}
```
### 在检查`optional`是否是nil时，如果不需要它的值，不要使用值的绑定
例如：
```swift
var thing: Thing?

// WRONG
if let _ = thing {
  doThing()
}

// RIGHT
if thing != nil {
  doThing()
}
```
## File Organization(文件组织)
### 按字母排序书写`import`语句，在`import`语句前，不要留有空行
例如：
```swift
// WRONG

//  Copyright © 2018 Airbnb. All rights reserved.
//
import DLSPrimitives
import Constellation
import Epoxy

import Foundation

//RIGHT

//  Copyright © 2018 Airbnb. All rights reserved.
//

import Constellation
import DLSPrimitives
import Epoxy
import Foundation
```
**例外：`@testable import`应该空一行，另起一组**
例如：
```swift
// WRONG

//  Copyright © 2018 Airbnb. All rights reserved.
//

import DLSPrimitives
@testable import Epoxy
import Foundation
import Nimble
import Quick

//RIGHT

//  Copyright © 2018 Airbnb. All rights reserved.
//

import DLSPrimitives
import Foundation
import Nimble
import Quick

@testable import Epoxy
```
### 垂直的空格分隔只能为一行
### 文件应该最后留有一个空行

## 感谢阅读，祝您生活愉快！
