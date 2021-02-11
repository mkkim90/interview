# interview
인터뷰

## what why how
## 키워드에 집중 
## 꼬리물기식 질문에 대한 답변으로 

## 대용량 

• 분당 50만건의 요청이 오면 어떻게 API를 만들어야할까?
 ```
 어떤 비즈니스 로직의 api를 만들지는 모르겠지만, 특정 비즈니스 로직 구현을 우선시 작업을 하고 이 구현의 완성도를 높인 다음
 질문과 같이 분당 50만건의 성능테스트를 진행할 것입니다. 
 병목 현상이 일어난다면 `캐시 아키텍쳐` 를 고려해볼것 같습니다. 캐시를 통해 실제 DB 부하를 방지하기 위해서입니다. 
 물론 그 수치가 맞는지는 직접 데이터 기반으로 판단을 해야겠지만, 레디스라는 메모리 캐시를 활용하면 평균적으로 초당 2만~10만개 요청을 처리할 수 있다고 합니다. 
 그래서 레디스를 DB 앞단에 위치시키고 db 요청데이터를 레디스에 적재함으로써 캐시 형태의 조회 데이터를 반환하는 방식으로 하는 것도 좋을 것 같습니다. 
 ```
  * 그럼 레디스는 무엇인가요 ? `NoSQL & Cache` 솔루션이며 메모리 기반으로 구성된 것입니다.  
  * 성능테스트라고 말씀하셨는데요 주로 어떤 식으로 성능테스트를 진행하시나요 ? 
    성능테스트로 실무에서는 jmeter를 활용하여 트래픽 부하를 주고 scouter를 활용하여 그 성능 부하 경과를 모니터링하는 식으로 진행을 했었습니다.
    * jmeter란 무엇인가요 ? 
      ```
        jemter는 부하테스트 측정을 돕는 도구인데요, 이를 통해 동시 사용자수를 입력하여 특정 url로 부하를 주는 방식으로 진행할 수 있습니다. 
      ```
    * scouter란 무엇인가요 ?
      ```
        scouter는 성능 모니터링 툴입니다. 
      ```
• 일일 PV가 1억일 경우 어디까지 캐시 Layer를 둬야할까
```
1억 일 경우에 대한 캐시 Layer를 둬야할지 가늠히 가지 않습니다. 
하지만 마스터와 슬레이브를 두는 관점으로 failover가 이러나지 않도록 할 것 같습니다. 
아니면 역할의 분리로 마스터에서는 명령에 대한 행위 그리고 슬레이브에서는 읽기 조회에 대한 행위를 할수 있게끔 처리하는것이 좋을 것 같습니다.
```

• 수억건에서 100만건씩을 조회하여 엑셀/메일로 제공하려면 어떻게 해야할까
```
병렬 방식으로 100만건을 따로 분리하여 엑셀 및 메일을 제공하도록 할 것 입니다.
사실상 100만건 데이터도 상당히 많은 데이터라는 생각이 드는데요. 이걸 사전에 수억건이라고 판단이 된다면 샤딩이나 분산db방식으로 나눠서 처리될수 있도록 한다면 좋을 것 같습니다.
```
 * 병렬 방식을 말씀하셨는데요, 병렬 프로그래밍에 대해서 설명해주시겠습니까?
``` 
자바는 멀티쓰레드 환경을 지원해주는데요,
이를 통해서 병렬로 하나의 쓰레드당 엑셀 또는 이메일을 100만건씩 작업하도록 처리할것입니다. 
```

  * 그럼 멀티쓰레드가 무엇인가요 ??
```
쓰레드란, 프로세스 내에서 실행되는 시간의 흐름 단위로 프로세스는 최소 하나 이상의 쓰레드를 갖는다것을 의미합니다. 
그렇다면 멀티쓰레드란 실행흐름단위를  동시에 처리할수 있는 것을 의미합니다. 
``` 

  * 오호 여러번 실행되서 효율적으로 작업을 처리할 수 있겠네요! 그렇다면 멀티쓰레드에 대한 장단점을 말씀해주시겠어요? 
```
말씀해주신대로, 멀티쓰레드를 사용하면 하나의 프로세스에서 여러개의 쓰레드를 사용하기 때문에 공유 자원과 시스템 자원 소모가 줄어듭니다.
그러므로 시스템 처리량이 향상되고 응답시간을 단축할 수 있습니다.
그리고 데이터영역과 힙영역을 공유 하기 때문에 쓰레드간 데이터 교환이 필요하지 않습니다.
```

	* [돌발질문] 말씀 끊어서 죄송한대요, 데이터영역과 힙영역이 공유된다고 하셨는데 데이터영역이랑 힙영역이 뭐예요? 추가적으로 스택영역은 뭐죠? 흠.. 메모리 구조 기반으로 	설명해주시면 좋을 것 같아요
		프로그램이 실행되기 위해서는 메모리가  로드가 되어야합니다. 프로그램에서 사용되는 변수들을 저장한 메모리 공간이 필요합니다.
		그러한 종류로 코드영역, 데이터영역, 힙영역, 스택영역이 있는데요.
			코드영역은 메모리의 코드(code) 영역은 실행할 프로그램의 코드가 저장되는 영역입니다. 
			cpu는 코드영역에 저장된 명령어를 하나씩 가져가서 처리하게 됩니다.
			데이터영역 전역변수 또는 정적변수입니다. 프로그램 시작과 함께 할당되며 프로그램이 종료되면 소멸됩니다.
			스택영역은 함수의 호출과 관계되는 지역변수와 매개변수가 저장되는 영역을 의미합니다. 
			힙영역은  사용자가 직접관리할 수 있는 메모리 영역입니다. 메모리 공간이 동적으로 할당되고 해제됩니다. 

	* [꼬리물기질문]힙영역이 메모리 공간을 동적으로 할당한다고 하셨는데요, 자바의 메모리 공간을 해제하기 위해서는 어떤 방법을 사용하죠? 
		GarbageCollector를 통해서 메모리 반환을 합니다. 
			*[꼬리물기질문]네, GarbageCollector에 대해 말씀해주세요.
				동적으로 할당했던 메모리 공간에서 필요없는 메모리 공간은 반환함으로써 메모리 고갈 이슈를 방지합니다. 
			*그렇군요, 어떻게 동작하는지 원리에 대해 말씀해주세요.
				Stop-the-world, Mark And Sweep에 대해 설명드리겠습니다.
				Stop the world는 gc 실행을 위해 jvm 애플리케이션을 멈추는 것을 의미합니다.
				gc를  실행하는 쓰레드를 제외한 모든 스레드들이 작업을 멈추게됩니다.
				여기서 Mark And Sweep이라는 개념으로  이때 어떤 객체를 참조하고 있는지 찾는 과정을 거치는데 이를 Mark라고 불리우며, 
				Sweep은 참조되는 않은 객체는 제거하는 과정 의미합니다.

			* 더 깊이 들어가는 질문이 있을시,  그리고 모르겠는 말하면 
				모르겠습니다. 튜닝 경험은 없지만, Stop-the-world 의 시간을 줄이는 작업을 하는 것으로 알고 있습니다. 
 
 * 샤딩이 뭐죠, 어떤 이점을 가지고 있나요?

•매일 수백만건의 주문 데이터를 타임아웃 이슈 없이 어떻게 API로 받아낼 수 있을까

•수억건의 데이터가 담긴 테이블에서 페이징 처리는 어떻게 해야할까

•200만건 중 10만건만 남기고 실패가 나면 어떻게 남은 건들을 처리해야할까

•JPA에서 복잡한 조회 쿼리는 어떻게 해결할 수 있지? 

•수억건 데이터 사이에서도 JPA가 성능이슈가 없는 것일까?

•OneToMany 관계가 2개 이상인 경우엔 N+1 문제를 어떻게 해결하지?

•FetchJoin은 OneToMany 관계의 1개 테이블에만 사용할 수 있습니다.

•Spring Batch와 Querydsl를 어떻게 함께 쓸수있을까?

•Querydsl의 Paging 기능이 성능이 얼마나 나오며, 성능 저하를 어떻게 해결할 수 있을까?

• blocking vs non-blocking / synchronous vs asynchronous
	Synchronous I/O와 Asynchtounous I/O
		- 동기 : 작업을 요청한 후 작업의 결과가 나올 때까지 기다린 후 처리
		- 비동기 : 직전 시스템 호출의 종료가 발생하면 그에 따른 처리를 진행
		
	Blocking I/O와 Non-Blocking I/O
		- Blocking : 유저 프로세스가 시스템 호출을 하고 나서 결과가 반환되기까지 다음 처리로 넘어가지 않음
		- Non-Blocking : 호출한 직후에 프로그램으로 제어가 돌아와서 시스템 호출의 종료를 기다리지 않고 다음 처리로 넘어갈 수 있음
		
		
		

## 싱글톤이란 무엇이고 장점과 단점을 설명해주세요
- 오직 1개만 생성
- 메모리 측면에서 데이터 공유 자원인 경우 
- 문제점 동기화 이슈가 있을듯 합니다. 
	내부 상태값아라든지 변경하는 부분에서
	유연성이 있는 패턴은 아니고 유틸성으로 쓰면 좋은 패턴인	것 같습니다.


## Mockito어떨 때 쓸까요
외부 api 같은 경우는 상위 객체로 책임을 전가 하기 힘듬
복잡한 레이어 또는 대체하기 어렵구 테스트 진행이 어려운 
불확실성한 경우에 테스트를 진행하면 좋을 것 같음


## LazyInitializationException이 뭔가요? 그리고 어떻게 해결하면 좋을 까요?
LazyInitializationException 라는 Exception이 발생한다
영속성 범위를 벗어날 경우 준영속성이 될때 작용하게 된다. 

-> 다시 불러온다. 
-> osiv true

## Contructor Injection 쓰는 이유가 있을까요?
-> DI를 실제 컨테이너를 통해 하는것보다 
객체 지향적이고 유연하게 사용할 수 있도록 도와줄 수 있습니다.

	1. 단일 책임의 원칙
  	2. 컨테이너와의 낮은 결합도
  	3. 필드의 불변성 보장
  	4. 순환 의존 방지 

## VO가 뭔가요?
VO 란 , 한개나 그 이상의 특정 값을 의미있게 나타낼수 있는 불변 객체

## 상속보다 컴포지트를 선호하는 이유가 뭘까요?
상속은 확장성에 좋을 수도 있지만 하위클래스가 상위클래스에 
의존하는 형식을 이루게됩니다. 그래서 변경에 유연하지 못할 수 있습니다. 

조합을 활용하면 메서드를 호출하는 방식으로 동작하기 때문에
캡슐화를 깨뜨리지 않는다.

변화에 영향이 적어지며, 안전하다

Is - a 관계면 상속이 수월 

## 테스트하기 좋은 코드가 뭘까요? 
- 시그니처를 주어 파라미터로 처리함
- 인터페이스로 분리하는 방식 


## jvm 설명
자바 가상 머신
자바 코드로 작성한 프로그램은 실행할 환경에 독립적으로 실행할 수 있도록 해주는 역할을 하는 가상머신

 java virtual machine의 줄임말이며 Java Byte Code를 OS에 맞게 해석 해주는 역할을 합니다.

클래스 로더를 통해 컴파일을 하여 바이트코드로 만들어서 로드합니다.
실행엔진의 인터프리터를 활용하여 바이트코드를 기계어로 번역하는 역할을 합니다. 
그외로 저장공간 메모리, 네이티브 메소드 들로 이루어져있습니다. 


JIT는 bytecode를 어셈블러 같은 nativecode로 바꿔서 실행이 빠르지만 
역시 변환하는데 비용이 발생한다. 이 같은 이유 때문에 jvm은 모든 코드를 jit compiler 방식으로 실행하지 않고 
interpreter 방식을 사용하다 일정 기준이 넘어가면 jit compiler 방식으로 실행한다.

- JRE(Java Runtime Environment)
- JDK(Java Development Kit)


