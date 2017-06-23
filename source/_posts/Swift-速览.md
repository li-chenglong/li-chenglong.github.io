title: Swift 速览
categories: The Swift Programming Language 笔记
---
这里的知识点，后续章节会有详细的讲解。
开启 Swift 的大门
```
print("Hello, world!")
```
在 Swift 中，这行代码就是一个完整的程序，不需要为输入输出或者字符串处理等功能导入单独的库。在全局范围编写的代码被用作程序的入口，不需要 main() 函数。也不需要在每个语句结尾处写分号。

## 简单值 (Simple Values)
使用 let 定义常量，使用 var 定义变量。编译时不需要知道常量的值，但是必须(只能)为它赋值一次。
```
var myVariable = 42
myVariable = 50
let myConstant = 42
```
常量和变量必须与要分配给它的值有相同的类型。但是不需要总是显式的写明类型。创建常量或者变量时提供一个初始值，编译器会自动推断其类型。上面的例子中，编译器推断 myVariable 是 integer 类型。
如果初始值不能提供足够的信息或者没有初始值，可以在变量名称后面指明类型，用 `: ` 分割
```
let implicitDouble = 70.0
let explicitDouble: Double = 70
let explicitFloat: Float = 4
```
值不会隐式转换为其他类型。需要明确的类型转换：
```
let label = "The width is "
let width = 94
let widthLabel = label + String(width)
```
在字符串中包含值有一种更简单的方法：将值写在括号中，并在括号前写反斜杠
```
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit"
```
多行的字符串，可以写在一对三个双引号 (`"""`) 之间。
```
let quotation = """
Even though there's whitespace to the left,
the actual lines aren't indented.
Except for this line.
Double quotes (") can appear without being escaped.

I still have \(apples + oranges) pieces of fruit.
"""
```
使用方括号 `[]` 创建数组和字典，通过索引或 key 访问它们的元素。最后一个元素后面允许使用逗号。
```
var shoppingList = ["catfish", "water", "tulips", "blue paint"]
shoppingList[0] = "bottle of water"

var occupations = [
"Malcolm": "Captain",
"Kaylee": "Mechainc",
]
occupations["Jayne"] = "Public Relations"
```
使用初始化语法，创建空数组或字典。
```
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```
如果能够推断类型信息，可以使用 `[]` 和 `[:]` 创建空数组和空字典。比如为变量设置新值或者将参数传递给函数时。
```
shoppingList = []
occupations = [:]
```

## 控制流 (Control Flow)
使用 if 和 switch 进行条件操作，使用 for-in, while,和 repeat-while 进行循环操作。条件或者循环变量的括号可以省略，语句体的大括号是必须的。
```
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
if score > 50 {
teamScore += 3
} else {
teamScore += 1
}
}
print(teamScore)
```
在条件语句中，条件必须是一个布尔表达式。这就意味着像 `if score { ... }`  这样的代码是错误的，它并不是隐式的和 0 比较。

可以使用 if 和 let 组合一起处理可能的值缺失。这些值表示为可选值。一个可选值表示有值，或者 nil (即值缺失)。在类型后面跟一个问号，表示值是可选的。
```
var optionalString: String? = "Hello"
print(optionalString == nil)

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
greeting = "Hello,\(name)"
}
``````
如果可选值为空，则条件为 false，就会跳过大括号内的代码。否则可选值被解包，并且赋给 let 后面的常量，这样代码块中就可以使用解包后的值了。

另一种处理可选值的方法是使用运算符  `??`  提供默认值。如果值缺失，则使用默认值。
```
let nickName: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "He \(nickName ?? fullName)"
```
Switch 语句支持各种数据类型和多种比较操作，不仅仅支持 integers 和相等判断。
```
let vegetable = "red pepper"
switch vegetable {
case "celery":
print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
print("Is it a spicy \(x)")
default:
print("Everingthing tastes good in soup.")
}
```
注意如何使用 let 将与模式相匹配的值赋给一个常量。
执行完匹配的 case 子句后，程序将退出 switch 语句。不会执行下一个 case，所以不需要在每一个 case 子句最后写 break。

通过提供一组键值对名称，使用 `for-in` 来迭代字典中的项。字典是无序的集合，所以键值对也是无序迭代。
```
let interestingNumbers = [
"Prime": [2, 3, 5, 7, 11, 13],
"Fibonacci": [1, 1, 2, 3, 5, 8],
"Square": [1, 4, 9, 16, 25],
]
var largest = 0
var name = ""
for (kind, numbers) in interestingNumbers {
for number in numbers {
if number > largest {
largest = number
name = kind
}
}
}
print("\(largest) from " + name)
```
使用 `while` 重复执行一段代码，直到条件不再满足。循环条件可以放在最后，确保循环至少执行一次。
```
var n = 2
while n < 100 {
n *= 2
}
print(n)

var m = 2
repeat {
m *= 2
} while m < 100
print(m)
```
可以通过使用 `..<` 创建索引范围
```
var total = 0
for i in 0..<4 {
total += i
}
print(total)
```
使用 `..<` 创建的范围不包含上限值，而使用 `...` 创建的范围包含上限值。

## 函数和闭包 (Functions and Closures)
使用`func`声明函数。通过函数名和括号中的参数列表来调用函数。使用 `->` 分隔参数名称、参数类型和返回值类型
```
func greet(person: String, day: String) -> String {
return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")
```
函数默认使用参数名称作为参数标签。可以在参数名称前自定义标签，或者使用 `_`表示不使用参数标签
```
func greet(_ person: String, on day: String) -> String {
return "Hello, \(person), today is \(day)."
}
print(greet("John", on: "Wednesday"))
```

使用元组 (tuple) 创建复合值，比如函数返回多个值。可以使用名称或者数字引用元组的值。
```
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
var min = scores[0]
var max = scores[0]
var sum = 0

for score in scores {
if score > max {
max = score
} else if score < min {
min = score
}
sum += score
}

return (min, max, sum)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.min)
print(statistics.2)
```
函数可以嵌套。嵌套函数可以访问其外部函数声明的变量。
```
func returnFifteen() -> Int {
var y = 10
func add() {
y += 5
}
add()
return y
}
returnFifteen()
```
函数是一等类型。可以将另一个函数当作返回值。
```
func makeIncrementer() -> ((Int) -> Int) {
func addOne(number: Int) -> Int {
return 1 + number
}
return addOne
}
var increment = makeIncrementer()
increment(7)
```
函数可以作为另一个函数的参数
```
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
for item in list {
if condition(item) {
return true
}
}
return false
}
func lessThanTen(number: Int) -> Bool {
return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```
函数是特殊的闭包，是一个稍后可以调用的代码块。闭包中的代码可以访问闭包创建时作用域内的变量和函数。即使闭包在其他不同的作用域中被执行，比如上面的嵌套函数。
可以写匿名闭包，使用 `in` 分隔参数、返回值和闭包体
```
numbers.map({ (number: Int) -> Int in
let result = 3 * number
return result
})
```
闭包的几种简写方式：
当类型已知时可以忽略类型，包括参数类型、返回类型。单语句闭包默认返回单语句的值。
```
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
```
可以通过数字而不是名称来引用参数。闭包作为函数的最后一个参数时，可以写在括号外面，当只有闭包一个参数时，可以省略括号。
```
let sortedNumbers = numbers.sorted(){ $0 > $1 }
// 或者 let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbres)
```
## 对象和类 (Objects and Classes)
使用 `class` 关键字加类名创建一个 class。类中属性和方法的声明分别与变量／常量和函数的声明方式一样，不过属性和方法是写在类中。
```
class Shape {
var numberOfSides = 0
func simpleDescription() -> String {
return "A shape with \(numberOfSides) sides."
}
}
```
类的实例化和点语法：
```
let shape = Shape()
shape.numberOfSides = 7
print(shape.simpleDescription())
```
构造器：
```
class NamedShape {
var numberOfSides: Int = 0
var name: String

init(name: String) {
self.name = name
}

func simpleDescription() -> String {
return "A shape with \(numberOfSides) sides."
}
}
```
使用 self 区分 name 属性和构造器参数 name。
无论是声明时赋初始值还是使用构造器，所有属性都需要被赋值。
在对象被释放之前，可以使用  `deinit` 创建析构器做一些清理工作。

子类在其类名后面添加其父类的名称，使用冒号分隔。可以根据需要添加或者忽略父类，不要求继承某一个标准的基类。
使用 `override` 重写父类的方法。不使用 `override` 就重写父类方法会被编译器标示为错误。编译器还会检查父类中是否真的有这个方法。
```
class Square: NamedShape {
var sideLength: Double

init(sideLength: Double, name: String) {
self.sideLength = sideLength
super.init(name: name)
numberOfSides = 4
}

func area() -> Double {
return sideLength * sideLength
}

override func simpleDescription() -> String {
return "A Square with sides of length \(sideLength)."
}
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```
除了简单的存储属性，属性还可以有一个 `getter` 和一个 `setter`。
```
class EquilateralTriangle: NamedShape {
var sideLength: Double = 0.0

init(sideLength: Double, name: String) {
self.sideLength = sideLength
super.init(name: name)
numberOfSides = 3
}

var perimeter: Double {
get {
return 3.0 * sideLength
}
set {
sideLength = newValue / 3.0
}
}

override func simpleDescription() -> String {
return "An equilateral triangle with sides of length \(sideLength)."
}
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
triangle.perimeter = 9.9
print(triangle.sideLength)
```
`perimeter` 的 setter 中，新值使用了隐式名称 newValue。也可以显示的定义名称。
```
set(value) {
sideLength = value / 3.0
}
```
`EquilateralTriangle` 类的初始化方法包含三步：
1. 设置子类声明的属性值。
2. 调用父类的构造器。
3. 修改父类声明的属性值。其他设置工作，包括调用方法，getter 或者 setter 也可以在此时完成。

属性观察器：`willSet` 和 `didSet` 当不需要计算属性值，但是仍然需要在设置新值前后提供某些操作时使用。提供的代码会在值改变时执行，但是构造器中赋值导致的值改变除外。
```
class TriangleAndSquare {
var triangle: EquilateralTriangle {
willSet {
square.sideLength = newValue.sideLength
}
}
var square: Square {
willSet {
triangle.sideLength = newValue.sideLength
}
}
init(size: Double, name: String) {
square = Square(sideLength: size, name: name)
triangle = EquilateralTriangle(sideLength: size, name: name)
}
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)
print(triangleAndSquare.triangle.sideLength)
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)
```

使用可选值时，在方法、属性或者下标前加 `?` 。如果可选值为 nil，`?` 之后的所有内容都被忽略，整个表达式的值为 nil。如果可选值可以被解包，`?` 之后的操作使用解包后的值。在两种情况下，表达式的值都是一个可选值。
```
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength
```

## 枚举和结构体 (Enumerations and Structures)
使用 `enum` 创建枚举。和类一样，枚举可以关联方法。
```
enum Rank: Int {
case ace = 1
case two, three, four, five, six, seven, eight, nine, ten
case jack, queen, king
func simpleDescription() -> String {
switch self {
case .ace:
return "ace"
case .jack:
return "jack"
case .queen:
return "queen"
case .king:
return "king"
default:
return String(self.rawValue)
}
}
}
let ace = Rank.ace
let aceRawValue = ace.rawValue
```
默认情况，Swift 为原始值从0分配值，依次加1。也可以显式的指定原始值。也可以使用浮点数或字符串作为原始值的类型。
可以使用构造器 `init?(rawValue:)` 初始化一个枚举。要么得到一个匹配的枚举成员，要么返回 nil。因为不是每一个值都有对应的枚举成员。
```
if let convertedRank = Rank(rawValue: 3) {
print(convertedRank.simpleDescription())
}
```
枚举值是实际的值，而不是原始值的一种书写方式。实际上，如果没有一个有意义的原始值，可以不提供原始值。
```
enum Suit {
case spades, hearts, diamons, clubs
func simpleDescription() -> String {
switch self {
case .spades:
return "spades"
case .hearts:
return "hearts"
case .diamons:
return "diamons"
case .clubs:
return "clubs"
}
}
}
let hearts = Suit.hearts
print(hearts.simpleDescription())
```
上面的代码中，有两种使用枚举值的方式：hearts 常量没有显式的指明类型，使用了全名引用 `Suit.hearts` 。方法体内 self 指明了类型，所以使用缩写引用 `.hearts`  。只要类型明确时，尽情使用缩写方式。

如果枚举成员有原始值，则原始值在枚举定义时就确定了，且不可修改。另一种方式是使用关联值，在创建实例的时候确定值，不同的实例可以关联不同的值，可以把关联值看作枚举的存储属性，可以修改。
```
enum ServerResponse {
case result(String, String)
case failure(String)
}
let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")

switch success {
case let .result(sunrise, sunset):
print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
case let .failure(message):
print("Failure...  \(message)")
}
```

使用 `struct` 创建结构体。结构体支持很多类的行为，包括方法和构造器。最大的不同是结构体在代码中传递时总是复制，而类是通过引用传递。
```
struct Card {
var rank: Rank
var suit: Suit
func simpleDescription() -> String {
return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
}
}
let threeOfSpades = Card(rank: .three, suit: .spades)
print(threeOfSpades.simpleDescription())
```

## 协议和扩展 (Protocols and Extensions)
使用 `protocol` 声明一个协议。类、枚举和结构体都可以遵循协议。
```
protocol ExampleProtocol {
var simpleDescription: String { get }
mutating func adjust()
}
class SimpleClass: ExampleProtocol {
var simpleDescription: String = "A very simple class."
var anotherProperty: Int = 69105
func adjust() {
simpleDescription += " Now 100% adjusted."
}
}
let a = SimpleClass()
a.adjust()
print(a.simpleDescription)

struct SimpleStructure: ExampleProtocol {
var simpleDescription: String = "A simple structure"
mutating func adjust() {
simpleDescription += " (adjusted)"
}
}
var b = SimpleStructure()
b.adjust()
print(b.simpleDescription)
```
SimpleStructure 中使用关键字 `mutating` 标记会修改结构体的方法。类不需要，因为类中的方法总是可以修改类。

使用拓展 `extension` 向已存在的类型中添加功能，包括新方法或者计算型属性。可以使用拓展来遵循协议。
```
extension Int: ExampleProtocol {
var simpleDescription: String {
return "The number \(self)"
}
mutating func adjust() {
self += 42
}
}
print(7.simpleDescription)
```
可以像使用其他类型一样使用协议。
```
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)
```
尽管 `a` 是一个类，但是编译器将 `protocolValue` 看作 `ExampleProtocol` 类型，所以不能访问协议以外的方法或者属性。

## 错误处理 (Error Handling)
使用 `Error`  协议定义错误类型。
```
enum PrinterError: Error {
case outOfPaper
case noToner
case onFire
}
```
使用 `throws` 标记一个可能会引发错误的函数，并使用 `throw` 抛出错误。调用函数的代码需要处理错误。
```
func send(job: Int, toPrinter printerName: String) throws -> String {
if printerName == "Never Has Toner" {
throw PrinterError.noToner
}
return "Job sent"
}
```
处理错误的一种方式是使用 `do-catch`。在 `do` 代码块中，使用 `try` 标记可能引发错误的代码，`catch` 代码块中默认给出错误，名称为 `error`，也可以自定义错误名称。
```
do {
let printerResponse = try send(job: 100, toPrinter: "Never Has Toner")
print(printerResponse)
} catch {
print(error)
}
```
也可以写多个 `catch` 语句，匹配不同的错误。
```
do {
let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
print(printerResponse)
} catch PrinterError.onFire {
print("I'll just put this over here, with the rest of the fire.")
} catch let printerError as PrinterError {
print("Printer error: \(printerError).")
} catch {
print(error)
}
```
另一种处理错误的方式是使用 `try?`。将结果转换为可选值，当有错误抛出时结果为  `nil`。
```
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
let printerFailure = try? send(job: 885, toPrinter: "Never Has Toner")
```

使用 `defer` 定义一个代码块，在函数 `return` 前执行。比如写完初始设置代码，可以马上使用 `defer` 写清理代码，他们会在不同的时机执行。
```
var fridgeIsOpen = false
let fridgeContent = ["milk", "eggs", "leftovers"]
func fridgeContains(_ food: String) -> Bool {
fridgeIsOpen = true
defer {
fridgeIsOpen = false
}
let result = fridgeContent.contains(food)
return result
}
fridgeContains("banana")
print(fridgeIsOpen)
```

## 范型 (Generics)
使用尖括号加名字，定义一个范型函数或者范型类型。
```
func makeArray<Item>(repeat item: Item, numberOfTimes: Int) -> [Item] {
var result = [Item]()
for _ in 0..<numerOfTimes {
result.append(item)
}
return result
}
makeArray(repeating: "knock", numberOfTimes: 4)
```
可以创建范型函数、方法，或者范型类、枚举和结构体。
```
enum OptionalValue<Wrapped> {
case none
case some(Wapped)
}
var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
```
在语句体的 `{}` 之前，可以使用 `where` 指定要求。比如要求类型遵循某协议，要求两种类型相同，或者要求某个类拥有特定的父类。
```
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool where T.Iterator.Element: Equatable, T.Iterator.Element == U.Iterator.Element {
for lhsItem in lhs {
for rhsItem in rhs {
if lhsItem == rhsItem {
return true
}
}
}
return false
}
anyCommonElements([1, 2, 3], [3])
```
```
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> [T.Element] where T.Iterator.Element: Equatable, T.Iterator.Element == U.Iterator.Element {
var commenElements: [T.Element] = []
for lhsItem in lhs {
if !commenElements.contains(lhsItem) {
for rhsItem in rhs {
if !commenElements.contains(rhsItem) {
if lhsItem == rhsItem {
commenElements.append(rhsItem)
}
}
}
}
}
return commenElements
}
anyCommonElements([1, 3, 2, 3], [2, 3, 2, 5])
```
`<T: Equatable>` 和 `<T> ... where T: Equatable` 是等价的。

---
本文是 [Apple 官方 Swift 教程 The Swift Programming Language (Swift 4)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html#//apple_ref/doc/uid/TP40014097-CH3-ID0) 的个人阅读笔记，并不是严谨的翻译文档。可能存在内容不准确或者更新不及时的情况。学习 Swift 请阅读官方教程，或官方推荐的 [中文版翻译](https://github.com/numbbbbb/the-swift-programming-language-in-chinese)
