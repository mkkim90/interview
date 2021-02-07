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
 DB를 활용해야 하는 api라면 우선 DB 쿼리 성능 튜닝부터 진행을 하고 
 그래도 병목 현상이 일어난다면 `캐시 아키텍쳐` 를 고려해볼것 같습니다. 캐시를 통해 실제 DB 부하를 방지하기 위해서입니다. 
 물론 그 수치가 맞는지는 직접 데이터 기반으로 판단을 해야겠지만, 레디스라는 메모리 캐시를 활용하면 평균적으로 초당 2만~10만개 요청을 처리할 수 있다고 합니다. 
 그래서 레디스를 DB 앞단에 위치시키고 db 요청데이터를 레디스에 적재함으로써 캐시 형태로 조회데이터를 반환하는 방식으로 하는 것도 좋을 것 같습니다. 
 ```
  * 그럼 레디스는 무엇인가요 ? `NoSQL & Cache` 솔루션이며 메모리 기반으로 구성된 것입니다.  
  * 성능테스트라고 말씀하셨는데요 주로 어떤 식으로 성능테스트를 진행하시나요 ? 
    성능테스트로 실무에서는 jmeter를 활용하여 트래픽 부하를 주고 scouter를 활용하여 그 성능 부하 경과를 보는 식으로 진행을 했었던 경험이 있습니다.
    * jmeter란 무엇인가요 ? 
      ```
        jemter는 부하테스트 측정을 돕는 도구인데요, 이를 통해 동시 사용자수를 입력해서 루프를 돌리는 방식으로 해서 특정 url로 부하를 주는 방식으로 진행할 수 있습니다. 
      ```
    * scouter란 무엇인가요 ?
      ```
        scouter는 성능 모니터링 툴입니다. 
      ```
• 일일 PV가 1억일 경우 어디까지 캐시 Layer를 둬야할까
```
1억 일 경우에 대한 캐시 Layer를 둬야할지 이해가 가늠히 가지 않습니다. 
하지만 마스터와 슬레이브를 두는 관점으로 failover가 이러나지 않도록 할 것 같습니다. 
아니면 역할의 분리로 마스터에서는 명령에 대한 행위 그리고 슬레이브에서는 읽기 조회에 대한 행위를 할수 있게끔 처리하는것이 좋을 것 같습니다.
```

• 수억건에서 100만건씩을 조회하여 엑셀/메일로 제공하려면 어떻게 해야할까
```
병렬 방식으로 100만건을 따로 분리하여 엑셀 및 메일을 제공하도록 할 것 입니다.
사실상 100만건 데이터도 상당히 많은 데이터라는 생각이 드는데요. 이걸 사전에 수억건이라고 판단이 된다면 샤딩이나 분산db방식으로 나눠서 처리될수 있도록한다면 좋을 것 같습니다.
업무에서 많이 
활용이 
사실상 100만건 데이터도 상당히 많은 데이터라는 생각이 드는데요. 이걸 사전에 수억건이라고 판단이 된다면 샤딩이나 분산db방식으로 나눠서 처리될수 있도록한다면 좋을 것 같습니다.도

```
•매일 수백만건의 주문 데이터를 타임아웃 이슈 없이 어떻게 API로 받아낼 수 있을까

•수억건의 데이터가 담긴 테이블에서 페이징 처리는 어떻게 해야할까

•200만건 중 10만건만 남기고 실패가 나면 어떻게 남은 건들을 처리해야할까

•JPA에서 복잡한 조회 쿼리는 어떻게 해결할 수 있지? 

•수억건 데이터 사이에서도 JPA가 성능이슈가 없는 것일까?

•OneToMany 관계가 2개 이상인 경우엔 N+1 문제를 어떻게 해결하지?

•FetchJoin은 OneToMany 관계의 1개 테이블에만 사용할 수 있습니다.

•Spring Batch와 Querydsl를 어떻게 함께 쓸수있을까?

•Querydsl의 Paging 기능이 성능이 얼마나 나오며, 성능 저하를 어떻게 해결할 수 있을까?

