---
layout: default
title: Conventions
parent: API Design Guidelines
nav_order: 3
---


# Conventions

## General Conventions

<details>
<summary>General Conventions 원문보기</summary>
<div markdown="1">

- **Document the complexity of any computed property that is not O(1).** People often assume that property access involves no significant computation, because they have stored properties as a mental model. Be sure to alert them when that assumption may be violated.

- **Prefer methods and properties to free functions.** Free functions are used only in special cases:
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    1. When there’s no obvious self:
        ```swift
        min(x, y, z)
        ```

    2. When the function is an unconstrained generic:
        ```swift
        print(x)
        ```

    3. When function syntax is part of the established domain notation:
        ```swift
        sin(x)
        ```

    </div>
    </details>

- **Follow case conventions.** Names of types and protocols are UpperCamelCase. Everything else is lowerCamelCase.
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    [Acronyms and initialisms](https://en.wikipedia.org/wiki/Acronym) that commonly appear as all upper case in American English should be uniformly up- or down-cased according to case conventions:

    ```swift
    var utf8Bytes: [UTF8.CodeUnit]
    var isRepresentableAsASCII = true
    var userSMTPServer: SecureSMTPServer
    ```

    Other acronyms should be treated as ordinary words:

    ```swift
    var radarDetector: RadarScanner
    var enjoysScubaDiving = true
    ```

    </div>
    </details>

- **Methods can share a base name** when they share the same basic meaning or when they operate in distinct domains.
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    For example, the following is encouraged, since the methods do essentially the same things:

    ```swift
    ✅
    extension Shape {
        /// Returns `true` iff `other` is within the area of `self`.
        func contains(_ other: Point) -> Bool { ... }

        /// Returns `true` iff `other` is entirely within the area of `self`.
        func contains(_ other: Shape) -> Bool { ... }

        /// Returns `true` iff `other` is within the area of `self`.
        func contains(_ other: LineSegment) -> Bool { ... }
    }
    ```

    And since geometric types and collections are separate domains, this is also fine in the same program:

    ```swift
    ✅
    extension Collection where Element : Equatable {
        /// Returns `true` iff `self` contains an element equal to
        /// `sought`.
        func contains(_ sought: Element) -> Bool { ... }
    }
    ```

    However, these index methods have different semantics, and should have been named differently:

    ```swift
    ❌
    extension Database {
        /// Rebuilds the database's search index
        func index() { ... }

        /// Returns the `n`th row in the given table.
        func index(_ n: Int, inTable: TableID) -> TableRow { ... }
    }
    ```

    Lastly, avoid “overloading on return type” because it causes ambiguities in the presence of type inference.

    ```swift
    ❌
    extension Box {
        /// Returns the `Int` stored in `self`, if any, and
        /// `nil` otherwise.
        func value() -> Int? { ... }

        /// Returns the `String` stored in `self`, if any, and
        /// `nil` otherwise.
        func value() -> String? { ... }
    }
    ```

    </div>
    </details>

</div>
</details>

번역문

.

## Parameters

<details>
<summary>Parameters 원문보기</summary>
<div markdown="1">


```swift
func move(from start: Point, to end: Point)
```

- **Choose parameter names to serve documentation.** Even though parameter names do not appear at a function or method’s point of use, they play an important explanatory role.
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    Choose these names to make documentation easy to read. For example, these names make documentation read naturally:

    ```swift
    ✅
    /// Return an `Array` containing the elements of `self`
    /// that satisfy `predicate`.
    func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]

    /// Replace the given `subRange` of elements with `newElements`.
    mutating func replaceRange(_ subRange: Range, with newElements: [E])
    ```

    These, however, make the documentation awkward and ungrammatical:

    ```swift
    ❌
    /// Return an `Array` containing the elements of `self`
    /// that satisfy `includedInResult`.
    func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]

    /// Replace the range of elements indicated by `r` with
    /// the contents of `with`.
    mutating func replaceRange(_ r: Range, with: [E])
    ```

    </div>
    </details>

- **Take advantage of defaulted parameters** when it simplifies common uses. Any parameter with a single commonly-used value is a candidate for a default.
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    Default arguments improve readability by hiding irrelevant information. For example:

    ```swift
    ❌
    let order = lastName.compare(
    royalFamilyName, options: [], range: nil, locale: nil)
    ```

    can become the much simpler:

    ```swift
    ✅
    let order = lastName.compare(royalFamilyName)
    ```

    Default arguments are generally preferable to the use of method families, because they impose a lower cognitive burden on anyone trying to understand the API.

    ```swift
    ✅
    extension String {
        /// ...description...
        public func compare(
            _ other: String, options: CompareOptions = [],
            range: Range? = nil, locale: Locale? = nil
        ) -> Ordering
    }
    ```

    The above may not be simple, but it is much simpler than:

    ```swift
    ❌
    extension String {
        /// ...description 1...
        public func compare(_ other: String) -> Ordering
        /// ...description 2...
        public func compare(_ other: String, options: CompareOptions) -> Ordering
        /// ...description 3...
        public func compare(
            _ other: String, options: CompareOptions, range: Range) -> Ordering
        /// ...description 4...
        public func compare(
            _ other: String, options: StringCompareOptions,
            range: Range, locale: Locale) -> Ordering
    }
    ```

    Every member of a method family needs to be separately documented and understood by users. To decide among them, a user needs to understand all of them, and occasional surprising relationships—for example, foo(bar: nil) and foo() aren’t always synonyms—make this a tedious process of ferreting out minor differences in mostly identical documentation. Using a single method with defaults provides a vastly superior programmer experience.

    </div>
    </details>

- **Prefer to locate parameters with defaults toward the end** of the parameter list. Parameters without defaults are usually more essential to the semantics of a method, and provide a stable initial pattern of use where methods are invoked.

</div>
</details>

번역문

.

## Argument Labels

<details>
<summary>Argument Labels 원문보기</summary>
<div markdown="1">


```swift
func move(from start: Point, to end: Point)
x.move(from: x, to: y) 
```

- **Omit all labels when arguments can’t be usefully distinguished,** e.g. min(number1, number2), zip(sequence1, sequence2).

- **In initializers that perform value preserving type conversions, omit the first argument label,** e.g. Int64(someUInt32)
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    The first argument should always be the source of the conversion.

    ```swift
    extension String {
    // Convert `x` into its textual representation in the given radix
    init(_ x: BigInt, radix: Int = 10)   ← Note the initial underscore
    }

    text = "The value is: "
    text += String(veryLargeNumber)
    text += " and in hexadecimal, it's"
    text += String(veryLargeNumber, radix: 16)
    ```

    In “narrowing” type conversions, though, a label that describes the narrowing is recommended.

    ```swift
    extension UInt32 {
    /// Creates an instance having the specified `value`.
    init(_ value: Int16)            ← Widening, so no label
    /// Creates an instance having the lowest 32 bits of `source`.
    init(truncating source: UInt64)
    /// Creates an instance having the nearest representable
    /// approximation of `valueToApproximate`.
    init(saturating valueToApproximate: UInt64)
    }
    ```

    > A value preserving type conversion is a [monomorphism](https://en.wikipedia.org/wiki/Monomorphism), i.e. every difference in the value of the source results in a difference in the value of the result. For example, conversion from Int8 to Int64 is value preserving because every distinct Int8 value is converted to a distinct Int64 value. Conversion in the other direction, however, cannot be value preserving: Int64 has more possible values than can be represented in an Int8.
    > 
    > Note: the ability to retrieve the original value has no bearing on whether a conversion is value preserving.

    </div>
    </details>

- **When the first argument forms part of a [prepositional phrase](https://en.wikipedia.org/wiki/Adpositional_phrase#Prepositional_phrases), give it an argument label.** The argument label should normally begin at the [preposition](https://en.wikipedia.org/wiki/Preposition_and_postposition), e.g. x.removeBoxes(havingLength: 12).
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    An exception arises when the first two arguments represent parts of a single abstraction.

    ```swift
    ❌
    a.move(toX: b, y: c)
    a.fade(fromRed: b, green: c, blue: d)
    ```

    In such cases, begin the argument label after the preposition, to keep the abstraction clear.

    ```swift
    ✅
    a.moveTo(x: b, y: c)
    a.fadeFrom(red: b, green: c, blue: d)
    ```

    </div>
    </details>


- **Otherwise, if the first argument forms part of a grammatical phrase, omit its label,** appending any preceding words to the base name, e.g. x.addSubview(y)
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    This guideline implies that if the first argument doesn’t form part of a grammatical phrase, it should have a label.

    ```swift
    ✅
    view.dismiss(animated: false)
    let text = words.split(maxSplits: 12)
    let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
    ```

    Note that it’s important that the phrase convey the correct meaning. The following would be grammatical but would express the wrong thing.

    ```swift
    ❌
    view.dismiss(false)   Don't dismiss? Dismiss a Bool?
    words.split(12)       Split the number 12?
    ```

    Note also that arguments with default values can be omitted, and in that case do not form part of a grammatical phrase, so they should always have labels.

    </div>
    </details>

- **Label all other arguments.**

</div>
</details>

번역문
