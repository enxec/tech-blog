---
title: "[Spring] OpenAPI 어노테이션"
description: 
author: 이원모
date: 2024-08-07
categories: [Framework, Spring]
tags: [OpenAPI, annotation, 어노테이션, rest api, spring boot, 스프링부트]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/spring-logo.jpg
  lqip: 
  alt: 
---

Spring에서의 OpenAPI 어노테이션은 주로 Springdoc OpenAPI 라이브러리를 사용하여 API 문서를 자동으로 생성할 때 사용되며, API 문서화할 때 큰 역할을 합니다. 이번 포스트에서는 OpenAPI의 주요 어노테이션에 대해 살펴보겠습니다.

## @Operation
---
@Operation은 특정 엔드포인트에 대한 정보를 정의합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">summary</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">엔드포인트에 대한 요약 설명</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">description</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">엔드포인트에 대한 자세한 설명</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">tags</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">태그를 사용하여 엔드포인트를 그룹화</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">responses</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">가능한 응답을 정의</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">parameters</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">엔드포인트의 파라미터를 정의</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">requestBody</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">요청 본문을 정의</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">deprecated</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">엔드포인트가 더 이상 사용되지 않음을 표시</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @Operation(summary = "Get user by ID", description = "Returns a user when a valid ID is provided")
  @GetMapping("/users/{id}")
  public ResponseEntity<User> getUserById(@PathVariable Long id) {
      // Implementation here
  }
  ```

<br>

## @Parameter
---
@Parameter는 API 메서드의 파라미터를 설명합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">name</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">파라미터 이름</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">description</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">파라미터에 대한 설명</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">required</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">파라미터의 필수 여부</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">example</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">파라미터의 예제 값</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">schema</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">파라미터의 스키마를 정의</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @GetMapping("/users/{id}")
  public ResponseEntity<User> getUserById(
      @Parameter(description = "ID of the user to be obtained", required = true)
      @PathVariable Long id) {
      // Implementation here
  }
  ```

<br>

## @RequestBody
---
@RequestBody는 요청 본문을 설명합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">description</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">요청 본문에 대한 설명</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">required</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">요청 본문의 필수 여부</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">content</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">요청 본문의 내용을 정의 (미디어 타입과 스키마)</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @Operation(summary = "Create a new user")
  @PostMapping("/users")
  public ResponseEntity<User> createUser(
      @RequestBody(description = "User to be created", required = true,
                  content = @Content(schema = @Schema(implementation = User.class)))
      @Valid @RequestBody User user) {
      // Implementation here
  }
  ```

<br>

## @ApiResponses
---
@ApiResponses는 여러 @ApiResponse 어노테이션을 그룹화합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">value</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">`@ApiResponse` 어노테이션 배열</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @ApiResponses(value = {
      @ApiResponse(responseCode = "200", description = "Successful operation",
          content = @Content(mediaType = "application/json",
                            schema = @Schema(implementation = User.class))),
      @ApiResponse(responseCode = "404", description = "User not found")
  })
  @GetMapping("/users/{id}")
  public ResponseEntity<User> getUserById(@PathVariable Long id) {
      // Implementation here
  }
  ```

<br>

## @ApiResponse
---
@ApiResponse는 가능한 응답을 설명합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">responseCode</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">응답 코드 (예: 200, 404)</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">description</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">응답에 대한 설명</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">content</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">응답 본문의 내용을 정의 (미디어 타입과 스키마)</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @ApiResponse(responseCode = "200", description = "Successful operation",
      content = @Content(mediaType = "application/json",
                        schema = @Schema(implementation = User.class)))
  @ApiResponse(responseCode = "404", description = "User not found")
  ```

<br>

## @Schema
---
@Schema는 모델 클래스 또는 필드의 스키마를 정의합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">description</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">필드 또는 클래스에 대한 설명</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">example</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">필드 또는 클래스의 예제 값</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">required</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">필드의 필수 여부 (클래스 레벨에서 사용)</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @Schema(description = "User entity")
  public class User {
      @Schema(description = "Unique identifier of the user", example = "1", required = true)
      private Long id;

      @Schema(description = "Name of the user", example = "John Doe", required = true)
      private String name;
      
      // Other fields, getters, and setters
  }
  ```

<br>

## @Content
---
@Content는 요청 또는 응답 본문의 내용을 정의합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">mediaType</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">미디어 타입 (예: "application/json")</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">schema</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">스키마를 정의</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">examples</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">예제 값을 정의</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @ApiResponse(responseCode = "200", description = "Successful operation",
      content = @Content(mediaType = "application/json",
                        schema = @Schema(implementation = User.class)))
  ```

<br>

## @ExampleObject
---
@ExampleObject는 예제 객체를 정의합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">name</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">예제 이름</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">summary</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">예제 요약</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">description</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">예제에 대한 설명</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">value</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">예제 값</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @Schema(description = "User entity")
  public class User {
      @Schema(description = "Unique identifier of the user", example = "1", required = true)
      private Long id;

      @Schema(description = "Name of the user", example = "John Doe", required = true)
      private String name;

      @Schema(description = "User email", example = "john.doe@example.com")
      private String email;
      
      // Other fields, getters, and setters
  }
  ```

<br>

## @ArraySchema
---
@ArraySchema는 배열 타입의 스키마를 정의합니다.

- 속성

  <table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
      <col style="width:40%;">
      <col style="width:60%;">
    </colgroup>
    <thead>
      <tr style="background-color:darkslategray;">
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">속성</th>
        <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">schema</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">배열의 요소 스키마를 정의</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">maxItems</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">배열의 최대 아이템 수</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">minItems</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">배열의 최소 아이템 수</td>
      </tr>
      <tr>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">uniqueItems</td>
        <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">배열의 아이템 고유 여부</td>
      </tr>
    </tbody>
  </table>

- 예제 코드

  ```java
  @ArraySchema(schema = @Schema(implementation = User.class))
  private List<User> users;
  ```

<br>

---

읽어주셔서 감사합니다. 😊 

__Reference__  
ChatGPT - OpenAI