---
title: "[Swift] 문자와 문자열"
description: 
author: 이원모
date: 2024-07-04
categories: [Language, Swift]
tags: [swift, 스위프트, string, character]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/swift-logo.png
  lqip: 
  alt: 
---

스위프트에서 문자열과 문자 처리는 매우 강력하고 유연하게 설계되어 있습니다. 문자(Character)와 문자열(String)를 다루는 기본적인 방법을 살펴보겠습니다.

## 문자
---
### 문자 초기화
- 문자 초기화

    ```swift
    let letter: Character = "A"
    let digit: Character = "5"
    let specialCharacter: Character = "!"
    ```

<br>

### 문자의 유니코드 및 ASCII 값
- 유니코드 스칼라 값 접근

    ```swift
    let character: Character = "A"
    for scalar in character.unicodeScalars {
        print(scalar.value) // 결과는 65
    }
    ```

- 유니코드 스칼라 값에서 문자 생성
    ```swift
    let scalar = Unicode.Scalar(65)
    let character = Character(scalar!) // 결과는 'A'
    ```

<br>

### 문자와 문자열 변환
- 문자에서 문자열 변환

    ```swift
    let character: Character = "A"
    let string = String(character) // 결과는 "A"
    ```

- 문자열에서 문자 배열 변환

    ```swift
    let string = "Hello"
    let characters = Array(string) // 결과는 ["H", "e", "l", "l", "o"]
    ```

<br>

### 문자 비교
- 문자 비교

    ```swift
    let char1: Character = "a"
    let char2: Character = "b"
    let isEqual = char1 == char2 // 결과는 false
    let isLess = char1 < char2 // 결과는 true
    ```

<br>

### 문자 속성과 메서드
- 문자가 숫자인지 확인

    ```swift
    let digit: Character = "5"
    let isDigit = digit.isNumber // 결과는 true
    ```

- 문자가 대문자인지 확인

    ```swift
    let uppercaseLetter: Character = "A"
    let isUppercase = uppercaseLetter.isUppercase // 결과는 true
    ```

- 문자가 소문자인지 확인

    ```swift
    let lowercaseLetter: Character = "a"
    let isLowercase = lowercaseLetter.isLowercase // 결과는 true
    ```

- 문자가 알파벳인지 확인

    ```swift
    let letter: Character = "a"
    let isLetter = letter.isLetter // 결과는 true
    ```

<br>

### 문자 연산
스위프트에서는 문자에 대한 직접적인 산술 연산은 지원하지 않지만, 유니코드 값을 이용하여 문자를 다룰 수 있습니다.

- 문자의 유니코드 값 변경

    ```swift
    if let scalar = letter.unicodeScalars.first {
         
        let nextScalarValue = scalar.value + 1
        
        if let nextScalar = Unicode.Scalar(nextScalarValue) {
            let nextCharacter = Character(nextScalar)
            print(nextCharacter) // 결과는 'b' (문자 'a'에서 +1)
        }
    }
    ```

<br>

## 문자열
---
### 문자열 초기화
- 문자열 초기화

    ```swift
    let greeting = "Hello, World!"
    ```

- 빈 문자열 초기화

    ```swift
    var emptyString = ""
    var anotherEmptyString = String()
    ```

<br>

### 문자열 연결
- 문자열 연결

    ```swift
    let firstName = "John"
    let lastName = "Doe"
    let fullName = firstName + " " + lastName // 결과는 "John Doe"
    ```

- 문자열 보간

    ```swift
    let age = 30
    let message = "I am \(age) years old" // 결과는 "I am 30 years old"
    ```

<br>

### 문자열 속성 및 메서드
- 문자열 길이 확인

    ```swift
    let str = "Hello, Swift!"
    let length = str.count // 결과는 13
    ```

- 문자열이 비어 있는지 확인

    ```swift
    let str = "Hello, Swift!"
    let isEmpty = str.isEmpty // 결과는 false
    ```

- 문자열 비교

    ```swift
    let string1 = "Hello"
    let string2 = "Hello"
    let areEqual = string1 == string2 // 결과는 true
    ```

- 문자열 접두사 및 접미사 확인

    ```swift
    let hasPrefix = str.hasPrefix("Hello") // 결과는 true
    let hasSuffix = str.hasSuffix("Swift!") // 결과는 true
    ```

<br>

### 문자열 인덱싱 및 슬라이싱
- 문자열의 특정 문자 접근
    ```swift
    let greeting = "Hello, World!"
    let firstCharacter = greeting[greeting.startIndex] // 결과는 'H'
    let lastCharacter = greeting[greeting.index(before: greeting.endIndex)] // 결과는 '!'
    let secondCharacter = greeting[greeting.index(after: greeting.startIndex)] // 결과는 'e'
    let fourthCharacter = greeting[greeting.index(greeting.startIndex, offsetBy: 3)] // 결과는 'l'
    ```

- 문자열 슬라이싱

    ```swift
    let start = greeting.index(greeting.startIndex, offsetBy: 7)
    let end = greeting.index(greeting.endIndex, offsetBy: -1)
    let substring = greeting[start..<end] // 결과는 "World"
    ```

<br>

### 문자열 수정
- 문자열에 문자 추가

    ```swift
    var welcome = "Hello"
    welcome.append("!")
    ```

- 문자열 삽입

    ```swift
    var welcome = "Hello"
    welcome.insert(",", at: welcome.index(welcome.startIndex, offsetBy: 5)) // 결과는 "Hello,"
    ```

- 문자열 제거

    ```swift
    var welcome = "Hello, World!"
    welcome.remove(at: welcome.index(before: welcome.endIndex)) // 결과는 "Hello, World"
    ```

---

읽어주셔서 감사합니다. 😊 

__Reference__  
ChatGPT - OpenAI