**1\. Vue 인스턴스란 ?**

루트 Vue 인스턴스 (이하 Vue 인스턴스) 란 생성된 Vue Object로 뷰 어플리케이션 시작을 위한 필수 요소다.

HTML 서식을 컴파일하고 데이터 초기화 및 생성 등 많은 동작을 수행한다.

Vue 앱은 **new Vue({ /\*옵션\*/})**를 통해 만들어진 Vue 인스턴스로 구성되며

선택적으로 중첩이 가능하고 재사용 가능한 컴포넌트 트리로 구성되어 있다. 몇 가지 옵션에 대해 알아보자.

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


* methods와 computed의 큰 차이점은 캐싱 여부이다. computed로 적용된 속성은
내부적으로 데이터를 캐싱하여 바로 리턴하는 반면 method는 매번 새로 호출하여 계산한다.

html 상에서 computed는 마치 변수처럼 쓰이고 methods는 함수처럼 쓰인다.
sum이라는 함수가 있다면 그 콧수염 표기법으로 computed는 {{sum}}, methods는 {{sum()}}으로 쓴다.
```

data 속성은 별도의 프록시를 통해 관리되고 있다. data 객체의 속성들은 자동적으로 Vue 인스턴스 객체의 속성이 된다. Vue 객체를 통해 프록시 처리된 data에 접근하는 방법을 알아보자.

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

위의 예시처럼 data 객체 내부 속성은 Vue 인스턴스가 할당될 vm(변수명).name(속성)으로 바로 접근할 수 있다.

변수명.속성 = 원하는 값; 이런 방식으로 새로운 값을 넣어주는 것도 가능하다(=>Setter)

※ 주의 : option 속성이나 콜백에 화살표 함수 사용을 지양할 것.  
화살표 함수는 부모 컨텍스트에 바인딩되기 때문에, this 컨텍스트가 호출하는 Vue 인스턴스에서   
사용할 경우 Uncaught TypeError: Cannot read property of undefined 또는  
Uncaught TypeError: this.myMethod is not a function와 같은 오류가 발생함.

---

**2\. Vue 생명주기 (라이프사이클)**

Vue 생명주기란 (이하 라이프사이클) Vue 인스턴스의 생성부터 메모리에서 삭제되기 전까지 일련의 과정을 말한다.

사용자는 Vue 인스턴스를 생성할 때 전달했던 option을 통해 라이프사이클 훅을 정의할 수 있다. (훅은 Vue 라이브러리 코드의 일부, 실행되는 함수를 의미)

_생명주기는 **생성 (Create) - 초기화 (Mount) - 갱신 (Update) - 파괴 (Destory)** 4단계로 나뉜다._

1) 생성(Create) 단계
<!--
[##_Image|kage@bls5yb/btqB2vJjS1h/DexcK2fekJpSLlAovOWbTK/img.jpg|alignLeft|data-filename="create.jpg" data-origin-width="1199" data-origin-height="619" width="769"|11||_##]
-->

Create 단계에서 실행되는 훅(Hook)들이 라이프사이클 중 가장 처음 실행된다.

아직 컴포넌트가 DOM에 추가되기 전이기 때문에 DOM에 접근하거나 this.$el를 사용할 수 없다.

\- beforeCreate : 모든 훅 중에 가장 먼저 실행되는 훅. Vue 인스턴스가 초기화 된 직후 발생.

아직 data와 event에도 접근할 수 없음.

\- created : data와 events가 활성화되어 접근할 수 있다. 템플릿과 가상돔은 렌더링되지 않은 상태

```
var app = new Vue({
  el: '#app',
  data(){
    return {
      message : 'hello';
    }
  },
  beforeCreate : function(){
    console.log(this.message); // undefined
  },
  created : function(){
    console.log(this.message); // hello
  }
})
```

2) 초기화(Mount) 단계

<!-- 
[##_Image|kage@ceyN4l/btqBZNLnG4H/4TOoOab0tztPK9CkYIPkG0/img.jpg|alignLeft|data-filename="mount.jpg" data-origin-width="1198" data-origin-height="1249" width="580" height="605"|||_##]
-->

컴포넌트가 DOM에 추가될 때 실행되는 훅이다. 서버 사이드 렌더링을 지원하지 않는다. 

렌더링될 때 DOM을 변경하고 싶다면 사용하되 컴포넌트 초기에 data가 세팅되야 한다면, created 훅을 이용하는 것이

좋다.

\- beforeMount : 컴포넌트 초기에 data가 세팅되어야 하면 created를, 렌더링 되고 DOM을 변경해야 한다면

mounted 훅을 사용하기 때문에 거의 사용하지 않는 라이프사이클 훅이다.

\- mounted : 가상 DOM의 내용이 실제 DOM에 부착되고 난 이후 실행되므로 대부분 모든 요소에 접근이 가능하다.

<!--
[##_Image|kage@HhoBy/btqB1c409SM/CJikKaJxVBnMuKm9joOEWk/img.png|alignLeft|data-filename="mounted.png" data-origin-width="537" data-origin-height="629"|||_##]
-->

위 그림처럼 Created훅은 부모 → 자식 순서로 실행되지만 Mounted훅은 자식 → 부모 순서로 실행된다.

부모 컴포넌트는 자식 컴포넌트가 모두 DOM에 추가 된 후에 mounted 훅이 실행된다.

3) 갱신(Update) 단계

<!--
[##_Image|kage@ba7Mc8/btqB1GqZ6pI/rplYBYr0HKyPUiPWw4vyZk/img.jpg|alignLeft|data-filename="update.jpg" data-origin-width="822" data-origin-height="580" width="599"|||_##]
-->

컴포넌트에서 사용되는 반응형 속성들이 변경되거나 어떤 이유로 재 렌더링됐을 때 실행된다.

변경된 값으로 어떠한 작업을 해야할 때 유용하게 사용되는 훅이다.  서버 사이드 렌더링은 지원하지 않는다.

\- beforeUpdate : 컴포넌트에서 사용되는 data 값이 변해서 DOM에도 그 변화를 적용시켜야 할 때, 변화 직전

호출되는 훅이다. 가상 DOM을 렌더링하기 전이지만 이 값을 이용해 작업을 할 수는 있다.

\- updated : 가상DOM을 렌더링하고 실제 DOM이 변경된 이후 호출되는 훅이다. 변경된 data가 DOM에도 적용된 상태다.

변경된 값들을 DOM을 이용해 접근하고 싶다면 여기서 하는 것이 적절, \*but, data를 변경하는 것은

무한 루프를 일으킬 수 있으니 데이터를 직접 바꾸진 말 것!

4) 파괴(Destroy) 단계
<!--
[##_Image|kage@bjPe7d/btqBZNYXb3p/Kso0eNPmyKwLylMGGpvpBk/img.jpg|alignLeft|data-filename="destory.jpg" data-origin-width="1199" data-origin-height="693" width="571"|||_##]
-->


컴포넌트가 제거될 때 실행되는 단계이다.

\- beforeDestroy : Vue 인스턴스가 제거되기 전 호출되므로 컴포넌트는 원래 모습과 모든 기능들을 그대로 가지고 있다.

이벤트 리스너를 제거하는 등 인스턴스가 사라지기 전 해야할 일들을 처리하는 단계다.

\- destoryed : 컴포넌트가 제거된 후 호출되는 훅이다. 컴포넌트의 모든 이벤트 리스너 (click, change 등)와 디렉티브 (v-model, v-show 등)의 바인딩이 해제되고 하위 컴포넌트도 모두 제거된다.