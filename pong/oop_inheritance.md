# 8.4 객체지향 개발이론 (사용자 정의 객체 만들기, 상속)

우리는 구체적인 객체지향 설계에 앞서 객체 자체에 대해서 더 심도 있게 알아야 할 필요가 있는데 지금까지는 남들이 만들어 놓은 객체(오브젝트)를 사용하는 코딩만 해봤지만, 이제는 직접 나만의 객체(사용자 정의 객체: User Defiined Object)를 만들어서 활용하는 법을 통해 객체를 더 심도 있게 이해해 보자.

우리에게 익숙한 실물세계의 객체 하나를 예로 가져와 이를 프로그래밍 세계에 맞게 객체화 시켜보자. 실물의 자동차를 프로그래밍 세계의 자동차 객체로 변환해보자. 객체화 할 때는 먼저, 한 객체 안에 존재하는 **속성(attribute)**/**행동(behavior)**&#xC774;란 두 가지의 관점으로 객체를 바라봐야 한다. 속성이란  자동차 그 자체가 가진 속성으로 예를 들어 일반의 모든 자동차가 가질 수 있는 공통적인 특징으로 차체의 모양(body), 엔진의 종류(engine), 바퀴의 종류(wheels) 등이 **속성이 될 수 있고, 이는 객체 안에서 변수값으로 표현된다.** _사실 자동차의 특징은 그 밖에도 수많은게 있으나, 모든 것을 다 표현할 필요는 없고, 우리 프로그램의 목적성취에 필요한 것들만 취하면 된다._

이번엔 자동차가 수행할 수 있는 행동들에 대해 생각해보면,  자동차가 할 수 있는 행동으로는 달리거나(drive), 멈추거나(brake), 회전하거나(turn) 등의 **행동을 할 수 있으며 이는 객체 안에서 메소드(객체 안에 존재하는 함수를 지칭하는 이름)로 표현된다.** _마찬가지로_ _자동차가 할 수 있는 행동으로는 그 밖에도 다양하게 더 있을 수 있으나, 모든 것을 다 표현할 필요는 없고, 우리 프로그램의 목적성취에 필요한 것들만 취하면 된다._

달리거나 멈추거나 회전하거나의 행동의 결과에 따른 현재 자동차의 상태(state)를 표현하려면 추가로 (현재)속도(speed), (현재)방향(direction)에 대한 속성값들도 추가로 필요하겠다. _이처럼 처음 자동차의 특징들을 통해 속성들을 추출했을 때는 속성으로 파악되지 않았던 것들이 행동들을 추출하는 시점에서 특정 행동의 결과로 인한 현 자동차의 상태를 파악하기 위한 목적으로 추가적인 속성들이 인식되어 추가되는 것도 자연스러운 현상이다._ 그럼, 이제 이를 코드로 표현해보자.

<figure><img src="../.gitbook/assets/image (2).png" alt="" width="563"><figcaption><p><a href="https://www.youtube.com/watch?v=X3cFiJnxUBY">https://www.youtube.com/watch?v=X3cFiJnxUBY</a></p></figcaption></figure>

### **객체 정의(define) 문법**

> **class** 클래스 이&#xB984;_<mark style="color:green;">(상속받을 클래스 이름들)</mark>_:
>
> &#x20;   _<mark style="color:green;">super().\_\_init\_\_(파라미터들)</mark>_
>
> &#x20;   **def** \_\_init\_\_(self, 파라미터들):
>
> &#x20;       속성들
>
>
>
> &#x20;   메소드들

우리는 함수를 만들 때, **def** 라는 키워드를 사용해 정의(define) 것처럼, 객체를 만들 때는 **class** 키워드를 사용해 정의해야 한다. 문법 중에 녹색으로 이탤릭체로 표현된 부분의 객체의 상속(Inheritance)에 관한 문법으로 이에 대해서는 추후 다루기로 하고 당장은 무시해도 된다.

{% code lineNumbers="true" %}
```python
class Car:
    def __init__(self, body='general', engine='general', wheels='general'):
        self._speed = 0
        self._direction = 'forward'
        self._body = body
        self._engine = engine
        self._wheels = wheels
    
    def drive(self):
        self._speed += 2
        print("Current speed is " + str(self._speed))
    
    def brake(self):
        self._speed -= 1
        if self._speed < 0:
            self._speed = 0
        print("Current speed is " + str(self._speed))
        
    def turn(self, direction):
        self._direction = direction        
        print("Changed direction to " + str(self._direction))
```
{% endcode %}

객체정의 문법에 맞춰 Car 라는 객체를 정의했는데, 위에서 언급된 속성 5개는 3-7라인에 걸쳐 변수형태로 존재하는 것을 파악했을 것이고, 위에서 언급된 3가지 행동에 대해선 함수형태로 존재하는 것을 파악했을 것이다. 그런데, **\_\_init\_\_** 이라는 메소드는 기존에 못보던 형태의 낯선 함수라 무엇인지 궁금할텐데 **이는 파이썬의 일종의 내장함수로 객체의 속성값들을 초기화 하기 위한 특별한 목적으로 클래스 안에서 사용되는 것**_으로 나중에 해당 객체를 사용하는 시점에서 해당 객체생성시 파이썬 안에서 자동호출되는 콜백 메소&#xB4DC;_&#xC774;다.&#x20;

그럼, 또 **self** 는 무엇인가? self의 역할은 나중에 객체가 생성된 시점에 실제 메모리 안에 **생성되어 존재하는 객체(인스턴스(Instance)) 그 자신**를 가르키게 된다. 아직은 이게 무슨 말인지 한번에 와닿지 않을 수 있는데, _복잡하게 여기지 말고 단순히 구분자의 역할로 생각해, self가 있으면 객체 자신 안의 속성값과 메소드들을 나타내고, self가 없으면 일반변수와 일반함수로 구분 용도로 여기는 것도 가능하다._

속성 부분을 좀 더 살펴보자. 속도(speed)와 방향(direction) 속성을 나타내는 변수에 **\_(언더바(underbar) 또는 언더스코어(underscore)라 읽음)**&#xB97C; 추가했음을 알 수 있다. 이미 과거에 여러 차례 언급되었지만, 코딩에서 점하나 공백하나 조차도 다 의미부여가 있기 때문에, 이것도 어떤 의도가 있으리라 유추할 수 있다. 당연히 이는 의도성이 있는 것이며, 파이썬 언어에서 객체지향 구현시 관습적인 약속으로 이 속성은 특별한 객체속성들로 **외부(여기서 외부는 객체의 이용자를 지칭)에서 저 속성값의 존재를 구지 알 필요가 없는 객체 내부용도로만 사용하는 속성이며 객체 이용자가 이 값을 직접 바꾸려는 시도를 거부한다라는 의미를 표현하기 위해 언더스코어를 붙인 것**이다. (객체지향 이론에서 이를 **private** **속성**이란 용어로 지칭)

\_\_init\_\_ 메소드 안에 이 속성값들을 초기화하는 부분을 좀 더 살펴보면, \_\_init\_\_ 메소드의 파마리터(body, engine, wheels) 로 얻은 값을 가지고서, 속성값들을 초기화 하고 있다. 이 말은 이 Car객체를 이용하려고 누군가 생성하는 시점에 인자값으로써 이 값들을 넘겨주고, 그 값을 내부에서 다시 이용하기 위해 **self.**&#xB97C; 붙혀 객체 속성값으로 설정해 놓는다는 것이다.

그외 나머지 메소드들은 코드해석에 크게 어려움은 없을 것으로 보인다. _drive를 계속 호출하면, 속도를 +2씩 증가하는 것이고, 반대로 brake를 계속 호출하면 속도를 -1씩 감소시키는 것이고, turn을 호출하면 방향을 전환하는 것이다._

### 객체의 이용

객체를 만들었으니, 자 이제는 이를 이용해 보자. 자동차를 가속시켰다가 속도를 줄여 좌회전 한 후 다시 가속시키는 코드이다. 객체를 만드는게 처음이지 이용은 이미 많이 해봤기 때문에 큰 어려움은 없을 것이다.

{% code lineNumbers="true" %}
```python
car = Car()

for _ in range(3):
    car.drive()
    
for _ in range(2):
    car.brake()  
     
car.turn('left')

for _ in range(2):
    car.drive() 
```
{% endcode %}

```
실행결과:
Current speed is 2
Current speed is 4
Current speed is 6
Current speed is 5
Current speed is 4
Changed direction to left
Current speed is 6
Current speed is 8
```

1번 라인에서 최초 객체를 생성할 때 아무 인자값도 넘기지 않고 생성했다. 그런데도, 실행시켜보면 아무 에러도 발생하지 않고, 실제 객체 안에 \_\_init\_\_ 메소드에 의한 속성값들의 초기화도 문제없이 진행되었다 어찌된 일인가? 그 비밀은 바로 \_\_init\_\_ 메소드를 정의할 때  파라미터들의 기본값(default값) 사전 설정해 놓는 문법이 있고, 이를 사용했기 때문이다.&#x20;

```python
def __init__(self, body='general', engine='general', wheels='general'):
```

바로 이 부분인데 3개의 파마리터값들(body, engine, wheels) 각각에 기본값으로 'general' 이라는 값을 설정해 놓았기에 사용하는 측에서 특별히 인자값을 넘기지 않을 경우, 이 값들이 자동으로 사용된다.

### 객체의 상속

객체의 이해를 좀 더 심화시켜보자. 이제 본격적인 객체지향이 가진 놀라운 능력들이 등장하기 시작한다. 지금까지 우리는 간단한 일반적 자동차 객체를 만들어봤다. 이제는 이 기본적인 자동차를 베이스(base) 삼아 이를 파생시킨 다음과 같이 더 다양한 종류의 자동차를 만들 수 있으면 좋지 않을까라는 생각이 들기 시작한다.&#x20;

<figure><img src="../.gitbook/assets/image (3).png" alt="" width="563"><figcaption><p><a href="https://www.youtube.com/watch?v=X3cFiJnxUBY">https://www.youtube.com/watch?v=X3cFiJnxUBY</a></p></figcaption></figure>

이게 가능하다고? 그렇다 가능하다. 객체지향은 탄생배경 자체가 기존 절차지향형의 한계를 극복한 소프트웨어 개발의 효율성과 확장성 증대에 있었으니 말이다. 이를 적용하기 위해서는 **객체간의 상속(Inheritance)에 대한 이해가 필요하다. 크게 어려운 개념은 아니고, 부모-자식간에 DNA의 유전처럼 자녀는 부모의 DNA를 물려받으면서 동시에 자신만의 독특성을 갖게 되는 것과 유사하게, 부모객체(Parent 또는 Super객체라 호칭) 를 상속한 자녀객체(Children 또는 Sub객체라 호칭)는 부모의 모든 것(속성/행동)을 다 물려받아 자기 것처럼 쓸 수 있고, 동시에 자신만의 속성/행동도 갖게 되는 것이다.**&#x20;

기본 베이스 자동차에서 파생할 수 있는 여러 자동차들 중에서 우리는 가속력이 뛰어난 노란색 스포츠카와 뚜껑이 열리는 기능이 추가된 빨간색 컨버터블 자동차를 만든다고 가정해 보자. 이러한 베이스를 기반한 상속관계를 코딩으로 표현하면 어떻게 될까?

<figure><img src="../.gitbook/assets/image (5).png" alt="" width="563"><figcaption><p><a href="https://www.youtube.com/watch?v=X3cFiJnxUBY">https://www.youtube.com/watch?v=X3cFiJnxUBY</a></p></figcaption></figure>

{% code lineNumbers="true" %}
```python
class SportsCar(Car):
    def __init__(self, color):
        super().__init__('sports', 'strong', 'big')
        self._color = color

    def drive(self):
        self._speed += 4
        print("Current speed is " + str(self._speed))
    
    def brake(self):
        self._speed -= 2
        if self._speed < 0:
            self._speed = 0
        print("Current speed is " + str(self._speed))
        
        
class ConvertableCar(Car):
    def __init__(self, color):
        super().__init__('convertable')
        self._color = color
        self._opened = False

    def move_roof(self):
        self._opened = not self._opened
        if self._opened:
            print("Current roof is opned")
        else:
            print("Current roof is closed")
```
{% endcode %}

이를 코딩하기 위해선 위에 문법표현에서 잠시 미뤄두자고 했던 객체 생성 문법에서 상속관련 문법(녹색의 이텔릭체 부분) 이 더해진 것을 기억하자. 1번, 17번 라인에서 파생객체(상속받은 객체를 지칭하는 용어)를 만들 때, 부모(여기서는 Car)는 누구였는지 괄호 안에 표기해 만들게 된다. 그리고, 이후에 객체 속성값 초기화 메소드인 \_\_init\_\_ 안 에서 제일 먼저는 **super().\_\_init\_\_() 라는 메소드를 사용해서 부모의 것(body, engine, wheels)은 부모에게 넘기는 방식으로 부모객체의 속성값을 초기화**하고 있다. _순서적으로 부모쪽이 먼저 준비(초기화)되고, 이후에 그걸 다 물려받아 사용할(상속받은) 내쪽이 그다음에 초기화되는게 논리적으로도 맞는 것이다._ 여기서, **super() 의 용도를 짐작했겠지만, 이전의 self와 유사한 용도로 객체 안에서 사용하는 파이썬 내장함수 같은 것으로 자신의 부모클래스(Parent)의 객체를 리턴해주는 특별 메소드**인 것이다.&#x20;

스포츠카는 기본적으로 베이스 자동차인 부모의 모든 것을 다 물려받을 것이고, 그밖에 자기만의 특성이라고 할만한 속성들은 무엇이 있을까? 우리가 만들려고 하는 것이 노란색 스포츠카라고 했던 점을 기억한다면 이 스포츠카는 차체색깔(color)이란 별도 속성을 가져야 할 것을 예상할 수 있다(4번 라인 참조). 또 기존에 부모와의 차이점은 무엇이 있는가? _부모와 똑같은 행동으로 달리고(drive), 회전하고(turn), 감속할 수 있는데(brake) 차이가 있다면, 차량 경주와 같은 다이나믹한 운전경험을 보장하기 위해 보다 떠빨리 달리고(drive), 더 빨리 감속할 수 있는 것(brake)에서 차이를 갖고 있다._ 따라서, **이미 부모에게 있는 행동들이라 그대로 물려받을 수 있음에도 불구하고, 이미 부모에 존재하는 메소드들인 drive, brake를 자식은 스포츠카 클래스 안에서 다시 재기능구현** 한 부분이다. 이 의미는 자식 입장에서 부모의 것이 그대로 쓰기에는 자기에 맞지 않아 부모 것이 아닌 따로 내꺼를 쓰겠다 라고 의도하는 것이다. 객체지향에선 이를 **오버라이딩**(0verriding) 이란 용어로 지칭하는데 이 단어뜻 그대로 기존 것을 덮어씌었다는 의미이다.&#x20;

drive와 brake 메소드 안에 구현부분을 자세히 보면 좋겠다. 7번과 11번 라인에서 self.speed라는 속성이 어디서 왔을까? \_\_init\_\_ 메소드 안에서 해당 속성값을 한번도 생성한 적이 없지만, 마지 원래 자기 것처럼 사용하고 있는데, 그렇다 _그 속성값은 본래 부모의 것이나, 부모의 모든 것을 상속받은 자녀입장에서 자신에게 없음에도 마치 자신에게 이미 존재했던 것처럼 사용하고 있는 것이다._

스포츠카의 부모-자녀 상속관계를 잘 이해했다면, 컨버터블카의 경우는 너무 싶다. 마찬가지로 부모의 모든 것을 다 물려받았고, 자기만의 특성이라고 할만한 것은 무엇이 있을까? 우리가 빨깐색 컨버터블이라고 했기때문에 마찬가지로 차체색깔(color)을 가져야 할 것이고, 컨버터블 차라고 불리는 이유가 되는 고유한 특성은 바로 차 지붕을 열고/닫을 수 있는(move\_roof) 행동을 할 수 있다는 것이다. 이를 상세구현하면 다음과 같다. 먼저, 17번 라인에서 ConvertibleCar(Car) 문법을 통해 Car의 상속을 받고, 위에서 정리한 속성과 행동면에서 부모와는 다른 자신만의 특성들에 대해서만 추가 코딩을 진행하면 되는 것이다.

컨버터블카가 과연 부모로부터 모든 것을 다 물려받은 것이 사실인지 알 수 있는 부분은 어디일까? 다음의 파생클래스의 기능점검 테스트 코드 중에 15, 17, 18, 20 라인에서 drive, brake, turn 메소드는 컨버터블 클래스 안에서 정의한 적 없음에도 마치 자신에게 이미 존재하는 것처럼 사용되고 있다.&#x20;

```python
# 스포츠카 테스트
s_car = SportsCar('yellow')
for _ in range(3):
    s_car.drive()
for _ in range(2):
    s_car.brake()  
s_car.turn('left')
for _ in range(2):
    s_car.drive() 
    
# 커버터블카 테스트
c_car = ConvertableCar('red')
c_car.move_roof()
for _ in range(2):
    c_car.drive()
for _ in range(2):
    c_car.brake()  
s_car.turn('right')
for _ in range(2):
    c_car.brake() 
c_car.move_roof()
```

```
실행결과:
Current speed is 4
Current speed is 8
Current speed is 12
Current speed is 10
Current speed is 8
Changed direction to left
Current speed is 12
Current speed is 16
Current roof is opned
Current speed is 2
Current speed is 4
Current speed is 3
Current speed is 2
Changed direction to right
Current speed is 1
Current speed is 0
Current roof is closed
```

여기까지해서 우리는 객체를 어떻게 만들고, 객체의 활용성을 높이는 객체의 특성 중 가장 기본이 되는 특성인 상속 정도를 이해했다. 그럼에도 불구하고 만약 당신에게 객체지향 개발이 처음이었다면 이해가 쉽지 않았을 수 있다. 그러나 걱정할 필요가 없다. 누구에게나 익숙해지는데는 시간이 필요하기 때문이다. 사실은 객체지향의 세계는 상당히 방대하고 넓다. 그런데, 일부러 여러분들이 처음부터 방대한 지식의 양으로 지치버리면 곤란하기 때문에 당장 필요한 수준에서 이해시키는 것으로 끝내려 한다. 이후에 또다른 개념적 이해가 필요한 부분이 나오면 그 부분에서 계속 다음 지식을 이어 설명하는 방식으로 진행할 예정이다. 지금까지 우리가 해오던 방법대로 말이다. 자, 그럼 다음 절부터는 본격적으로 기존의 절차지향형으로 개발한 퐁 게임을 예제 삼아 이를 객체지향으로 구현해 보는 것을 시작해 보자.
