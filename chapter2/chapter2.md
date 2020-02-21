**1\. Vue 인스턴스란 ?**

루트 Vue 인스턴스 (이하 Vue 인스턴스)란 뷰 어플리케이션 시작을 위한 필수 요소다.
HTML 서식을 컴파일하고 데이터 초기화 및 생성 등 많은 동작을 수행
Vue 앱은 **new Vue({ /\*옵션\*/})**를 통해 만들어진 Vue 인스턴스로 구성

```
new Vue({
  el : '#app',    // 대상인 html 요소
  template : '',  // 화면에 표시할 html,css 등의 마크업 요소를 정의
  data: {},       // 화면에 사용되는 data를 반환하는 함수 또는 객체, 데이터 변경 시 화면 재 렌더링
  props: [],      // 부모 component에서 전달받은 속성의 array or object
  methods : {},   // this 컨텍스트를 Vue 인스턴스에 바인딩
  computed : {},  // Vue 객체에서 어떤 계산 결과를 속성으로 이용하기 위해 사용
  watch : {}      // Vue 인스턴스에 데이터가 변경되는 시점에 호출
});


* methods와 computed의 큰 차이점은 캐싱 여부, computed로 적용된 속성은
내부적으로 데이터를 캐싱하여 바로 리턴하는 반면 method는 매번 새로 호출하여 계산한다.
```

data 속성은 별도의 프록시를 통해 관리, data 객체의 속성들은 자동적으로 Vue 인스턴스 객체의 속성이 된다. 
Vue 객체를 통해 프록시 처리된 data에 접근하는 방법을 알아보자.
<br/>
<br/>

▶ var vm = new Vue({..})와 같이 선언된 경우 vm을 통해 접근
```
var vm = new Vue({
  data : {
    name:"하이",
    age :20
  }
})

vm.name = "이서";
vm.age = 18;
```

▶ $data를 사용하여 직접 접근
```
vm.$data.name = "이서";
vm.$data.age = 18;
```

<br/>
위의 예시처럼 data 객체 내부 속성은 Vue 인스턴스가 할당될 vm(변수명).name(속성)으로 바로 접근가능
변수명.속성 = 원하는 값; 이런 방식으로 새로운 값을 넣어주는 것도 가능하다(=>Setter)


---


**2\. Vue 생명주기 (라이프사이클)**


Vue 생명주기란 (이하 라이프사이클) Vue 인스턴스의 생성부터 메모리에서 삭제되기 전까지 과정
사용자는 Vue 인스턴스를 생성할 때 전달했던 option을 통해 라이프사이클 훅을 정의할 수 있다. 
(훅은 Vue 라이브러리 코드의 일부, 실행되는 함수를 의미)
<br/>
생명주기는 **생성 (Create) - 초기화 (Mount) - 갱신 (Update) - 파괴 (Destory)** 4단계로 나뉜다.
<br/>
<br/>

https://yi-seo.tistory.com/25?category=794412
자세한 내용은 블로그 참조해주세요!
