# AOP

> 관심사를 기준으로 개발

예를 들면 똑같은 코드가 여러곳에 흩어져 있을 경우, 유지보수를 더 잘 하기 위해 모으면 좋다! 기존의 코드 삽입없이

### 다양한 AOP 구현 방법

1. 컴파일 A.java ----(AOP)---> A.class (AspectJ) 

2. 바이트코드 조작 A.java -> A.class ---(AOP)---> 메모리 (AspectJ) 
3. 프록시 패턴 (**스프링** AOP)

`@Transactional`에도 프록시 패턴을 사용하고 있다.





# Portable Spring Application

서블릿을 쓰고 있지만 아직까지 서블릿을 개발한 적은 없다. 서블릿에서 처리하는 것(Controller역할)하는 것을 어노테이션으로 처리된다.(추상화)

왜 추상화계층인가

1. 서블릿이라는 low level 코드를 짜지 않아도 된다.
2. portable이라는 의미는 다른 방식으로 이동/구현 가능하다는 것이다.
   - 현재 톰캣을 이용해서 띄우고 있는데 네티 기반으로 구현방식을 변경할 때 코드 변경을 최소화할 수 있다. (spring 5의  webflux를 이용하는 방식이 가능한데 이를 사용하기 위해 기술기반을 네티로 가져가야함.)



## 스프링 트랜잭션

여기도 `@Transaction`이라는 어노테이션을 붙이면 사용하는 DB에 따라 자동으로 구현이 가능해진다. (추상화)