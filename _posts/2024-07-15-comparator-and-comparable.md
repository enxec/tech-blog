---
title: "Comparator와 Comparable"
description: 
author: 이원모
date: 2024-07-15
categories: [Language, Java]
tags: [Comparator, Comparable, java, 자바, 컬렉션 프레임워크, Collections Framework]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

Java 프로그래밍에서 객체를 정렬하는 기능은 매우 중요하며, 이를 통해 데이터의 효율적인 검색, 정렬, 저장이 가능해집니다. Java는 이러한 객체 정렬을 위해 Comparator와 Comparable이라는 두 가지 인터페이스를 제공합니다. 이 글에서는 이 두 인터페이스의 정의, 용도, 구현 방법, 그리고 차이점을 살펴보겠습니다.

## Comparator
---
Comparator 인터페이스는 두 개의 객체를 비교하는 메서드를 정의하여 클래스 외부에서 객체의 순서를 지정합니다. 이를 통해 다양한 기준으로 객체를 정렬할 수 있습니다.

- compare() 메서드

    compare() 메서드는 두 객체를 비교하며, 아래와 같은 값을 반환합니다.

    - 음수: 첫 번째 객체가 두 번째 객체보다 작음
    - 0: 두 객체가 같음
    - 양수: 첫 번째 객체가 두 번째 객체보다 큼

<br>

- 기본 구현 방법

    Comparator 인터페이스를 구현하려면 클래스를 만들고 compare() 메서드를 정의해야 합니다.

    아래 예제에서 StudentGradeComparator 클래스는 Student 객체를 학점을 기준으로 정렬하는 Comparator를 정의합니다.

    ```java
    import java.util.Comparator;

    public class StudentGradeComparator implements Comparator<Student> {
        @Override
        public int compare(Student s1, Student s2) {
            return Integer.compare(s1.getGrade(), s2.getGrade());
        }
    }
    ```

<br>

- 장단점
    - 장점
        - 여러 가지 정렬 순서를 정의할 수 있습니다.
        - 기존 클래스 코드를 수정하지 않아도 됩니다.

    - 단점
        - 코드가 비교적 복잡할 수 있습니다.
        - 정렬 기준을 외부에서 정의해야 합니다.

<br>

- 사용 사례

    Comparator 인터페이스는 다양한 정렬 순서가 필요한 경우 유용합니다. 예를 들어, 학생 목록을 이름 순서와 학점 순서로 각각 정렬해야 하는 경우 사용할 수 있습니다.

<br>

## Comparable
---
Comparable 인터페이스는 객체의 자연 순서를 정의하기 위해 사용됩니다. 이 인터페이스를 구현하는 클래스는 단일 compareTo() 메서드를 오버라이드하여 자신의 인스턴스와 다른 인스턴스를 비교합니다.

- compareTo() 메서드

    compareTo() 메서드는 현재 객체와 지정된 객체의 순서를 비교하며, 아래와 같은 값을 반환합니다.

    - 음수: 현재 객체가 비교 객체보다 작음
    - 0: 현재 객체가 비교 객체와 같음
    - 양수: 현재 객체가 비교 객체보다 큼

<br>

- 기본 구현 방법

    Comparable 인터페이스를 구현하려면 클래스에 compareTo() 메서드를 추가해야 합니다.

    아래 예제에서 Student 클래스는 Comparable 인터페이스를 구현하여 학생의 학점을 기준으로 자연 순서를 정의합니다.

    ```java
    public class Student implements Comparable<Student> {
        private String name;
        private int grade;

        public Student(String name, int grade) {
            this.name = name;
            this.grade = grade;
        }

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.grade, other.grade);
        }

        // getters and setters
    }
    ```

<br>

- 장단점
    - 장점
        - 코드가 간단하고 읽기 쉽습니다.
        - 기본 정렬 순서를 한 번만 정의하면 됩니다.

    - 단점
        - 클래스 설계를 수정해야 합니다.
        - 단일 정렬 순서만 정의할 수 있습니다.

<br>

- 사용 사례

    Comparable 인터페이스는 기본 정렬 순서가 필요한 경우 유용합니다. 예를 들어, 학생 목록을 학점 순으로 정렬하는 경우 사용할 수 있습니다.

<br>

## Comparator와 Comparable의 차이점
---
- 인터페이스 목적 비교

    Comparable은 클래스 내부에서 기본 정렬 순서를 정의하는 반면, Comparator는 클래스 외부에서 다양한 정렬 순서를 정의합니다.

- 사용 용도와 시기

    Comparable은 객체의 자연 순서를 정의할 때 사용하고, Comparator는 여러 정렬 순서가 필요하거나 기존 클래스 코드를 수정할 수 없는 경우 사용합니다.

- 코드 예제 비교

    ```java
    // Comparable 예제
    public class Student implements Comparable<Student> {
        private String name;
        private int grade;

        @Override
        public int compareTo(Student other) {
            return Integer.compare(this.grade, other.grade);
        }
    }

    // Comparator 예제
    import java.util.Comparator;

    public class StudentNameComparator implements Comparator<Student> {
        @Override
        public int compare(Student s1, Student s2) {
            return s1.getName().compareTo(s2.getName());
        }
    }
    ```

<br>

## 응용
---
- 다중 필드 정렬

    Comparator를 사용하여 다중 필드로 객체를 정렬할 수 있습니다.

    ```java
    public class MultiFieldComparator implements Comparator<Student> {
        @Override
        public int compare(Student s1, Student s2) {
            int gradeComparison = Integer.compare(s1.getGrade(), s2.getGrade());
            if (gradeComparison != 0) {
                return gradeComparison;
            }
            return s1.getName().compareTo(s2.getName());
        }
    }
    ```

- 익명 클래스와 람다 표현식 사용

    Java 8 이후, 람다 표현식을 사용하여 간단하게 Comparator를 정의할 수 있습니다.

    ```java
    Comparator<Student> byName = (s1, s2) -> s1.getName().compareTo(s2.getName());
    Comparator<Student> byGrade = Comparator.comparingInt(Student::getGrade);
    ```

- 역순 정렬

    Comparator의 reversed() 메서드를 사용하여 역순 정렬할 수 있습니다.

    ```java
    Comparator<Student> byGradeReversed = Comparator.comparingInt(Student::getGrade).reversed();
    ```

- 컬렉션에서 정렬 사용하기

    Collections.sort() 및 Arrays.sort() 메서드를 사용하여 컬렉션과 배열을 정렬할 수 있습니다.

    ```java
    List<Student> students = ...;
    Collections.sort(students);

    Student[] studentArray = ...;
    Arrays.sort(studentArray, new StudentNameComparator());
    ```

- Stream API와 정렬

    Java 8의 Stream API를 사용하여 컬렉션을 정렬할 수 있습니다.

    ```java
    List<Student> sortedStudents = students.stream()
    .sorted(Comparator.comparing(Student::getName))
    .collect(Collectors.toList());
    ```

<br>

## 성능 고려 사항
---
- 정렬 알고리즘 및 복잡도

    Java의 Collections.sort()와 Arrays.sort()는 병합 정렬(merge sort) 알고리즘을 사용하며, 시간 복잡도는 O(n log n)입니다.

- 큰 데이터 집합에서의 성능 최적화

    큰 데이터 집합에서 정렬 성능을 최적화하려면 필요한 경우 정렬 기준을 효율적으로 정의하고, 데이터 구조를 적절하게 선택해야 합니다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
ChatGPT - OpenAI