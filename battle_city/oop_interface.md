# 9.4 객체지향 개발이론 (인터페이스)

우리는 지난번 객체지향 개발이론에서 상속이라는 개념을 익혔다. 이번 절에서는 개념적으로는 상속의 한 형태이나 목적이 조금 다른 인터페이스(Interface)라는 개념을 배울 필요가 있다. 상속이 매우 유용한 개념임에도 불구하고 모든 문제를 상속으로만 풀 수 없는 한계를 가지고 있는데 다음과 같은 예의 상황이다.

<figure><img src="../.gitbook/assets/Pasted image 20241219092928.png" alt=""><figcaption></figcaption></figure>

그림에서 보는 것과 같이 우리가 살아가는 생태계에는 펭귄처럼 조류라고 해서 모두 날 수 있는 건 아니고, 날 수 있지만 조류에 속하지 않는 박쥐가 있고, 포유류이지만 걸어다니지 않고 어류처럼 헤엄쳐 다니는 고래처럼 여러 분류에 걸쳐지는 하이브리드 생명체들이 존재한다. 이처럼 일부 객체에게서만 필요한 특정 기능(수영할 수 있는(Swimmable), 날 수 있는(Flyable) 기능)이 존재할 수 있는데, 이를 위해 상위 부모객체(Bird, Mammal, Fish)에서 구현을 해놓으면 그 기능이 실제가 필요가 없는 모든 자식들에게도 존재하게 되고 결국 모든 자식 객체의 크기도 일괄 커지게 되어 불필요 메모리만 차지하게 되는 단점이 존재한다. 따라서, 이러한 기능들에 대해서는 독립적인 추상(abstract) 객체를 만들고, 해당 기능이 필요한 각각의 객체들이 이 추상객체를 상속 후 이를 직접 구현을 강제(?)하도록 하게 하는 방법을 사용하는데, 이러한 추상객체(abstract object)를 인터페이스라고 부른다.&#x20;

여기서, _추상적이라는 의미는 구체적(concrete)의 반대적 의미로, 실제 구체화된 구현(implementation) 내용은 없다는 의&#xBBF8;_&#xB85C; 따라서, **인터페이스는 메서드의 시그니처(이름, 매개변수, 반환 타입)만을 정의(define)하고 구현은 포함하지 않는 추상적인 타입**이다. 객체는 하나 이상의 인터페이스를 상속할 수 있으며, 상속한 클래스는 인터페이스에 정의된 모든 메서드를 반드시 구현해야 한다(구현 안하고 실행시키면 에러가 발생)는 의미가 구현을 강제한다는 의미이다.

위에 그림의 일부를 인터페이스를 포함해 코드로 표현해 보면 다음과 같다.&#x20;

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    @abstractmethod
    def has_organ(self):
        pass

class Bird(Animal, ABC):
    def has_organ(self):
        print("I have wings")
    
    @abstractmethod
    def has_feather(self):
        pass
        
class Flyable(ABC):
    @abstractmethod
    def fly(self):
        pass

class Eagle(Bird, Flyable):
    def has_feather(self):
        print("I have 718 feathers.")

    def fly(self):
        print("I can fly.")
        
class Penguin(Bird):
    def has_feather(self):
        print("I have short and waterproof feathers.")
```

참고로 설명을 위한 예졔 코드라 단순화 시켜 보여주기 위해 \_\_init\_\_ 함수들은 제외했음을 인지하기 바란다.

<pre><code># 사용예시
eagle = Eagle()
eagle.has_organ() 
eagle.fly()  
eagle.has_feather()

penguin = Penguin()
penguin.has_feather()
<strong>
</strong><strong>실행결과:
</strong>I have wings
I can fly.
I have 718 feathers.
I have short and waterproof feathers.
</code></pre>

### **추상객체 정의(define) 문법**

> from <mark style="color:green;">abc</mark> import ABC, abstractmethod
>
>
>
> **class** 추상클래스 이름(<mark style="color:green;">ABC</mark>):
>
> &#x20;   _**def** \_\_init\_\_(self, 파라미터들):_
>
> &#x20;       _속성들_
>
>
>
> &#x20;   <mark style="color:green;">**@abstractmethod**</mark>
>
> &#x20;   **def** 추상메소드 이름(self):   &#x20;
>
> &#x20;       pass

추상객체들을 만드는 문법은 처음 등장했으므로 유심히 살펴보도록 하자. 먼저는 **abc**라는 내장 모듈을 import 하고 **ABC**([Abstract Base Class](https://docs.python.org/3.13/library/abc.html)) 라는 추상클래스를 만들기 위한 베이스 클래스를 상속받고, 추상클래스 안에 존재하게 될 추상메소드 위에는 **@abstractmethod** 라는 데코레이터(decorator)를 붙혀서 만들고, _추상이기 때문에 당연히 그 함수의 상세 구현내용 없이 pass 라는 아무 것도 실행할 것이 없다는 구문을 하나 넣어놓고 끝낸다._ _데코레이터 문법도 새롭게 등장했는데 기존 함수를 수정하지 않고 그 기능을 확장하는 방법을 제공하는 문법으로_ 여기서는 간단히 이 함수는 일반 메소드가 아니라 추상 메소드로 인지하라는 의미로 붙혔다고 간단히 생각하자.

Flyable 인터페이스를 상속받은 Eagle 클래스를 살펴보자. Flyable 인터페이스를 상속 받았기 때문에 Flyable 인터페이스의 추상메소드였던 fly 함수를 반드시 구현하게(이전 시간에 배운 함수 오버라이딩 구현방식) 되어있다. 그럼, 상속만 받고 구현을 안하면 어떻게 될까? 직접 해보면 아는데 아래와 같이, 추상메소드인 fly 함수를 구현하지 않았다 라고 에러를 내고 종료하게 된다. 그도 그걸 것이 사용하는 측 입장에서 실제 구현이 없는 것을 어떤 기능을 하도록 기대하며 사용할 수 는 없지 않은가?

```
File "test.py", line 64, in <module>
    eagle = Eagle()
TypeError: Can't instantiate abstract class Eagle with abstract methods fly
```

다시 정리하면, 여기서의 _**인터페이스(추상객체) 상속의 개념은 우리가 부모의 모든 기능을 물려받는 목적에서의 상속개념이라기 보다, 이를 상속한 객체의 경우, 그 기능을 반드시 갖고 있다는 것을 보장하게 되는 약속 개념과 그 인터페이스의 구현을 강제화 함으로써  객체지향설계를 한 사람의 규칙을 따르게 하려는 목적에 있다.**_ 결국 그 규칙을 따르게 되면 어떠한 이점을 갖게 되는가가 궁금할 수 있는데, 그 부분은 객체지향의 또 다른 커다란 주제인 **다형성(Polymorphism)**&#xC774;란 개념과 밀접한 관련이 있고, 워낙 내용적으로 설명할 내용이 많아 이번에 다루지 않고, 추후에 이 개념을 알고 사용해야 하는 시점에서 다시한번 설명할 기회를 갖도록 하겠다. 자, 그럼 이 인터페이스 개념을 활용해 이번 과의 목적인 배틀 시티게임의 객체지향형으로의 개발을 계속 이어가도록 하자.



