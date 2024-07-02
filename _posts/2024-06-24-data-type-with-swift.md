---
title: "[Swift] 데이터 타입"
description: 
author: Enxec
date: 2024-06-24
categories: [Language, Swift]
tags: [swift, 스위프트, 데이터 타입, dataType]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/swift-logo.png
  lqip: 
  alt: 
---

프로그래밍에서 데이터 타입은 프로그래밍의 기본 요소로서 매우 중요한 역할을 한다. 이번 포스트에서는 스위프트의 데이터 타입에 대해 살펴보자.

## 기본 데이터 타입
---
Swift에는 몇가지 기본 데이터 타입이 존재하며, 내용은 다음과 같다.

### Int
- 정수를 표현하는 데 사용된다. 64비트 플랫폼에서는 64비트 정수, 32비트 플랫폼에서는 32비트 정수를 의미한다.

```swift
let age: Int = 25
```

### UInt
- 부호가 없는 정수이며, 음수를 허용하지 않고 양수만 표현할 수 있다.

```swift
let unsignedInt: UInt = 50
```

### Float
- 32비트 부동소수점 숫자를 표현하며, 주로 소수점을 포함한 숫자를 표현하는데 사용된다.

```swift
let pi: Float = 3.14
```

### Double
- 64비트 부동소수점 숫자를 표현하며, Float보다 더 많은 소수점 자리를 표현할 수 있다.

```swift
let precisePi: Double = 3.1415926535
```

### Bool
- 논리값을 표현하며, true 또는 false 값을 가질 수 있다.

```swift
let isSwiftAwesome: Bool = true
```

### Character
- 단일 문자를 표현하며, Swift의 Character는 유니코드 문자를 지원합니다.

```swift
let letter: Character = "A"
```

### String
- 문자열을 표현하며, String은 문자의 시퀀스이다. (Swift는 유니코드 완전 지원을 제공한다.)

```swift
let greeting: String = "Hello, Swift!"
```

<br>

## 컬렉션 데이터 타입
---
Swift는 여러 값의 그룹을 표현할 수 있는 몇 가지 컬렉션 타입을 제공한다.

### Array
- 동일한 타입의 값들을 순서대로 저장하는 리스트이다.

```swift
let numbers: [Int] = [1, 2, 3, 4, 5]
```

### Set
- 동일한 타입의 유일한 값들을 저장하는 컬렉션입니다. 순서가 없고, 중복된 값을 허용하지 않습니다.

```swift
let uniqueNumbers: Set<Int> = [1, 2, 3, 3, 4]
```

### Dictionary
- 키와 값의 쌍으로 구성된 컬렉션입니다. 각 키는 유일하며, 키와 값의 타입을 지정할 수 있습니다.

```swift
let studentGrades: [String: Int] = ["Alice": 90, "Bob": 85, "Charlie": 88]
```

<br>

## 사용자 정의 데이터 타입
---
Swift에서는 구조체(Struct), 클래스(Class), 열거형(Enum) 등을 사용하여 사용자 정의 데이터 타입을 만들 수 있다.

### Struct
- 값 타입으로, 주로 간단한 데이터 구조를 정의하는 데 사용됩니다.

```swift
struct Person {
    var name: String
    var age: Int
}

let john = Person(name: "John", age: 30)
```

### Class
- 참조 타입으로, 더 복잡한 데이터 구조나 객체 지향 프로그래밍을 위한 기능을 제공한다.

```swift
class Animal {
    var name: String
    init(name: String) {
        self.name = name
    }
}

let dog = Animal(name: "Dog")
```

### Enum
- 연관된 값들의 그룹을 정의하는 데 사용됩니다. 각 값은 고유한 식별자를 가집니다.

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}

let direction = CompassPoint.north
```

<br>

## 튜플
---
튜플(Tuple)은 Swift에서 데이터 타입 중 하나다. 튜플은 여러 개의 값을 하나의 그룹으로 묶어주며, 이는 다양한 데이터 타입을 하나의 단위로 묶어 처리할 때 유용하다. 튜플은 함수에서 여러 개의 값을 반환하거나, 관련된 여러 개의 값을 하나로 묶을 때 자주 사용된다.

### 선언 및 초기화
- 튜플은 소괄호 ()를 사용하여 정의하며, 각 값은 쉼표로 구분합니다. 예를 들어, 두 개의 정수를 포함하는 튜플은 다음과 같이 정의할 수 있다.

```swift
let mixedTuple: (Int, String, Double) = (1, "Hello", 3.14)
```

### 요소 접근
- 튜플의 각 요소는 인덱스를 사용하여 접근할 수 있다. 인덱스는 0부터 시작한다.

```swift
let person: (String, Int) = ("Alice", 25)
let name = person.0  // "Alice"
let age = person.1   // 25
```

- 또는 튜플을 정의할 때 각 요소에 이름을 부여하여 접근할 수 있다.

```swift
let person: (name: String, age: Int) = (name: "Alice", age: 25)
let name = person.name  // "Alice"
let age = person.age    // 25
```

### 사용 예시
1\. 여러 값을 반환하는 함수
  - 튜플은 함수가 여러 값을 반환할 때 유용하다.

  ```swift
  func getUserInfo() -> (String, Int) {
      return ("Bob", 30)
  }

  let userInfo = getUserInfo()
  print("Name: \(userInfo.0), Age: \(userInfo.1)")
  ```

2\. 임시 데이터 그룹화
  - 튜플은 간단하게 관련 데이터를 그룹화할 때 사용된다.

  ```swift
  let coordinates: (x: Int, y: Int) = (10, 20)
  print("x: \(coordinates.x), y: \(coordinates.y)")
  ```

3\. 스위프트의 패턴 매칭과 함께 사용
  - 튜플은 스위프트의 패턴 매칭과 함께 사용될 수 있어 강력한 기능을 제공한다.

  ```swift
  let point = (2, 3)

  switch point {
  case (0, 0):
      print("The point is at the origin.")
  case (let x, 0):
      print("The point is on the x-axis at \(x).")
  case (0, let y):
      print("The point is on the y-axis at
  ```

---

읽어주셔서 감사합니다. 😊 

__Reference__  
ChatGPT - OpenAI