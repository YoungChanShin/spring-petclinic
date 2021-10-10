# Inversion of Control

제어권이 뒤바뀌었다. 예를 들면 `controller`에서 필요한 객체를 `controller`밖에서 구현하고 그것을 호출해 쓰는 방식을 말한다. 달리 말해서 의존성을 관리하는 일이 해당 로직 밖에 있다는 의미

뒤바뀌지 않은 제어권 

```java
class OwnerController {
    private OwnerRepository repository = new OwnerRepository();
}
```

`OwnerController`를 구현할 때 필요한 `repo`를 직접 호출(생성)해서 사용한다.



**제어권 역전**

```java
class OwnerController {
	private OwnerRepository repo;
	public OwnerController(OwnerRepository repo) {
	this.repo = repo;
}
// repo를 사용합니다.
}
class OwnerControllerTest {
	@Test
	public void create() {
	OwnerRepository repo = new OwnerRepository();
	OwnerController controller = new OwnerController(repo);
    }
}
```

`OwnerController`를 구현할 때 밖에서 생성된 `repo`를 사용한다.

## 스프링 IOC 컨테이너(applicationContext, Beanfactory)

소스에는 핵심적인 클래스이지만 직접 참고해서 쓴는 빈팩토리를 보기 어렵다. 

빈을 만들고 엮어주며 제공해준다. 

컨테이너 안에 등록된 빈이 있고 관리가 된다.

어노테이션. 인터페이스 상속,  빈을 명시적으로 표시 세가지 방법으로 빈 등록

의존성 주입은 IOC 컨테이너 안에 빈으로 등록된 클래스만 가능하다.

```java
@Controller
class OwnerController {

	private static final String VIEWS_OWNER_CREATE_OR_UPDATE_FORM = "owners/createOrUpdateOwnerForm";

	private final OwnerRepository owners;
    private final ApplicationContext applicationContext;

	public OwnerController(OwnerRepository clinicService, VisitRepository visits, ApplicationContext applicationContext) {
		this.owners = clinicService;
		this.visits = visits;
        this.applicationContext = applicationContext;
	}


	@GetMapping("/bean")
    @ResponseBody
    public String bean() {
        return "Bean : "+ applicationContext.getBean(OwnerRepository.class)+"\n" +
            "Owner : "+ this.owners;
    }
}
```

> Bean : org.springframework.data.jpa.repository.support.SimpleJpaRepository@975ba7c 
>
> Owner : org.springframework.data.jpa.repository.support.SimpleJpaRepository@975ba7c

해쉬값이 같은 것을 확인할 수 있다. ApplicationContext라는 스프링 컨테이너 IoC를 명시적으로 사용하지 않더라도 컨테이너에 등록된 bean들을 스프링이 관리해주기 때문에 작동한다. 

## BEAN

>  스프링 IoC 컨테이너가 관리하는 객체 = application Context가 알고 있는 객체

### 등록하는 방법

- Component Scanning 
  - @Component
  - @Repository
  - @Service 
  - @Controller 
  - @Configuration 
  
- 또는 직접 일일히 XML이나 자바 설정 파일에 등록

  ```java
  // sample/SampleConfig
  @Configuration 
  public class SampleConfig {
   @ Bean 
   public SampleController sampleController() {
       return new SampleController();
   }
  }
  ```



### 사용하는 방법

- @Autowired 또는 @Inject 
- ApplicationContext에서 getBean()으로 직접 꺼내거나



## 의존성 주입

### 1. 생성자

권장하는 방법. 필수적으로 사용해야 하는 레퍼런스 없이는 인스턴스를 만들지 못하도록 강제할 수 있다. 아래 두가지는 `OwnerRepository`없이 `OwnerController`를 생성할 수 있다. 다만 순환 참조가 발생할 경우에는 아래 두가지 방법을 이용할 수 밖에 없다.

### 2. 필드



### 3. Setter

스프링 4.3부터, 빈으로 등록되어 있으면 생성자에 @autowired 생략 가능.  자동으로 추가해주도록 하는 기능 추가. 

### 과제

> OwnerController에 petRepository 주입하기

```java
// 1. 생성자를 이용하는 방식
private final PetRepository petRepository; // final 붙이는 것은 선택

public OwnerController(OwnerRepository clinicService, VisitRepository visits, PetRepository petRepository) {
	this.owners = clinicService;
	this.visits = visits;
	this.petRepository = petRepository;
}

```

```java
// 2. 필드를 이용하는 방식
@Autowired
private PetRepository petRepository;
```

```java
// 3. setter를 이용하는 방식
private PetRepository petRepository; // final 키워드를 쓰면 안된다.

@Autowired
public void setPetRepository(PetRepository petRepository) {
    this.petRepository = petRepository;
}
```

