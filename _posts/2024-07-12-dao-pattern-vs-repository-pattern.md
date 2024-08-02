---
title: "[Spring] DAO 패턴 vs Repository 패턴"
description: 
author: 이원모
date: 2024-07-12
categories: [Framework, Spring]
tags: [디자인패턴, dao패턴, repository패턴, spring, 스프링]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/spring-logo.jpg
  lqip: 
  alt: 
---

이번 글에서는 데이터 접근 로직과 비즈니스 로직을 분리하는 대표적인 디자인 패턴인 Dao패턴과 Repository패턴에 대해 간단히 알아보겠습니다.

## Repository 패턴
---
레퍼지토리 패턴은 객체 지향 설계의 원칙을 따르며, 데이터 접근 로직과 비즈니스 로직을 분리하는 데 중점을 둡니다. 이는 주로 도메인 주도 설계(DDD)에서 사용되며, 도메인 객체의 컬렉션을 관리하는 개념을 제공합니다. 레퍼지토리는 데이터베이스의 구체적인 구현 세부 사항을 추상화하여 비즈니스 로직이 데이터 접근 방법에 종속되지 않도록 합니다.

예시 (Java, Spring Data JPA 사용)

```java
// User 엔티티 클래스
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
    
    // Getters and Setters
}

// UserRepository 인터페이스
public interface UserRepository extends JpaRepository<User, Long> {
    // 추가적인 사용자 정의 메서드
}

// UserService 클래스
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;
    
    public User registerUser(String name, String email) {
        User user = new User();
        user.setName(name);
        user.setEmail(email);
        return userRepository.save(user);
    }
    
    public List<User> getAllUsers() {
        return userRepository.findAll();
    }
    
    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }
}

// UserController 클래스
@RestController
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @PostMapping
    public User registerUser(@RequestBody User user) {
        return userService.registerUser(user.getName(), user.getEmail());
    }
    
    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }
    
    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.getUserById(id);
    }
}
```

<br>

## DAO 패턴
---
DAO 패턴은 데이터베이스와 상호작용하는 코드를 추상화하고 분리하는 데 중점을 둡니다. DAO는 데이터 접근 로직을 캡슐화하여, 데이터베이스와의 직접적인 상호작용을 서비스나 비즈니스 로직으로부터 분리합니다. 이는 주로 데이터베이스 테이블이나 쿼리에 대한 추상화를 제공합니다.

예시 (Java, JPA 사용)

```java
// User 엔티티 클래스
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;

    // Getters and Setters
}

// UserDao 인터페이스
public interface UserDao {
    void save(User user);
    User findById(Long id);
    List<User> findAll();
    void update(User user);
    void deleteById(Long id);
}

// UserDaoImpl 클래스
@Repository
@Transactional
public class UserDaoImpl implements UserDao {
    @PersistenceContext
    private EntityManager entityManager;

    @Override
    public void save(User user) {
        entityManager.persist(user);
    }

    @Override
    public User findById(Long id) {
        return entityManager.find(User.class, id);
    }

    @Override
    public List<User> findAll() {
        return entityManager.createQuery("from User", User.class).getResultList();
    }

    @Override
    public void update(User user) {
        entityManager.merge(user);
    }

    @Override
    public void deleteById(Long id) {
        User user = findById(id);
        if (user != null) {
            entityManager.remove(user);
        }
    }
}

// UserService 클래스
@Service
public class UserService {
    @Autowired
    private UserDao userDao;

    public User registerUser(String name, String email) {
        User user = new User();
        user.setName(name);
        user.setEmail(email);
        userDao.save(user);
        return user;
    }

    public List<User> getAllUsers() {
        return userDao.findAll();
    }

    public User getUserById(Long id) {
        return userDao.findById(id);
    }

    public void updateUser(User user) {
        userDao.update(user);
    }

    public void deleteUser(Long id) {
        userDao.deleteById(id);
    }
}

// UserController 클래스
@RestController
@RequestMapping("/users")
public class UserController {
    @Autowired
    private UserService userService;

    @PostMapping
    public User registerUser(@RequestBody User user) {
        return userService.registerUser(user.getName(), user.getEmail());
    }

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.getUserById(id);
    }

    @PutMapping
    public void updateUser(@RequestBody User user) {
        userService.updateUser(user);
    }

    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
    }
}
```

<br>

## 공통점과 차이점
---
- 공통점

  - 비즈니스 로직과 데이터 접근 로직의 분리  

    두 패턴 모두 비즈니스 로직과 데이터 접근 로직을 분리하여 유지 보수성을 높입니다.

  - 추상화  

    데이터베이스와 상호작용하는 코드를 추상화하여, 서비스 계층이 데이터베이스 접근 방법에 종속되지 않도록 합니다.

<br>
- 차이점
  
  - 객체 지향 설계의 원칙

    - 레퍼지토리 패턴  

      객체 지향 설계 원칙을 더욱 엄격하게 따르며, 도메인 모델과의 상호작용을 추상화합니다. 주로 도메인 주도 설계(DDD)에서 사용됩니다.

    - DAO 패턴  

      데이터 접근 로직을 단순히 추상화하는 데 중점을 둡니다. 데이터베이스 테이블이나 쿼리에 더 가까운 추상화를 제공합니다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
ChatGPT - OpenAI