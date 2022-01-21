## MVVM은 해결하지 못한 것들

상태 문제 

프로그래밍은 상태 제어와 깊은 연관성이 있습니다. 화면에 나타나는 모든 정보, 프로그래스바 상태, 버튼 활성화 등 무수한 상태들로 구성되어 있습니다. 

하지만 상태를 관리하기 힘들어지고 의도하지 않은 방향으로 제어가 된다면, 우리는 이것을 상태 문제라고 합니다.

예) 서버의 응답으로 리스트를 성공적으로 출력했지만 프로그래스바는 계속 보이는 상태

<img src="https://user-images.githubusercontent.com/54509842/146784964-51e3fd85-0f3a-48db-8d38-c797a8ab86ef.PNG" width="20%"></img>

그럼 이러한 상태 문제는 왜 일어날까요?

1) 분산된 데이터

화면에 표시해야 할 데이터가 view, viewmodel, model 등에 분산되어 관리하지 않으면 상태의 충돌이 일어날 수 있기 때문입니다.

2) 복잡한 데이터 흐름

네트워크 통신 또는 복잡한 데이터 처리, 여러 비동기 작업이 섞이고 서로 앱의 상태를 변화하려고 하면 상태의 충돌이 발생합니다.

이런 관점에서 **MVI는 이런 상태의 충돌을 피하고, 데이터 흐름의 추적을 쉬운 앱을 만드는 것**을 목표로 합니다.

## MVI가 어떻게? 

그럼 MVI에서는 뭘로 이것을 해결하려 할까요?

**하나의 불변 객체를 사용하고, 데이터 흐름을 단방향으로 설계**하여 이런 문제를 해결합니다.

이런 상태 지향 어플리케이션을 설계, 개발하는 것으로 MVI는 문제를 해결합니다.

<img src="https://user-images.githubusercontent.com/54509842/146784976-c0d3b440-9c1a-4efa-9de6-ee7fe0da3407.png"></img>

사람과 기기가 작용하는 것을 다음과 같이 표현할 수 있습니다. 이때 우리는 입력과 출력을 위한 중간 단계들이 필요합니다. 

<img src="https://user-images.githubusercontent.com/54509842/146784985-65fc4df6-83fe-4973-9fff-c26f797e285c.png"></img>

그리고 이러한 작용들을 수학적인 관점으로 함수를 사용해 표현하면 위 그림과 같습니다.

<img src="https://user-images.githubusercontent.com/54509842/146785016-6a081b3b-7a7c-47a3-b0d5-753b23743cbb.PNG"></img>

여기서 user는 프로그래밍의 대상이 아니므로 제외합니다.

<img src="https://user-images.githubusercontent.com/54509842/146785022-d4cd5e3a-7a9f-4ae9-9531-000637371aef.PNG"></img>

이런 모양이 되고, intent는 반복되니 제외하겠습니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785027-ae457d4c-b1cb-44b5-959e-0c837b92f0ff.PNG"></img>

결과적으로 이런 모습을 가지게 됩니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785036-54ab54c9-2220-470c-81fa-fd0039e7cadb.PNG"></img>

intent > model : intent는 의도를 가지고 model에게 앱에 변화를 주려는 의도를 입력으로 전달합니다. 

model > view : 이런 의도를 받고 model 함수는 model을 출력합니다.

model은 출력으로 상태를 나타내고, view함수에서는 리턴 값이 따로 없이 화면에 상태에 맞는 view를 렌더링 합니다. 
user는 이런 바뀐 view를 보고 다시 interaction을 시도합니다. 

이 Model, View, Intent가 남긴 것을 우리는 **MVI**라고 부릅니다.

정리하자면 MVI는 **의도를 상태로 만들어 표시하는 것**이라 정리할 수 있습니다. 

여기서 view가 의미하는 것은 그동안 나온 mvX 시리즈에서 나온 view와 크게 다르지 않습니다. 

다만, mvc, mvp, mvvm의 controller, presenter, viewmodel이 앱의 중요 로직을 처리하는 실제 컴포넌트를 지칭하는 의미였던 것과는 다르게 mvi에서의 I(intent)는 단순히 앱의 상태를 바꾸려는 의도라는 점에서 차이가 있습니다. 

이처럼 이름의 중요한 한자리를 다른 시리즈와는 다르게 구조적인 컴포넌트에 할당하지 않았다는 것은 MVI는 조금 다른 시각으로 접근하고 있음을 시사합니다.

안드로이드를 어떤 구조로 구성해야 할까 > **앱의 상태와 데이터 흐름을 어떻게 다룰까**

이렇듯 구조적인 문제가 주안점이 아닌 MVI는 **데이터 흐름을 어떻게 다룰지에 대한 패러다임**에 오히려 가깝습니다. 

(MVI예제를 찾으면 presenter나 viewmodel을 갖고 있는 코드들을 쉽게 볼 수 있는 이유가 이렇습니다.)

## 왜 MVI?

그럼 왜 MVI를 사용해야 할까요?

아키텍처를 선택하는데 필요한 두 가지 조건을 다음과 같이 생각해보겠습니다. 

+ 유의미한 테스트가 가능할 것
+ 디버깅이 쉬울 것

이런 지극히 평범하고 수용 가능한 조건이고, 그렇기 때문에 앞서 나온 mvp와 mvvm에서도 이를 만족했습니다. 

하지만 조건이 있었습니다. 잘 짜야 했다는 것입니다. 

여기서 나온 **MVI의 강력한 사용 이유는 누가 개발하더라도 일정한 수준 이상의 결과물이 보장된다는 것**입니다. 

+ Intent : 의도**만**
+ Model : 상태를 **만들어내는 것**
+ View : 상태를 받아 표시**만**

Intent와 View는 각각의 역할이 정형적이지만, Model에서 상태를 정규화 할 수 있다면, 누가 개발하더라도 일정한 결과물을 낼 수 있을 것입니다. 

### Box란

<img src="https://user-images.githubusercontent.com/54509842/146785053-dea0e93d-e394-4275-8efe-8c41e56568a7.PNG"></img>

여기서 Model 안에서 상태를 잘 만들어 내며 MVI 아키텍처로 안드로이드 어플을 개발할 수 있도록 도와주는 Kotlin 기반 프레임워크가 바로 **Box**입니다.

+ BluePrint : Box에서 MVI의 개발을 위해 Intent 처리와 Model 생성을 정규화시키기 위해 가지고 있는 개념입니다.
이것은 말 그대로 앱의 설계도 역할을 합니다. 
앱에서 발생하는 이벤트와 앱 상태의 상호 관계를 DSL로 선언해주기만 하면, 나머지는 BOX가 알아서 처리하도록 설계되어 있습니다.

+DSL(Domain Specific Language) : 특정 영역을 타켓팅 하고 있는 언어

Box를 통해 아까 본 user가 기기와 interaction하는 그림을 보시겠습니다.

### SideEffect

<img src="https://user-images.githubusercontent.com/54509842/146785063-25211d8f-9829-42eb-9c73-c0e5dc1ce465.png"></img>

그 전에 위 사진과 함께 짚고 넘어가야 하는 개념이 있습니다. 바로 MVI에서의 SideEffect입니다. 

하지만 우리가 개발하는 앱은 이런 단조로운 cycle만 존재하지는 않습니다. **사용자의 의도가 앱의 상태를 전환하기가 어려운 작업이 있다**는 뜻입니다. 

시간이 오래걸리는 백그라운드 작업, API 통신, 또는 토스트나 다이얼로그 노출과 같은 인스턴트 뷰가 이에 해당합니다. 

MVI에서는 이를 **SideEffect**라는 개념으로 처리합니다. 

원래의 MVI cycle과는 무관하게 처리된 SideEffect는 작업이 끝난 이후에 **새로운 intent가 되어 작업에 합류**합니다. 

### Box

<img src="https://user-images.githubusercontent.com/54509842/146785068-c0ebb6f2-2bd2-4021-99a0-0ce0a42fd2b5.PNG"></img>

Box에서는 Intent를 보다 직관적으로 BoxEvent라고 합니다. Model은 BoxState라고 합니다. 
SideEffect는 우리가 개발할 때 접하는 SideEffect와는 의미가 차이가 있지만, BoxSideEffect라고 합니다. 

+ Intent > BoxEvent
+ Model > BoxState
+ SideEffect > BoxSideEffect

이렇게 3가지가 Box의 기본 컴포넌트입니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785106-22516e42-4e8f-4528-8804-7463f4523569.PNG"></img>

이 컴포넌트들을 사용해서 Intent와 Model의 역할을 수행하는 것을 BoxVM이라고 합니다. 

일반적인 mvvm아키텍처에서 viewmodel이라고 생각하시면 됩니다. 

실제로 BoxVM은 viewmodel을 구현해서 만들었습니다. 

View의 역할을 수행하는 것을 BoxView라고 합니다. 

대표적으로 안드로이드에서는 activity와 fragment를 view라고 들 수 있습니다. 

Box에서는 이를 위해 각각 **BoxActivity와 BoxFragment**를 제공하고 있습니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785113-ea8c7be1-ece9-4de6-9b0e-ec5d3a5abfb8.PNG"></img>

Event가 State를 일으키고, State는 SideEffect를 일으킵니다. 

따라서 어떤 Event가 어떤 State를 만들고 어떤 SideEffect를 일으키는지에 대한 정의가 있는 것이 바로 **BluePrint**입니다. 

이런 BluePrint를 **BoxVM** 즉 viewmodel에 선언합니다. 

그리고 이런 BoxVM은 BoxView, 즉 activity에 포함되어 동작합니다.

## 코드
이제 다음과 같은 앱을 만든다고 가정하겠습니다. 
1. 서버에 이미지 데이터를 요청한다. 
2. 응답으로 가져온 이미지를 목록으로 화면에 표시한다. 
3. 불러오는 도중은 진행중 상태를 표시하고, 로딩에 실패한 경우 에러 화면을 표시한다. 
4. 에러 화면이 표시된 경우 이미지 요청을 재시도 할 수 있다. 

### BoxState

<img src="https://user-images.githubusercontent.com/54509842/146785119-faaebb68-6952-446a-8a82-24a82d42dc0c.PNG"></img>

BoxState를 먼저 만들겠습니다. 

상태는 다음과 같습니다. 

- 이미지 데이터 로딩 상태
- 성공 시 화면에 표시되는 상태
- 실패 시 화면에 표시되는 상태

onProgress : true 일 경우 프로그래스 바를 그립니다.

onError : true 일 경우 실패 화면을 표시합니다.

source : 실패 시 재시도를 위해 필요한 데이터입니다.

images : 화면에 표시할 데이터 입니다. 

### BoxEvent

<img src="https://user-images.githubusercontent.com/54509842/146785126-9b3cb733-0256-4ff9-a752-e7638efee00a.PNG"></img>

다음은 BoxEvent입니다. 

BoxEvent는 만드는 여러방법이 있겠지만, SealedClass를 사용하는 것을 권장합니다. 

RequestImages : 이미지 데이터를 서버에 요청합니다. 앱이 초기화 되면서 발생합니다. 

OnImagesFetched : 요청한 이미지를 성공적으로 받았을 때 발생합니다. 

OnError : 요청이 실패했을 때 발생합니다. 

Retry : 실패 시 재시도를 눌렀을 경우 발생합니다. 

### BoxSideEffect

<img src="https://user-images.githubusercontent.com/54509842/146785133-b0777931-07a2-43b5-9fcb-176c3b028855.PNG"></img>

마지막으로 SideEffect에 대한 정의입니다. 

RequestImages : 서버에 이미지를 요청하는 경우 

이것으로 Box의 기본적인 컴포넌트의 구성은 끝이 났습니다. 

아래에서는 BluePrint를 담고 있는 BoxVM과 BoxView를 보시겠습니다. 

### BoxVM

<img src="https://user-images.githubusercontent.com/54509842/146785139-4ac10e1a-671f-41d1-b111-a680d6c6bbe6.PNG"></img>

BoxVM을 상속 받게 되면 다음과 같이 BluePrint를 선언하도록 되어있습니다. 

onCreateBluePrint()라는 함수로 BluePrint를 선언했는데, 한번 보겠습니다.

<img src="https://user-images.githubusercontent.com/54509842/146785143-fcae6225-c184-4037-b4c6-021d7124bca0.PNG"></img>

먼저 눈에 띄는 점은 onCreateBluePrint()가 확장함수로 작성됐다는 점입니다. (UnitTest와 관련이 있습니다.)

+확장함수 : 기존 클래스에 메소드를 추가하는 것입니다. 

이러한 개념을 사용하면, 외부라이브러리가 제공하는 클래스 자체를 변경할 수는 없지만, 이를 통하여 개발자가 새로운 함수들을 개발하여 추가 할 수 있고, 마치 원래 라이브러리인 것처럼 사용할 수 있습니다. 
상속과 비슷하지만, 상속을 하지 않아도 가능합니다.

이미지 요청 이벤트를 작성해보겠습니다. 

BluePrint는 BoxVM의 blueprint (builder)로 작성이 가능합니다.

이 builder는 이벤트를 정의하는 **on 함수**와 이벤트를 정의 받아 새로운 상태를 정의하는 **to 함수**로 이루어집니다. 

위 경우 RequestImages라는 이벤트를 받아 copy()라는 바뀐 상태와 필요하다면 발생시킬 이미지 요청을 위한 SideEffect (MainSideEffect.RequestImages())를 선언합니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785151-978b7c53-46b2-40ee-b7b1-0c2de321c4bb.PNG"></img>

그리고 바로 이미지 요청을 위해 발생 시킨 SideEffect를 정의하겠습니다. 

코드를 보시면 **io 함수**와 **SideEffect**가 선언되어 있습니다. 

SideEffect는 해당 SideEffect가 수행되어야 할 Dispatcher에 따라서 각각 **main() / background() / io()**로 선언합니다. 

위와 같은 경우는 이미지 요청에는 Dispatcher.io를 사용할 예정이기 때문에 io 함수를 사용했습니다. 

그렇기 때문에 Dispatcher.io에서 수행되므로 requestImages 함수는 **suspend함수**로 구현이 가능합니다.

<img src="https://user-images.githubusercontent.com/54509842/146785158-b7826158-43c7-464b-a827-5f718fae41d9.PNG"></img>

앞서 to 함수에서 전달한 SideEffect는 SideEffect 함수 내의 it으로 전달됩니다. 

전달 받은 it과 함께 requestImages 함수도 구현해보겠습니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785159-7a0e456b-029c-4f1e-a2d8-90076d7f1c1f.PNG"></img>

requestImages 함수 앞서 말씀 드린 대로 네트워크를 사용하여 통신하기 때문에 suspend함수로 구현되어 있습니다. 

중요한 것은 **State, Event, SideEffect에 대한 정의가 아니기 때문에** blueprint가 아닌 BoxVM에 직접 구현했다는 점입니다. 

또한 SideEffect에서 설명 드린 것과 같이 SideEffect는 새로운 Event를 결과로 반환할 수 있습니다. (MainEvent 반환)

<img src="https://user-images.githubusercontent.com/54509842/146785188-9806429c-c85d-438a-a385-679b49ac9b51.jpg"></img>

<img src="https://user-images.githubusercontent.com/54509842/146785206-bcefc24a-e379-47db-bf49-b501dddea444.PNG"></img>

위 코드는 성공과 실패를 포함한 나머지 Event를 정의한 것입니다. 

1번에서 이미지를 요청하면서 함께 2번 SideEffect를 발생시킵니다. 

2번의 SideEffect에서는 그 결과가 3번(성공)인지, 4번(실패)인지에 따라 requestImages에서 분기하여 결과를 처리합니다. 

3번(성공)이면 이미지를 표시합니다.

4번(실패)이면, 실패한 화면에서 재시도를 하면 5번(재시도)를 발생시키며 기존에 표시하고 있던 이미지를 지우기 위해서 images를 초기화해주는 것은 다르지만, 1번과 같이 SideEffect를 다시 함께 발생시킵니다. 

### BoxView

BoxView만 구현되면 샘플 어플이 완성됩니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785209-1f5c6cff-43cb-4a71-b80e-5ad34aa49598.PNG"></img>

BoxView는 BoxActivity나 BoxFragment를 상속하여 사용할 수 있고, 여기서는 Boxactivity를 이용해 구현했습니다. 

Box에서 activity는 테스트 작성의 편의와 관심사의 분리를 위해 많은 역할을 하지 않습니다. 

단지 Box구현을 위한 컴포넌트의 선언 정도만 합니다.

layout : 화면이 없는 경우 생략할 수 있지만, DataBinding을 필수로 사용해야만 합니다.

renderer : State를 전달받아 화면을 그립니다. 화면이 없으면 생략 가능합니다. 

viewInitializer : 화면에 사용할 뷰를 초기화 시킵니다. 혹은 앱의 첫 이벤트를 발생시킬 때 사용합니다. 

vm : 안드로이드 viewmodel을 확장하여 구현했기 때문에 viewModelProvider를 사용하여 구현 받을 수 있습니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785217-efab5eeb-3f70-46f1-952f-90e333ec2908.PNG"></img>

Box는 DataBinding을 강제하고 있습니다. 

State를 선언했을 때 선언했던 데이터들이고, 이 변수들이 추후에 renderer에서 값이 할당되고, 그 할당된 값이 view에 표시됩니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785229-eb5a52be-d689-4890-a79b-9c3d31edea74.PNG"></img>

NascaView : 고용량 이미지를 출력해주는 MyRealTrip의 오픈소스 

Progressbar : 프로그래스바

Button : 재시도 버튼 

TextView : 실패 문구

<img src="https://user-images.githubusercontent.com/54509842/146785246-a1757bd7-d21f-483f-befc-dd8b5ddfea0b.PNG"></img>

ViewInitiallizer 입니다. initializeView 함수는 BoxActivity가 생성된 후에 1회 호출됩니다. 

따라서 화면 초기화에 필요한 이벤트나 view의 초기화에 적합합니다. (리싸이클러뷰의 초기화가 여기서 이루어지면 됩니다. ) 

이때, **v: BoxAndroidView를 이용하여 view에 접근할 수 있습니다.** 

<img src="https://user-images.githubusercontent.com/54509842/146785250-e5dedfeb-955c-4b8e-96f9-937fc9dcb154.PNG"></img>

마지막 Renderer에서는 화면을 그리는 것을 담당합니다.

Initialize에서 봤던, v: BoxAndroidView와 s: MainState (그리기 요청 받은 앱의 상태) 그리고 vm을 전달받아서 화면을 그립니다. 

Layout에 있는 것들을 전달 받은 state를 이용해서 채워 넣는 모양을 볼 수 있습니다.

## Test

앞서 본 Box는 BluePrint에 정의된 State, Event, SideEffect의 상호 관계에 따라 동작합니다. 

그리고 거기서 발생하는 새로운 State에 따라 view를 새롭게 구성합니다. 

그렇기 때문에 **Event에 따른 bluePrint의 변화와 State에 따른 Renderer의 변화를 검증하는 것**은 그것이 유의미한 검증입니다.

단순히 Mock의 테스트 코드로 진행하는 것은 까다롭기 때문에 Box에서 제공하는 기본 테스트 클래스를 이용하여 진행하는 것을 권장합니다.

대표적으로 두가지가 있습니다. 

testIntent() : mock vm으로 이벤트를 전달해서 결과물을 리턴받습니다. 

실제 코드는 VM으로 전달된 이벤트는 정의된 스펙에 따라 새로운 State가 되고, SideEffect로 각각 수행되지만, testIntent는 결과를 리턴합니다. 의도한 상태가 됐는지 확인할 수 있습니다. 

do~SideEffect() : testIntent로 받은 결과물에 SideEffectr가 있는 경우, 그 SideEffect를 실행 시킬 수 있습니다. 


<img src="https://user-images.githubusercontent.com/54509842/146785258-5eb14577-49a8-4066-91ea-01c940ac471e.PNG"></img>

이 때, mockBluePrint라는 함수명을 사용했지만, bluePrint는 mocking이 까다롭기 때문에 위 테스트 코드에서는 실제 BluePrint를 불러오고 있다고 이해하시면 됩니다. 


<img src="https://user-images.githubusercontent.com/54509842/146785268-1b16ec7e-4778-4cb5-be53-88ccb51186b1.PNG"></img>

위 테스트 코드로 변화된 State, SideEffect가 기대한 값인지, 그 경우 vm의 특정한 함수가 호출되었는지 확인할 수 있습니다. 


<img src="https://user-images.githubusercontent.com/54509842/146785272-18b8da95-e4fb-4ac0-bacd-aed368069c74.PNG"></img>

성공 실패에 대한 코드는 다음과 같이 테스트 해볼 수 있습니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785277-4611ae87-a9de-4a09-88a8-ff13995ac960.PNG"></img>

이전까지는 BluePrint의 테스트를 끝냈고, 이것은 renderer에 대한 테스트 코드입니다. 

Renderer는 사용되는 view와 state에 따라 많이 달라지기 때문에 규격화 시키기가 어렵습니다. 

하지만 Box의 renderer는 비교적 수월합니다. 다른 것 없이 render만 수행하기 때문입니다. 

이 중 State를 제외한 BoxView와 vm은 mock으로 선언하는 것은 쉽습니다. 

<img src="https://user-images.githubusercontent.com/54509842/146785286-800f9d60-8669-499a-9a0a-35ab38b522ae.PNG"></img>

위와 같이 State 값을 바꿔주면서 알맞게 renderer가 binding한 값을 바꾸는지 지켜보는 방법으로 테스트합니다. 

## 마지막으로 

- 단방향 데이터 순환 : 문제가 생겼을 때 추적이 쉽습니다.
- 상태의 충돌 X : MVI에서는 상태를 한번에 하나씩만, 불변상태로만 가질 수 있기 때문입니다. 

Thread 안정성이나, 공유 가능성 등 불변 객체가 갖는 이점을 갖고 있습니다. 
- 철저하게 분리된 로직 : 단위 테스트가 쉽고, 기능 수정이나 추가가 쉽습니다. 

But 단점이..

- 높은 진입 장벽
- 많은 파일 (state, event, sideeffect…) : 객체 생성의 비용도 늘어납니다.
- 간단한 작업에는 부적절

그럼에도 불구하고, 

멀티 Renderer와 Scope를 사용한 부분 랜더링 지원

LinkedVM 기능을 사용한 공통 VM 재사용 지원

BoxView에 내장된 간단한 InAppEvent 기능 지원

ViewInitializer에서 State에 접근이 가능한 PendingState지원 등 

이런 여러 장점이 있어서 MVI도 사용합니다. 

(출처)[https://medium.com/myrealtrip-product/android-mvi-79809c5c14f0]

## 남는 의문

+ 그래서 이 예제에 그리고 큰 어플에 어떻게 MVI가 낄 수 있을까?
+ Box가 데이터바인딩을 강제한다고 어디에 나와있을까 : 납득은 됨
