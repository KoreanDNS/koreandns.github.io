---
title: MSSQL 저장 프로시저 분석
author: Koreandns
date: 2020-08-14 04:40:00 +0800
categories: [MSSQL, procedure]
tags: [MSSQL, procedure]
---



저장 프로시저에 대해서 알아 보자.

![](..\..\img\procedure1.png)



저장 프로시저란 ?

프로그래밍 언어의 메소드라고 생각하면 된다.

매개 변수, 출력 매개변수, 리턴 값을 가질 수 있다.

저장 프로시저는 EXECUTE문을 사용하여 호출해야 하며, 테이블에 직접 액세스 하여 사용하지 말고 저장 프로시저를 만들어서 사용하는게 올바른 방법이다.



---



저장 프로시저를 사용하면 좋은점은?

1. 캡슐화

   - 저장 프로시저내의 로직이 변경된다 하더라도 매개 변수의 형태의 변화가 없으면 기존 저장 프로시저 사용하는 곳은 영향을 받지 않는다.

2. 성능

   - 저장된 실행계획을 재사용하여 CPU와 메모리 사용량을 절약하고, 코드의 구문 분석, 이름 확인 및 최적화에 걸리는 시간을 단축할 수 있다.

3. 네트워크 트래픽 최소화

   - C++에서 데이터베이스에 내용을 전달해야 한다고 가정해 보자. 저장 프로시저를 사용하지 않으면 쿼리문이 길 경우 문자열의 길이 만큼 비용이 들어가게 될 것이다. 하지만 저장 프로시저의 경우 이름과 매개변수만 제공되기 때문에 훨씬 비용이 감소하게 될 것이다.

4. 보안 계층으로 사용

   - 개체에 대한 권한을 직접 부여하는게 아니라 저장 프로시저에 대한 실행 권한만을 부여하여 개체에 대한 불필요한 접근을 제한할 수도 있다.

   

---



![](..\..\img\procedure2.png)



일반적인 SQL 쿼리문이 처음 실행 될 때의 과정을 단계적으로 보여주고 있다.

문법을 검사하여 불필요한 구문을 제거해서 표준화를 만들고 SQL 쿼리문을 실행할 수 있는지 권한 확인을 하여 최적의 실행 계획을 만들어서 컴파일 한 후 캐시에 등록된 것을 사용하고 있다.

다음에 똑같은 쿼리문을 사용하면 캐시에 등록된 것을 사용하기 때문에 즉시 수행 되지만

만약 쿼리문이 조금이라도 변경된다면 방금 전 작업을 똑같이 수행하므로 CPU와 메모리 비용 부담이 커질 것이다.

----



![](..\..\img\procedure3.png)



프로시저 만드는 과정을 단계적으로 보여주고 있다.

문법을 검사한 후 해당 개체의 이름이 존재하는지 확인을 안 한다는 말이 프로시저 내부 로직에서 참조하는 개체 이름이 정말 존재 하지 않아도 지연된 이름 확인으로 무시된다는 것이다.

그리고 만들 수 있는지 권한 확인을 한 후 시스템 테이블에 개체의 정보를 저장한다.

----



![](..\..\img\procedure4.png)



만들었던 프로시저를 사용할때의 과정들을 단계적으로 보여주고 있다.

처음에 사용할땐 개체가 존재하는지 확인하고 저장 프로시저 실행 권한을 확인 한 후 최적화된 실행 계획을 컴파일하여 캐시에 등록한 후 사용한다.

이후에 다시 사용할땐 캐시에 등록되어 있으면 즉시 실행하고 없으면 처음 단계부터 다시 시작한다.

저장 프로시저는 구문이 변경되지 않으므로 동일한 쿼리문 사용으로 인해 저장된 실행 계획을 재사용할 가능성이 높으므로 CPU와 메모리 사용량을 줄 일 수 있는 강력한 이점이 있다.

----



프로시저 사용시 단점

계속 업데이트 예정..