# 아이템 36 해당 분야의 용어로 타입 이름 짓기

### 요약
- 가독성, 추상화 수준을 높이기 위해 해당 분야의 용어를 사용해야 한다.
- 같은 의미에 다른 이름을 붙이면 안된다. 특별한 의미가 있을 때만 용어를 구분해야 한다.

엄선된 타입, 속성, 변수의 이름은 의도를 명확히 하고 코드와 타입의 추상화 수준을 높여준다.
다음 코드는 동물들의 데이터베이스를 구축하는 코드다.
```ts
interface Animal {  
  name: string;  
  endangered: boolean;  
  habitat: string;  
}

const leopard: Animal = {  
  name: 'Snow Leopard',  
  endangered: false,  
  habitat: 'tundra'  
}
```
위 코드는 네 가지 문제가 있다.
- name: 매우 일반적인 용어라 동물 학명인지 일반적 명칭인지 알 수 없음
- endangered: 멸종 위기를 표현하기 위해 boolean 타입을 사용하는 것이 알기 어렵다. 멸종 위기인지 멸종된건지 알 수가 없음
- habitat: 너무 범위가 넓은 string 타입
- 객체의 변수명이 leopard이지만, name 속성의 값은 'Snow Leopard'로 객체의 이름과 속성의 name이 다른 의도로 사용된 건지 불분명하다.

이 문제를 해결하려면 해당 속성을 작성한 사람을 찾아서 의도를 물어 봐야 한다.

다음 코드의 타입 선언은 의미가 분명하다.
```ts
interface Animal {  
  commonName: string;  
  genus: string;  
  species: string;  
  status: ConservationStatus;  
  climates: KoppenClimate[];  
}  
type ConservationStatus = 'EX' | 'EW' | 'CR' | 'EN' | 'VU' | 'NT' | 'LC';  
type KoppenClimate = 'AF' | 'Am' | 'ET' | 'EF' | 'Dfd';  
  
const snowLeopard: Animal = {  
  commonName: 'Snow Leopard',  
  genus: 'Panthera',  
  species: 'Uncia',  
  status: 'VU',  
  climates: ['ET', "EF", "Dfd"],  
}
```
데이터를 훨씬 명확하게 표현되었다. 정보를 찾기 위해 사람에 의존할 필요가 없다.

코드로 표현하고자 하는 모든 분야에는 주제를 설명하기 위한 전문 용어들이 있다. 자체적으로 용어를 만들어 내려고 하지 말고, 해당 분야에 이미 존재하는 용어를 사용해야 한다.

### 타입, 속성, 변수에 이름을 붙일 때 지켜야할 세 가지 규칙
- 동일한 의미를 나타낼 때는 같은 용어를 사용한다.
- data, info, thing, item, object, entity 같은 모호하고 의미 없는 이름은 피해야 한다.
	- 예를 들어 entity라는 용어가 해당 분야에서 특별한 의미를 가진다면 상관없다.
- 이름을 지을 때는 포함된 내용이나 계산 방식이 아니라 데이터 자체가 무엇인지를 고려해야 한다.
	- INodeList 보다 Directory가 더 의미있는 이름이다.

### 참고사항
해당 분야의 용어를 사용하는 것은 적절하지만, 코드를 읽는 사람도 프로그래머라는 사실을 잊으면 안된다.
#### 클린코드
p34에서 이름을 지을 때 다음과 같은 주의를 표하고 있다.

> 해법 영역에서 가져온 이름을 사용하라

코드를 읽는 사람도 프로그래머라는 사실을 명심하며, 전산, 알고리즘, 패턴, 수학 용어 등을 사용해도 괜찮다.
그러나 모든 이름을 문제 영역(domain)에서 가져오는 정책은 현명하지 않다.
같은 개념을 다른이름으로 이해하던 동료들이 매번 고객에게 의미를 물어야하기 때문이다.

> 문제 영역에서 가져온 이름을 사용하라

만약 적절한 프로그래머 용어가 없다면 문제 영역(domain)에서 이름을 가져온다. 그러면 보수하는 프로그래머가 분야 전문가에게 의미를 물어 파악할 수 있다.

우수한 프로그래머와 설계자라면 해법 영역과 문제 영역을 구분할 줄 알아야 한다. 문제 영역 개념과 관련이 깊은 코드라면 문제 영역에서 이름을 가져와야 한다.

#### DDD(도메인 주도 설계)
도메인 주도 설계란 소프트웨어를 특정 도메인과 일치하도록 모델링하는 설계 아키텍처이다.

**도메인 주도 설계의 원칙과 패턴**
- 유비쿼터스 랭귀지: 도메인에 대해 모든 관계자가 공통으로 사용하는 언어
- 어그리거트
- 도메인 서비스
- 리포지토리

도메인 주도 설계의 가장 큰 장점은 의사소통과 협업이 강화된다는 것이다. 도메인 전문가와 개발자 간의 의사소통과 협업을 강화한다.
비즈니스 도메인의 용어와 개념을 코드에 반영함으로써, 비즈니스 전문가와 개발자는 동일한 언어로 사용하며 서로간의 의사소통을 원활하게 할 수 있게 된다.