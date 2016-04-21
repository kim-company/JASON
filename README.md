<br/>
<h3 align="center">JASON</h3>
<br/>
<br/>

<p align="center">
    <a href="https://travis-ci.org/delba/JASON"><img alt="Travis Status" src="https://img.shields.io/travis/delba/JASON.svg"/></a>
    <a href="https://img.shields.io/cocoapods/v/JASON.svg"><img alt="CocoaPods compatible" src="https://img.shields.io/cocoapods/v/JASON.svg"/></a>
    <a href="https://github.com/Carthage/Carthage"><img alt="Carthage compatible" src="https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat"/></a>
</p>

**`JASON`** is a faster JSON deserializer written in Swift.

```md
JASON is the best framework we found to manage JSON at Swapcard. This is by far the fastest and
the most convenient out there, it made our code clearer and improved the global performance
of the app when dealing with large amout of data.
```
> *[Gautier Gédoux](https://github.com/gautier-gdx), lead iOS developer at [Swapcard](https://www.swapcard.com/)*

<p align="center">
<a href="#features">Features</a> • <a href="#usage">Usage</a> • <a href="#example">Example</a> • <a href="#references">References</a> • <a href="#installation">Installation</a> • <a href="#license">License</a>
</p>

## Features

- [x] Very fast
- [x] Fully tested
- [x] Fully documented
<p></p>
- [x] Clean code
- [x] Beautiful API
- [x] Regular updates
<p></p>
- [x] Support for iOS, OSX, tvOS, watchOS
- [x] Compatible with Carthage/CocoaPods
- [x] Provide extensions

## Usage

#### Initialization

```swift
let json = JSON(anything) // where `anything` is `AnyObject?`
```

If you're using [`Alamofire`](https://github.com/Alamofire/Alamofire), include [`JASON+Alamofire.swift`](https://github.com/delba/JASON/blob/master/Extensions/JASON%2BAlamofire.swift) in your project for even more awesomeness:

```swift
Alamofire.request(.GET, peopleURL).responseJASON { response in
    if let json = response.result.value {
        let people = json.map(Person.init)
        print("people: \(people)")
    }
}
```

#### Parsing

Use subscripts to parse the `JSON` object:

```swift
json["people"][0]["name"]
```

Alternatively, you can use a path:

```swift
json[path: "people", 0, "name"]
```

#### Casting the value

Cast `JSON` value to its appropriate type by using the computed property `json.<type>`:

```swift
let name = json["name"].string // the name as String?
```

The non-optional variant `json.<type>Value` will return a default value if not present/convertible:

```swift
let name = json["wrong"].string // the name will be ""
```

You can also access the internal value as `AnyObject?` if you want to cast it yourself:

```swift
let something = json["something"].object
```

*See the [References section](https://github.com/delba/JASON#references) for the full list of properties.*

#### `JSONKey`

## Example

```swift
extension JSONKeys {
    static let id    = JSONKey<Int>("id")
    static let title = JSONKey<String>("title")
    
    static let normalImageURL = JSONKey<NSURL?>(path: "images", "normal")
    static let hidpiImageURL  = JSONKey<NSURL?>(path: "images", "hidpi")
    
    static let user = JSONKey<JSON>("user")
    static let name = JSONKey<String>("name") 
}
```

```swift
struct Shot {
    let id: Int
    let title: String
    
    var normalImageURL: NSURL!
    var hidpiImageURL: NSURL?
    
    let user: User

    init(_ json: JSON) {
        id    = json[.id]
        title = json[.title]
        
        normalImageURL = json[.normalImageURL]
        hidpiImageURL  = json[.hidpiImageURL]
        
        user = User(json[.user])
    }
}
```

```swift
struct User {
    let id: Int
    let name: String

    init(_ json: JSON) {
        id   = json[.id]
        name = json[.name]
    }
}
```

## References

Property              | JSONKey Type           | Default value
--------------------- | ---------------------- | -------------
`json`                | `JSON`                 |
`string`              | `String?`              |
`stringValue`         | `String`               | `""`
`int`                 | `Int?`                 |
`intValue`            | `Int`                  | `0`
`double`              | `Double?`              |
`doubleValue`         | `Double`               | `0.0`
`float`               | `Float?`               |
`floatValue`          | `Float`                | `0.0`
`cgFloat`             | `CGFloat?`             |
`cgFloatValue`        | `CGFloat`              | `0.0`
`bool`                | `Bool?`                |
`boolValue`           | `Bool`                 | `false`
`nsURL`               | `NSURL?`               |
`dictionary`          | `[String: AnyObject]?` |
`dictionaryValue`     | `[String: AnyObject]`  | `[:]`
`jsonDictionary`      | `[String: JSON]?`      |
`jsonDictionaryValue` | `[String: JSON]`       | `[:]`
`nsDictionary`        | `NSDictionary?`        |
`nsDictionaryValue`   | `NSDictionary`         | `NSDictionary()`
`array`               | `[AnyObject]?`         |
`arrayValue`          | `[AnyObject]`          | `[]`
`jsonArray`           | `[JSON]?`              |
`jsonArrayValue`      | `[JSON]`               | `[]`
`nsArray`             | `NSArray?`             |
`nsArrayValue`        | `NSArray`              | `NSArray()`

> Include [`Extensions/JASON+Properties.swift`](https://github.com/delba/JASON/blob/master/Extensions/JASON%2BProperties.swift) for even more types

## Installation

#### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that automates the process of adding frameworks to your Cocoa application.

You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

```bash
$ brew update
$ brew install carthage
```

To integrate **`JASON`** into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "delba/JASON" >= 2.0
```

#### CocoaPods

[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects.

You can install it with the following command:

```bash
$ gem install cocoapods
```

To integrate **`JASON`** into your Xcode project using CocoaPods, specify it in your `Podfile`:

```ruby
use_frameworks!

pod 'JASON', '~> 2.0'
```

## License

Copyright (c) 2015-2016 Damien (http://delba.io)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
