# 字符串和字符（Strings and Characters）
---

> 1.0
> 翻译：[wh1100717](https://github.com/wh1100717)
> 校对：[Hawstein](https://github.com/Hawstein)

> 2.0
> 翻译+校对：[DianQK](https://github.com/DianQK)

本页包含内容：

- [字符串字面量](#string_literals)
- [初始化空字符串](#initializing_an_empty_string)
- [字符串可变性](#string_mutability)
- [字符串是值类型](#strings_are_value_types)
- [使用字符](#working_with_characters)
- [连接字符串和字符](#concatenating_strings_and_characters)
- [字符串插值](#string_interpolation)
- [Unicode](#unicode)
- [计算字符数量](#counting_characters)
- [访问和修改字符串](#accessing_and_modifying_a_string)
- [比较字符串](#comparing_strings)
- [字符串的 Unicode 表示形式](#unicode_representations_of_strings)


`String`是例如"hello, world"，"albatross"这样的有序的`Character`（字符）类型的值的集合，通过`String`类型来表示。
Swift 的`String`和`Character`类型提供了一个快速的，兼容 Unicode 的方式来处理代码中的文本。
创建和操作字符串的语法与 C 语言中字符串操作相似，轻量并且易读。
字符串连接操作只需要简单地通过`+`符号将两个字符串相连即可。
与 Swift 中其他值一样，能否更改字符串的值，取决于其被定义为常量还是变量。
尽管语法简易，但`String`类型是一种快速、现代化的字符串实现。
每一个字符串都是由编码无关的 Unicode 字符组成，并支持访问字符的多种 Unicode 表示形式（representations）。
你也可以在常量、变量、字面量和表达式中进行字符串插值操作，这可以帮助你轻松创建用于展示、存储和打印的自定义字符串。

> 注意：  
> Swift 的`String`类型与 Foundation `NSString`类进行了无缝桥接。就像 [`AnyObject`类型](./19_Type_Casting.html#anyobject) 中提到的一样，在使用 Cocoa 中的 Foundation 框架时，您可以将创建的任何字符串的值转换成`NSString`，并调用任意的`NSString` API。您也可以在任意要求传入`NSString`实例作为参数的 API 中用`String`类型的值代替。
> 更多关于在 Foundation 和 Cocoa 中使用`String`的信息请查看 *[Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html#//apple_ref/doc/uid/TP40014216)*。  



<a name="string_literals"></a>
## 字符串字面量（String Literals）

您可以在您的代码中包含一段预定义的字符串值作为字符串字面量。字符串字面量是由双引号 ("") 包裹着的具有固定顺序的文本字符集。
字符串字面量可以用于为常量和变量提供初始值：

```swift
let someString = "Some string literal value"
```

注意`someString`常量通过字符串字面量进行初始化，Swift 会推断该常量为`String`类型。
> 注意：
> 更多关于在字面量的特殊字符，请查看 [Special Characters in String Literals](#special_characters_in_string_literals) 。


<a name="initializing_an_empty_string"></a>
## 初始化空字符串 (Initializing an Empty String)

要创建一个空字符串作为初始值，可以将空的字符串字面量赋值给变量，也可以初始化一个新的`String`实例：

```swift
var emptyString = ""               // 空字符串字面量
var anotherEmptyString = String()  // 初始化方法
// 两个字符串均为空并等价。
```

您可以通过检查其`Boolean`类型的`isEmpty`属性来判断该字符串是否为空：

```swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
// 打印输出："Nothing to see here"
```

<a name="string_mutability"></a>
## 字符串可变性 (String Mutability)

您可以通过将一个特定字符串分配给一个变量来对其进行修改，或者分配给一个常量来保证其不会被修改：

```swift
var variableString = "Horse"
variableString += " and carriage"
// variableString 现在为 "Horse and carriage"
let constantString = "Highlander"
constantString += " and another Highlander"
// 这会报告一个编译错误 (compile-time error) - 常量不可以被修改。
```

> 注意：
> 在 Objective-C 和 Cocoa 中，您需要通过选择两个不同的类(`NSString`和`NSMutableString`)来指定该字符串是否可以被修改。

<a name="strings_are_value_types"></a>
## 字符串是值类型（Strings Are Value Types）

Swift 的`String`类型是值类型。
如果您创建了一个新的字符串，那么当其进行常量、变量赋值操作，或在函数/方法中传递时，会进行值拷贝。
任何情况下，都会对已有字符串值创建新副本，并对该新副本进行传递或赋值操作。
值类型在 [结构体和枚举是值类型](./09_Classes_and_Structures.html#structures_and_enumerations_are_value_types) 中进行了详细描述。

> 注意：  
> 与 Cocoa 中的`NSString`不同，当您在 Cocoa 中创建了一个`NSString`实例，并将其传递给一个函数/方法，或者赋值给一个变量，您传递或赋值的是该`NSString`实例的一个引用，除非您特别要求进行值拷贝，否则字符串不会生成新的副本来进行赋值操作。

Swift 默认字符串拷贝的方式保证了在函数/方法中传递的是字符串的值。
很明显无论该值来自于哪里，都是您独自拥有的。
您可以放心您传递的字符串本身不会被更改。

在实际编译时，Swift 编译器会优化字符串的使用，使实际的复制只发生在绝对必要的情况下，这意味着您将字符串作为值类型的同时可以获得极高的性能。

<a name="working_with_characters"></a>
## 使用字符（Working with Characters）

您可通过`for-in`循环来遍历字符串中的`characters`属性来获取每一个字符的值：

```swift
for character in "Dog!🐶".characters {
    print(character)
}
// D
// o
// g
// !
// 🐶
```

for-in 循环在 [For Loops](./05_Control_Flow.html#for_loops) 中进行了详细描述。

另外，通过标明一个`Character`类型并用字符字面量进行赋值，可以建立一个独立的字符常量或变量：

```swift
let exclamationMark: Character = "!"
```
字符串可以通过传递一个值类型为`Character`的数组作为自变量来初始化：

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// 打印输出："Cat!🐱"
```

<a name="concatenating_strings_and_characters"></a>
## 连接字符串和字符 (Concatenating Strings and Characters)

字符串可以通过加法运算符（`+`）相加在一起（或称“连接”）创建一个新的字符串：

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 +　string2
// welcome 现在等于 "hello there"
```

您也可以通过加法赋值运算符 (`+=`) 将一个字符串添加到一个已经存在字符串变量上：

```swift
var instruction = "look over"
instruction += string2
// instruction 现在等于　"look over there"
```

您可以用`append`方法将一个字符附加到一个字符串变量的尾部：

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
// welcome 现在等于 "hello there!"
```

> 注意：  
> 您不能将一个字符串或者字符添加到一个已经存在的字符变量上，因为字符变量只能包含一个字符。


<a name="string_interpolation"></a>
## 字符串插值 (String Interpolation)

字符串插值是一种构建新字符串的方式，可以在其中包含常量、变量、字面量和表达式。
您插入的字符串字面量的每一项都在以反斜线为前缀的圆括号中：

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message 是 "3 times 2.5 is 7.5"
```

在上面的例子中，`multiplier`作为`\(multiplier)`被插入到一个字符串常量量中。
当创建字符串执行插值计算时此占位符会被替换为`multiplier`实际的值。

`multiplier`的值也作为字符串中后面表达式的一部分。
该表达式计算`Double(multiplier) * 2.5`的值并将结果 (7.5) 插入到字符串中。
在这个例子中，表达式写为`\(Double(multiplier) * 2.5)`并包含在字符串字面量中。

> 注意：  
> 插值字符串中写在括号中的表达式不能包含非转义双引号 (`"`) 和反斜杠 (`\`)，并且不能包含回车或换行符。


<a name="unicode"></a>
## Unicode

Unicode 是一个国际标准，用于文本的编码和表示。
它使您可以用标准格式表示来自任意语言几乎所有的字符，并能够对文本文件或网页这样的外部资源中的字符进行读写操作。
Swift 的字符串和字符类型是完全兼容 Unicode 标准的。  

<a name="unicode_scalars"></a>
### Unicode 标量（Unicode Scalars）

Swift 的`String`类型是基于 *Unicode 标量* 建立的。
Unicode 标量是对应字符的唯一21位数字或者修饰符，例如`U+0061`表示小写的拉丁字母(`LATIN SMALL LETTER A`)("`a`")，`U+1F425`表示小鸡表情(`FRONT-FACING BABY CHICK`) ("`🐥`")
> 注意：
> Unicode *码位(code poing)* 的范围是`U+0000`到`U+D7FF`或者`U+E000`到`U+10FFFF`。Unicode 标量不包括 Unicode *代理项(surrogate pair)* 码位，其码位范围是`U+D800`到`U+DFFF`。

注意不是所有的21位 Unicode 标量都代表一个字符，因为有一些标量是保留给未来分配的。已经代表一个典型字符的标量都有自己的名字，例如上面例子中的`LATIN SMALL LETTER A`和`FRONT-FACING BABY CHICK`。

<a name="special_characters_in_string_literals"></a>
### 字符串字面量的特殊字符 (Special Characters in String Literals)

字符串字面量可以包含以下特殊字符：

* 转义字符`\0`(空字符)、`\\`(反斜线)、`\t`(水平制表符)、`\n`(换行符)、`\r`(回车符)、`\"`(双引号)、`\'`(单引号)。
* Unicode 标量，写成`\u{n}`(u为小写)，其中`n`为任意一到八位十六进制数且可用的 Unicode 位码。

下面的代码为各种特殊字符的使用示例。
`wiseWords`常量包含了两个双引号；
`dollarSign`、`blackHeart`和`sparklingHeart`常量演示了三种不同格式的 Unicode 标量：

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imageination is more important than knowledge" - Enistein
let dollarSign = "\u{24}"             // $, Unicode 标量 U+0024
let blackHeart = "\u{2665}"           // ♥, Unicode 标量 U+2665
let sparklingHeart = "\u{1F496}"      // 💖, Unicode 标量 U+1F496
```

<a name="extended_grapheme_clusters"></a>  
### 可扩展的字形群集(Extended Grapheme Clusters)
每一个 Swift 的`Character`类型代表一个可扩展的字形群。
一个可扩展的字形群是一个或者更多可生成人类可读的字符 Unicode 标量的有序排列。
举个例子，字母 é 可以用单一的 Unicode 标量 é (`LATIN SMALL LETTER E WITH ACUTE`, 或者`U+00E9`)来表示。然而一个标准的字母 e (`LATIN SMALL LETTER E`或者`U+0065`) 加上一个急促重音(`COMBINING ACTUE ACCENT`)的标量(`U+0301`)，这样一对标量就表示了同样的字母 é。
这个急促重音的标量形象的将 e 转换成了 é。
在这两种情况中，字母 é 代表了一个单一的 Swift 的字符串，同时代表了一个可扩展的字形群。
在第一种情况，这个字形群包含一个单一标量；而在第二种情况，它是包含两个标量的字形群：

```swift
let eAcute: Character = "\u{E9}"                         // é
let combinedEAcute: Character = "\u{65}\u{301}"          // e 后面加上  ́
// eAcute 是 é, combinedEAcute 是 é
```

可扩展的字符群集是一个灵活的方法，用许多复杂的脚本字符表示单一字符。
例如，来自朝鲜语字母表的韩语音节能表示为组合或分解的有序排列。
在 Swift 都会表示为同一个单一的字符：


```swift
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ
// precomposed 是 한, decomposed 是 한
```

可拓展的字符群集可以使包围记号(例如`COMBINING ENCLOSING CIRCLE`或者`U+20DD`)的标量包围其他 Unicode 标量，作为一个单一的字符：

```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"
// enclosedEAcute 是 é⃝
```  

局部的指示符号的 Unicode 标量可以组合成一个单一的字符，例如 `REGIONAL INDICATOR SYMBOL LETTER U`(`U+1F1FA`)和`REGIONAL INDICATOR SYMBOL LETTER S`(`U+1F1F8`)：


```swift
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
// regionalIndicatorForUS 是 🇺🇸
```

<a name="counting_characters"></a>
## 计算字符数量 (Counting Characters)

如果想要获得一个字符串中字符的数量，可以使用字符串的characters属性的count属性：

```swift
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.characters.count) characters")
// 打印输出 "unusualMenagerie has 40 characters"
```

注意在 Swift 中，使用可拓展的字符群集作为字符来连接或改变字符串时，并不一定会更改字符串的字符数量。

例如，如果你用四个字符的单词 cafe 初始化一个新的字符串，然后添加一个 `COMBINING ACTUE ACCENT`(`U+0301`)作为字符串的结尾。最终这个字符串的字符数量仍然是4，因为第四个字符是 é ，而不是 e ：

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.characters.count)")
// 打印输出 "the number of characters in cafe is 4"
     
word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301
     
print("the number of characters in \(word) is \(word.characters.count)")
// 打印输出 "the number of characters in café is 4"
```

> 注意：  
> 可扩展的字符群集可以组成一个或者多个 Unicode 标量。这意味着不同的字符以及相同字符的不同表示方式可能需要不同数量的内存空间来存储。所以 Swift 中的字符在一个字符串中并不一定占用相同的内存空间数量。因此在没有获取字符串的可扩展的字符群的范围时候，就不能计算出字符串的字符数量。如果您正在处理一个长字符串，需要注意`count(_:)`函数必须遍历全部的 Unicode 标量，来确定字符串的字符数量。
> 
> 另外需要注意的是通过`count(_:)`返回的字符数量并不总是与包含相同字符的`NSString`的`length`属性相同。`NSString`的`length`属性是利用 UTF-16 表示的十六位代码单元数字，而不是 Unicode 可扩展的字符群集。作为佐证，当一个`NSString`的`length`属性被一个Swift的`String`值访问时，实际上是调用了`utf16Count`。


<a name="accessing_and_modifying_a_string"></a>
## 访问和修改字符串 (Accessing and Modifying a String)
你可以通字符串的属性和方法来访问和读取一个它，当然也可以用下标语法完成。

<a name="string_indices"></a>
### 字符串索引 (String Indices)
每一个字符串都有一个关联的索引(*index*)类型，`String.index`，它对应着字符串中的每一个字符的位置。

前面提到，不同的字符可能会占用不同的内存空间数量，所以要知道字符的确定位置，就必须从字符串开头遍历每一个 Unicode 标量到字符串结尾。因此，Swift 的字符串不能用整数(integer)做索引。

使用`startIndex`属性可以获取字符串的第一个字符。使用`endIndex`属性可以获取最后的位置（译者注：其实endIndex在值上与字符串的长度相等）。如果字符串是空值，`startIndex`和`endIndex`是相等的。

```swift
let greeting = "Guten Tag"
println(greeting.startIndex)
// 0
println(greeting.endIndex)
// 9
```

你可以通过下表来获得`String`对应位置的`Character`：

```swift
greeting[greeting.startIndex]
// G
```

通过调用`String.Index`的`predecessor()`方法，可以立即得到前面一个索引，调用`successor()`方法可以立即得到后面一个索引。任何一个字符串的索引都可以通过锁链作用的这些方法来获取另一个索引，也可以调用`advance(start:n:)`函数来获取。但如果尝试获取出界的字符串索引，就会抛出一个运行时错误。
你可以使用下标语法来访问字符在字符串的确切索引。尝试获取出界的字符串索引，仍然抛出一个运行时错误。

```swift
let greeting = "Guten Tag"
greeting[greeting.startIndex]
// G
greeting[greeting.endIndex.predecessor()]
// g
greeting[greeting.startIndex.successor()]
// u
let index = advance(greeting.startIndex, 7)
greeting[index]
// a
greeting[greeting.endIndex] // 错误
greeting.endIndex.successor() // 错误
```

使用全局函数`indices`会创建一个包含全部索引的范围(`Range`)，用来在一个字符串中访问分立的字符。  

```swift
for index in greeting.characters.indices {
   print("\(greeting[index]) ", appendNewline: false)
}
// prints "G u t e n   T a g !"
```

<a name="inserting_and_removing"></a>
### 插入和删除 (Inserting and Removing)
调用`insert(_:atIndex:)`方法可以在一个字符串的指定索引插入一个字符。

```swift
var welcome = "hello"
welcome.insert("!", atIndex: welcome.endIndex)
// welcome now 现在等于 "hello!"
```

调用`splice(_:atIndex:)`方法可以在一个字符串的指定索引插入一个字符串。

```swift
welcome.splice(" there".characters, atIndex: welcome.endIndex.predecessor())
// welcome 现在等于 "hello there!"
```

调用`removeAtIndex(_:)`方法可以在一个字符串的指定索引删除一个字符。

```swift
welcome.removeAtIndex(welcome.endIndex.predecessor())
// welcome 现在等于 "hello there"
// 翻译的人解释：最后还有一个换行符，所以这里删除的是 !
```

调用`removeRange(_:)`方法可以在一个字符串的指定索引删除一个子字符串。

```swift
let range = advance(welcome.endIndex, -6)..<welcome.endIndex
welcome.removeRange(range)
// welcome 现在等于 "hello"
```


<a name="comparing_strings"></a>
## 比较字符串 (Comparing Strings)

Swift 提供了三种方式来比较文本值：字符串字符相等、前缀相等和后缀相等。

<a name="string_and_character_equality"></a>
### 字符串/字符相等 (String and Character Equality)
字符串/字符可以用等于操作符(`==`)和不等于操作符(`!=`)，详细描述在[比较运算符](./02_Basic_Operators.html#comparison_operators)：

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// 打印输出 "These two strings are considered equal"
```

如果两个字符串（或者两个字符）的可扩展的字形群集是标准相等的，那就认为它们是相等的。在这个情况下，即使可扩展的字形群集是有不同的 Unicode 标量构成的，只要它们有同样的语言意义和外观，就认为它们标准相等。

例如，`LATIN SMALL LETTER E WITH ACUTE`(`U+00E9`)就是标准相等于`LATIN SMALL LETTER E`(`U+0065`)后面加上`COMBINING ACUTE ACCENT`(`U+0301`)。这两个字符群集都有效的表示字符 é ，所以它们被认为是标准相等的：

```swift
// "Voulez-vous un café?" 使用 LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"
// "Voulez-vous un café?" 使用 LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"
if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// 打印输出 "These two strings are considered equal"
```

相反，英语中的`LATIN CAPITAL LETTER A`(`U+0041`，或者`A`)不等于俄语中的`CYRILLIC CAPITAL LETTER A`(`U+0410`，或者`A`)。两个字符看着是一样的，但却有不同的语言意义：

```swift
let latinCapitalLetterA: Character = "\u{41}"
let cyrillicCapitalLetterA: Character = "\u{0410}"
if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters are not equivalent")
}
// 打印 "These two characters are not equivalent"
```

> 注意：
> 在 Swift 中，字符串和字符并不区分区域。


<a name="prefix_and_suffix_equality"></a>
### 前缀/后缀相等 (Prefix and Suffix Equality)

通过调用字符串的`hasPrefix(_:)`/`hasSuffix(_:)`方法来检查字符串是否拥有特定前缀/后缀，两个方法均需要以字符串作为参数传入并传出`Boolean`值。
下面的例子以一个字符串数组表示莎士比亚话剧《罗密欧与朱丽叶》中前两场的场景位置：

```swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```

您可以调用`hasPrefix(_:)`方法来计算话剧中第一幕的场景数：

```swift
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        ++act1SceneCount
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// 打印输出 "There are 5 scenes in Act 1"
```

相似地，您可以用`hasSuffix(_:)`方法来计算发生在不同地方的场景数：

```swift
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        ++mansionCount
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        ++cellCount
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// 打印输出 "6 mansion scenes; 2 cell scenes"
```

> 注意：
> `hasPrefix(_:)`和`hasSuffix(_:)`方法都是在每个字符串中一个一个字符的比较其可扩展的字符群集是否标准相等，详细描述在[字符串/字符相等](#string_and_character_equality)。


<a name="unicode_representations_of_strings"></a>
## 字符串的 Unicode 表示形式（Unicode Representations of Strings）
当一个 Unicode 字符串被写进文本文件或者其他储存时，字符串中的 Unicode 标量会用 Unicode 定义的几种编码格式编码。每一个字符串中的小块编码都被称为代码单元。这些包括 UTF-8 编码格式（编码字符串为8位的代码单元）， UTF-16 编码格式（编码字符串位16位的代码单元），以及 UTF-32 编码格式（编码字符串32位的代码单元）。

Swift 提供了几种不同的方式来访问字符串的 Unicode 表示形式。
您可以利用`for-in`来对字符串进行遍历，从而以 Unicode 可扩展的字符群集的方式访问每一个字符值。
该过程在 [使用字符](#working_with_characters) 中进行了描述。

另外，能够以其他三种 Unicode 兼容的方式访问字符串的值：

* UTF-8 代码单元集合 (利用字符串的`utf8`属性进行访问)
* UTF-16 代码单元集合 (利用字符串的`utf16`属性进行访问)
* 21位的 Unicode 标量值集合，也就是字符串的 UTF-32 编码格式 (利用字符串的`unicodeScalars`属性进行访问)

下面由`D``o``g``‼`(`DOUBLE EXCLAMATION MARK`, Unicode 标量 `U+203C`)和`🐶`(`DOG FACE`，Unicode 标量为`U+1F436`)组成的字符串中的每一个字符代表着一种不同的表示：

```swift
let dogString = "Dog‼🐶"
```


<a name="UTF-8_representation"></a>
### UTF-8 表示
您可以通过遍历字符串的`utf8`属性来访问它的`UTF-8`表示。
其为`String.UTF8View`类型的属性，`UTF8View`是无符号8位 (`UInt8`) 值的集合，每一个`UInt8`值都是一个字符的 UTF-8 表示：

<table style='text-align:center'>
 <tr height=77>
  <td>Character</td>
  <td>D<br>U+0044</td>
  <td>o<br>U+006F</td>
  <td>g<br>U+0067</td>
  <td colspan=3>‼<br>U+203C</td>
  <td colspan=4>🐶<br>U+1F436</td>
 </tr>
 <tr height=77>
  <td height=77>UTF-8<br>Code Unit</td>
  <td>68</td>
  <td>111</td>
  <td>103</td>
  <td>226</td>
  <td>128</td>
  <td>188</td>
  <td>240</td>
  <td>159</td>
  <td>144</td>
  <td>182</td>
 </tr>
 <tr>
  <td height=77>Position</td>
  <td>0</td>
  <td>1</td>
  <td>2</td>
  <td>3</td>
  <td>4</td>
  <td>5</td>
  <td>6</td>
  <td>7</td>
  <td>8</td>
  <td>9</td>
 </tr>
</table>


```swift
for codeUnit in dogString.utf8 {
   print("\(codeUnit) ", appendNewline: false)
}
print("")
// 68 111 103 226 128 188 240 159 144 182
```

上面的例子中，前三个10进制代码单元值 (68, 111, 103) 代表了字符`D`、`o`和 `g`，它们的 UTF-8 表示与 ASCII 表示相同。
接下来的三个10进制代码单元值 (226, 128, 188) 是`DOUBLE EXCLAMATION MARK`的3字节 UTF-8 表示。
最后的四个代码单元值 (240, 159, 144, 182) 是`DOG FACE`的4字节 UTF-8 表示。


<a name="UTF-16_representation"></a>
### UTF-16 表示

您可以通过遍历字符串的`utf16`属性来访问它的`UTF-16`表示。
其为`String.UTF16View`类型的属性，`UTF16View`是无符号16位 (`UInt16`) 值的集合，每一个`UInt16`都是一个字符的 UTF-16 表示：

<table style='text-align:center'>
 <tr height=77>
  <td>Character</td>
  <td>D<br>U+0044</td>
  <td>o<br>U+006F</td>
  <td>g<br>U+0067</td>
  <td>‼<br>U+203C</td>
  <td colspan=2>🐶<br>U+1F436</td>
 </tr>
 <tr height=77>
  <td height=77>UTF-16<br>Code Unit</td>
  <td>68</td>
  <td>111</td>
  <td>103</td>
  <td>8252</td>
  <td>55357</td>
  <td>56374</td>
 </tr>
 <tr>
  <td height=77>Position</td>
  <td>0</td>
  <td>1</td>
  <td>2</td>
  <td>3</td>
  <td>4</td>
  <td>5</td>
 </tr>
</table>


```swift
for codeUnit in dogString.utf16 {
    print("\(codeUnit) ")
}
print("\n")
// 68 111 103 8252 55357 56374
```

同样，前三个代码单元值 (68, 111, 103) 代表了字符`D`、`o`和`g`，它们的 UTF-16 代码单元和 UTF-8 完全相同（因为这些 Unicode 标量表示 ASCII 字符）。

第四个代码单元值 (8252) 是一个等于十六进制203C的的十进制值。这个代表了`DOUBLE EXCLAMATION MARK`字符的 Unicode 标量值`U+203C`。这个字符在 UTF-16 中可以用一个代码单元表示。

第五和第六个代码单元值 (55357 和 56374) 是`DOG FACE`字符的UTF-16 表示。
第一个值为`U+D83D`(十进制值为 55357)，第二个值为`U+DC36`(十进制值为 56374)。

<a name="unicode_scalars_representation"></a>
### Unicode 标量表示 (Unicode Scalars Representation)

您可以通过遍历字符串的`unicodeScalars`属性来访问它的 Unicode 标量表示。
其为`UnicodeScalarView`类型的属性， `UnicodeScalarView`是`UnicodeScalar`的集合。
`UnicodeScalar`是21位的 Unicode 代码点。

每一个`UnicodeScalar`拥有一个值属性，可以返回对应的21位数值，用`UInt32`来表示：


<table style='text-align:center'>
 <tr height=77>
  <td>Character</td>
  <td>D<br>U+0044</td>
  <td>o<br>U+006F</td>
  <td>g<br>U+0067</td>
  <td>‼<br>U+203C</td>
  <td>🐶<br>U+1F436</td>
 </tr>
 <tr height=77>
  <td height=77>UTF-16<br>Code Unit</td>
  <td>68</td>
  <td>111</td>
  <td>103</td>
  <td>8252</td>
  <td>128054</td>
 </tr>
 <tr>
  <td height=77>Position</td>
  <td>0</td>
  <td>1</td>
  <td>2</td>
  <td>3</td>
  <td>4</td>
 </tr>
</table>


```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ")
}
print("\n")
// 68 111 103 8252 128054
```

前三个代码单元值 (68, 111, 103) 仍然代表字符`D`、`o`和`g`。
第四个代码单元值 (8252) 仍然是一个等于十六进制203C的的十进制值。这个代表了`DOUBLE EXCLAMATION MARK`字符的 Unicode 标量`U+203C`。

第五位数值，128054，是一个十六进制1F436的十进制表示。其等同于`DOG FACE`的Unicode 标量`U+1F436`。

作为查询字符值属性的一种替代方法，每个`UnicodeScalar`值也可以用来构建一个新的字符串值，比如在字符串插值中使用：

```swift
for scalar in dogString.unicodeScalars {
    print("\(scalar) ")
}
// D
// o
// g
// ‼
// 🐶
```
