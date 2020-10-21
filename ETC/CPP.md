# CPP 전체적인 개념 정리

---

# OOP(Object-Oriented Programing)
컴퓨터 프로그래밍의 패러다임  
프로그램을 명령어의 목록이 아닌, 독립된 "객체"들의 모임으로 바라봄

객체란, 변수들과 참고 자료들로 이루어진 소프트웨어 덩어리  

특징  
- **추상화 (abstraction)**
    - 대상의 특성 중 불필요한 부분을 무시하고 필요한 공통점만을 다루어, 현실의 복잡성을 극복하고 목적에 집중할 수 있도록 하는 것

    - 공통의 속성이나 기능을 묶어 이름을 붙이는 것

    - Class를 정의하는 것

- **캡슐화 (Encapsulation)**  
    - 객체 스스로가 자신의 상태를 책임지게 하여, 해당 객체의 역할 수행에 집중할 수 있도록 자율성을 높이는 것

    - 데이터 구조와 데이터를 다루는 방법들을 결합시켜 묶는 것

    - 변수와 함수를 하나로 묶는 것

    - 데이터를 외부에서 접근하는 것을 방지하고, 오로지 함수를 통해서만 접근할 수 있게 하는 것

    - 캡슐화의 2가지 관점

        1) 상태-행위 캡슐화 (데이터 캡슐화) : 객체는 상태와 행동을 하나의 단위로 묶는 자율적 실체

        2) 사적인 비밀의 캡슐화 (은닉화) : 외부에서 객체의 상태를 변경할 수 없도록 은닉

    - 접근 변경자

        public > protected > default > private

        모든 접근 허용 > default + 상속 관계인 다른 패키지의 클래스 > 같은 패키지 > 같은 클래스  

- **상속성 (Ingeritance)**  

    - 부모 클래스의 속성과 기능을 상속받아 동일하게 사용하는 것

    - 범용적인 클래스를 작성한 후 상속을 활용하면, 여러 클래스에서 중복되는 속성과 기능을 재사용할 수 있음

- **다형성 (Polymorphism)**

    - 동일 요청에 대해 서로 다른 방식으로 응답할 수 있도록 만드는 것

    - 오버라이딩 (Overriding) : 상속받은 동일한 메소드 재정의

    - 오버로딩 (Overloading) : 동일한 메소드가 매개변수 타입, 개수 차이에 따라 다르게 동작 

C++ 에서 클래스를 이용해서 만들어진 객체를 **인스턴스(instance)** 라고 부름  

---

# 복사 생성자 (copy constructor)
```
T(const T& a);
```
다른 T 의 객체 a 를 상수 레퍼런스로 받음  
여기서 a 가 const 이기 때문에 우리는 복사 생성자 내부에서 a 의 데이터를 변경할 수 없고,  
오직 새롭게 초기화 되는 인스턴스 변수들에게 '복사' 만 할 수 있게 됨  

```
Photon_Cannon::Photon_Cannon(const Photon_Cannon& pc) {
  std::cout << "복사 생성자 호출 !" << std::endl;
  hp = pc.hp;
  shield = pc.shield;
  coord_x = pc.coord_x;
  coord_y = pc.coord_y;
  damage = pc.damage;
}
```
위와 같이 복사 생성자 내부에서 pc 의 인스턴스 변수들에 접근해서 객체의 shield, coord_x, coord_y 등을 초기화 할 수 는 있지만

`pc.coord_x = 3;` 처럼 pc 의 값 자체는 변경할 수 없다는 이야기

`Photon_Cannon pc2 = pc1;`  
위 코드 역시 복사 생성자가 호출  
C++ 컴파일러는 위 문장을 아래와 동일하게 해석  
`Photon_Cannon pc2(pc1);`  

C++ 컴파일러는 디폴트 복사 생성자를 지원, 이는 대응되는 원소들을 1 대 1 복사를 해줌

## 디폴트 복사 생성자의 한계
만약 초기화되는 멤버 변수 중 포인터가 있을때 디폴트 복사 생성자를 사용하면  
기존의 클래스에 있는 포인터와 새로 생성된 클래스에 있는 포인터가 같은 값을 가지게 됨  

이 때, 하나의 클래스가 소멸하며 포인터에 할당되어있던 메모리를 해제하면 남은 클래스에서는 해제된 메모리 주소를 가리키고 있음  
이런 상태에서 남은 클래스의 포인터를 해제시키려하면 이미 해제된 메모리에 접근하기때문에 런타임 오류가 발생  

이를 막기 위해서는 해당 포인터를 그대로 복사하지 말고 따로 다른 메모리에 동적 할당을 해서 그 내용만 복사하면 됨  

이렇게 메모리를 새로 할당해서 내용을 복사하는 것을 **깊은 복사(deep copy)**,  
단순히 대입만 해주는 것을 **얕은 복사(shallow copy)** 라고 함

컴파일러가 생성하는 디폴트 복사 생성자의 경우 얕은 복사밖에 할 수 없으므로 깊은 복사가 필요한 경우에는 사용자가 직접 복사 생성자를 만들어야함  

---

# explict
```
void DoSomethingWithString(MyString s) {
  // Do something...
}
```
위 코드는
```
DoSomethingWithString(MyString("abc"))
```
MyString 객체를 생성해서 이를 인자로 전달  

MyString을 명시적으로 생성하지 않는 경우
```
DoSomethingWithString("abc")
```

"abc"는 MyString 타입이 아니지만 컴파일러는 생성자 중에서 이를 MyString으로 변환할 수 있는지 찾음
```
// 문자열로 부터 생성
MyString(const char* str);
```

MyString에는 const char* 로부터 MyString을 만드는 생성자가 있으므로  
`DoSomethingWithString("abc")`는 알아서 `DoSomethingWithString(MyString("abc"))`로 변환되어 컴파일됨  

위와 같은 변환을 **암시적 변환(implicit conversion)** 이라고 부름  

암시적 변환이 항상 사용자에세 편리한 것은 아님  

```
DoSomethingWithString(3)
```
이는 아마도 함수를 사용자가 잘못 사용했을 가능성이 높음  
문자열을 받는 함수에 정수를 입력했기 때문  
하지만 컴파일러는 위 문장을 오류로 판단하지 않음  

```
// capacity 만큼 미리 할당함.
MyString(int capacity);
```
위와 같이 int 인자를 받는 MyString 생성자가 있다면 위 함수는

```
DoSomethingWithString(MyString(3))
```
으로 변환되어 컴파일됨  
즉, 사용자가 의도하지 않은 암시적 변환이 일어나게 됨  

이러한 의도하지 않는 암시적 변환을 막기위해 사용하는 키워드가 **explicit**  

```
// capacity 만큼 미리 할당함. (explicit 키워드에 주목)
explicit MyString(int capacity);
```

explicit 은 또한 해당 생성자가 복사 생성자의 형태로도 호출되는 것을 막게 됨

```
MyString s(5);   // 허용
MyString s = 5;  // 컴파일 오류!
```

위와 같이 explicit을 이용해 생성자를 선언해두면 명시적으로 생성자를 부를 때 에만 허용할 수 있게 됨

# mutable
mutable로 선언하면 const 함수에서도 이들 값을 바꿀 수 있음  

```
#include <iostream>

class A {
  mutable int data_;

 public:
  A(int data) : data_(data) {}
  void DoSomething(int x) const {
    data_ = x;  // 가능!
  }

  void PrintData() const { std::cout << "data: " << data_ << std::endl; }
};

int main() {
  A a(10);
  a.DoSomething(3);
  a.PrintData();
}
```

왜 mutable을 사용하나?  
멤버 함수를 const 로 선언하는 의미는 **이 함수는 객체의 내부 상태에 영향을 주지 않습니다** 를 표현하는 방법  
**읽기** 작업을 수행하는 함수들이 그 예  

대부분의 경우 의미상 상수 작업을 하는 경우, 실제로도 상수 작업을 하게 되나, 실제로 꼭 그렇지만은 않음  

예를 들어, 아래와 같은 서버 프로그램을 만든다고 가정했을 때
```
class Server {
  // .... (생략) ....

  // 이 함수는 데이터베이스에서 user_id 에 해당하는 유저 정보를 읽어서 반환한다.
  User GetUserInfo(const int user_id) const {
    // 1. 데이터베이스에 user_id 를 검색
    Data user_data = Database.find(user_id);

    // 2. 리턴된 정보로 User 객체 생성
    return User(user_data);
  }
};
```
이 서버에는 GetUserInfo 라는 함수가 있는데 입력 받은 user_id 로 데이터베이스에서 해댱 유저를 조회해서 그 유저의 정보를 리턴하는 함수  
당연히도 데이터베이스를 업데이트 하지도 않고, 무언가 수정하는 작업도 당연히 없기 때문에 const 함수로 선언되어 있음  

그런데 대개 데이터베이스에 요청한 후 받아오는 작업은 꽤나 오래 걸림  
그래서 보통 서버들의 경우 메모리에 캐쉬(cache)를 만들어서 자주 요청되는 데이터를 굳이 데이터베이스까지 가서 찾지 않아도 메모리에서 빠르게 조회할 수 있도록 함

물론 캐쉬는 데이터베이스만큼 크지 않기 때문에 일부 유저들 정보 밖에 포함하지 않음 따라서 캐쉬에 해당 유저가 없다면, 데이터베이스에 직접 요청해야됨  
대신 데이터베이스에서 유저 정보를 받으면 캐쉬에 저장해놓아서 다음에 요청할 때는 빠르게 받을 수 있게 됨  

이를 구현하면
```
class Server {
  // .... (생략) ....

  Cache cache; // 캐쉬!

  // 이 함수는 데이터베이스에서 user_id 에 해당하는 유저 정보를 읽어서 반환한다.
  User GetUserInfo(const int user_id) const {
    // 1. 캐쉬에서 user_id 를 검색
    Data user_data = cache.find(user_id);

    // 2. 하지만 캐쉬에 데이터가 없다면 데이터베이스에 요청
    if (!user_data) {
      user_data = Database.find(user_id);

      // 그 후 캐쉬에 user_data 등록
      cache.update(user_id, user_data); // <-- 불가능
    }

    // 3. 리턴된 정보로 User 객체 생성
    return User(user_data);
  }
};
```

문제는 GetUserInfo 가 const 함수라는 점, 따라서 
```
cache.update(user_id, user_data);  // <-- 불가능
```

위 처럼 캐쉬를 업데이트 하는 작업을 수행할 수 없음  
왜냐하면 캐쉬를 업데이트 한다는 것은 케쉬 내부의 정보를 바꿔야 된다는 뜻이기 때문  

따라서 저 update 함수는 const 함수가 아니어야 함  
그렇다고 해서 GetUserInfo 에서 const 를 뗄 수 없는 것이,  
이 함수를 사용하는 사용자의 입장에선 당연히 const 로 정의되어야 하는 함수임  

따라서 이 경우, Cache 를 mutable 로 선언해버리면 됨

---

# 상속
## ◇ 업 캐스팅, 다운 캐스팅
Parent 클래스를 상속받는 Child 클래스가 있을 때  
```
Child c;
Parent* p = &c;
```
위와 같은 선언이 가능  

Child 는 Parent 를 상속받았기 때문에 Parent가 가지고 있는 멤버 변수, 멤버 함수를 모두 포함하고 있음  
따라서 별 이상 없이 실행 가능  

이렇게 자식 클래스에서 부모 클래스로 캐스팅 하는 것을 **업 캐스팅** 이라고 하고,  
이와 반대로 부모 클래스에서 자식 클래스로 캐스팅 하는 것을 **다운 캐스팅** 이라고 함  

다운 캐스팅에서 Parent 는 Child 가 가지고 있는 멤버 변수, 멤버 함수를 모르기 때문에  
컴파일러상 함부로 다운 캐스팅 하는 것을 금지하고 있음(컴파일 에러가 발생)  

```
Parent p;
Child* c = static_cast<child*>(p);
```

위와 같이 강제로 다운 캐스팅이 가능하나, 다운 캐스팅 이후 Child 에서 선언, 혹은 오버라이드 한 함수나 변수에 접근한다면  
런타임 에러가 발생함  

따라서 강제적으로 다운 캐스팅을 하는 경우, 컴파일 타임에서 오류를 찾아내기 매우 힘들기 때문에  
작동이 보장되지 않는 다운 캐스팅은 권장하지 않음

## ◇ vitural
```
class Parent {
 public:
  virtual void what() { cout << "Parent what()\n"; }
};

class Child : public Parent {
 public:
  void what() { cout << "Child what()\n"; }
};

int main(){
  Parent p;
  Child c;

  Parent* p_p = &p;
  Parent* p_c = &c;

  p_p->what();
  p_c->what();
}
```
virtual 키워드를 사용하면 위와 같은 상황에서 적절한 함수를 호출해줌  
virtual 키워드는 컴파일 시에 어떤 함수가 실행될 지 정해지지 않고 런타임 시에 정해줌  
이러한 일을 **동적 바인딩(dynamic binding)** 이라고 부름  

이러한 virtual 키워드가 붙은 함수를 **가상 함수(virtual function)** 라고 부르고,  
파생 클래스의 함수가 기반 클래스의 함수를 오버라이드 하기 위해서는 두 함수의 꼴이 정확히 같아야함  

C++ 11 에서는 파생 클래스에서 기반 클래스의 가상 함수를 오버라이드 하는 경우,  
override 키워드를 통해서 명시적으로 나타낼 수 있음  
override 키워드를 사용하게 되면, 실수로 오버라이드를 하지 않는 경우를 막을 수 있음

## ◇ virtual 소멸자
```
#include <iostream>
using namespace std;

class Parent {
 public:
  Parent() { cout << "Parent 생성자 호출\n"; }
  ~Parent() { cout << "Parent 소멸자 호출\n"; }
};
class Child : public Parent {
 public:
  Child() : Parent() { cout << "Child 생성자 호출\n"; }
  ~Child() { cout << "Child 소멸자 호출\n"; }
};

int main() {
  cout << "--- 평범한 Child 만들었을 때 ---\n";
  { Child c; }
  cout << "--- Parent 포인터로 Child 가리켰을 때 ---\n";
  {
    Parent *p = new Child();
    delete p;
  }
}
```
위 코드를 실행시키면

```
--- 평범한 Child 만들었을 때 ---
Parent 생성자 호출
Child 생성자 호출
Child 소멸자 호출
Parent 소멸자 호출
--- Parent 포인터로 Child 가리켰을 때 ---
Parent 생성자 호출
Child 생성자 호출
Parent 소멸자 호출
```
와 같은 결과가 나옴  

위와 같은 상황에서 문제는 Parent 포인터가 Child 객체를 가리킬 때에서  
delete p 를 하더라도, p 가 가리키는 것은 Parent 객체가 아닌 Child 객체 이기 때문에,  
위에서 보통의 Child 객체가 소멸되는 것과 같은 순서로 생성자와 소멸자들이 호출되어야만 함  
하지만 실제로는, Child 소멸자가 호출되지 않음  

소멸자가 호출되지 않는다면 여러가지 문제가 생길 수 있음  
예를 들어서, Child 객체에서 메모리를 동적으로 할당하고 소멸자에서 해제하는데,  
소멸자가 호출 안됬다면 메모리 누수(memory leak)가 생기게 됨  

이 상황의 해결책은 Parent 의 소멸자를 virtual 로 만들어버리면 됨  
Parent 의 소멸자를 virtual 로 만들면, p 가 소멸자를 호출할 때, Child 의 소멸자를 성공적으로 호출할 수 있게 됨

```
#include <iostream>
using namespace std;

class Parent {
 public:
  Parent() { cout << "Parent 생성자 호출\n"; }
  virtual ~Parent() { cout << "Parent 소멸자 호출\n"; }
};
class Child : public Parent {
 public:
  Child() : Parent() { cout << "Child 생성자 호출\n"; }
  ~Child() { cout << "Child 소멸자 호출\n"; }
};

int main() {
  cout << "--- 평범한 Child 만들었을 때 ---\n";
  { Child c; }
  cout << "--- Parent 포인터로 Child 가리켰을 때 ---\n";
  {
    Parent *p = new Child();
    delete p;
  }
}
```

```
--- 평범한 Child 만들었을 때 ---
Parent 생성자 호출
Child 생성자 호출
Child 소멸자 호출
Parent 소멸자 호출
--- Parent 포인터로 Child 가리켰을 때 ---
Parent 생성자 호출
Child 생성자 호출
Child 소멸자 호출
Parent 소멸자 호출
```

왜 Parent 소멸자는 호출이 되었는가?  
이는 Child 소멸자를 호출하면서, Child 소멸자가 **알아서** Parent 의 소멸자도 호출해주기 때문  
(Child 는 자신이 Parent 를 상속받는다는 것을 알고 있음)

반면에 Parent 소멸자를 먼저 호출하게 되면, Parent 는 Child 가 있는지 없는지 모르므로,  
Child 소멸자를 호출해줄 수 없음  
(Parent 는 자신이 누구에서 상속해주는지 알 수 없음)

이와 같은 이유로, 상속될 여지가 있는 Parent 클래스들은 반드시 소멸자를 virtual 로 만들어주어야  
나중에 문제가 발생할 여지가 없게 됨

## ◇ 가상 함수의 구현 원리  
왜 C++ 에서는 virtual 키워드를 이용해 사용자가 직접 virtual 로 선언하도록 하였을까  
그 이유는 가상 함수를 사용하게 되면 약간의 **오버헤드 (overhead)** 가 존재하기 때문  

C++ 컴파일러는 가상 함수가 하나라도 존재하는 클래스에 대해서, **가상 함수 테이블(virtual function table; vtable)** 을 만들게 됨  
함수의 이름과 실제로 어떤 함수가 대응되는지 테이블로 저장하고 있는 것  

비 가상함수들은 그냥 단순히 특별한 단계를 걸치지 않고 함수를 호출하면 직접 실행됨  

하지만, 가상 함수를 호출하였을 때는 가상 함수 테이블을 한 단계 더 걸쳐서, 실제로 어떤 함수를 고를지 결정하게 됨  

따라서 보통의 함수를 호출하는 것 보다 가상 함수를 호출하는 데 걸리는 시간이 조금 더 오래 걸림  

## ◇ 순수 가상 함수(pure virtual function)와 추상 클래스(abstract class)
```
class Animal {
 public:
  Animal() {}
  virtual ~Animal() {}
  virtual void speak() = 0;
};
```
speak 함수는 함수의 몸통이 정의되어 있지 않고 단순히 = 0; 으로 처리되어 있는 가상 함수  
이 함수는 "무엇을 하는지 정의되어 있지 않는 함수"  
다시 말해 이 함수는 **반드시 오버라이딩 되어야만 하는 함수**  

이렇게, 가상 함수에 = 0; 을 붙여서, 반드시 오버라이딩 되도록 만든 함수를  
완전한 가상 함수라 해서, **순수 가상 함수(pure virtual function)** 라고 부름  

순수 가상 함수는 본체가 없기 때문에, 이 함수를 호출하는 것은 불가능  
그렇기 때문에, Animal 객체를 생성하는것 또한 불가능  

따라서 Animal 처럼, 순수 가상 함수를 최소 한 개 이상 포함하고 있는 클래스는 객체를 생성할 수 없으며,  
인스턴스화 시키기 위해서는 이 클래스를 상속 받는 클래스를 만들어서 모든 순수 가상 함수를 오버라이딩 해주어야만 함

이렇게 순수 가상 함수를 최소 한개 포함하고 있는, 반드시 상속 되어야 하는 클래스를 가리켜  
**추상 클래스 (abstract class)** 라고 부름

추상 클래스는 이 클래스를 상속받아서 사용하는 사람에게 "이 기능은 일반적인 상황에서 만들기 힘드니 너가 직접 특수화 되는 클래스에 맞추어서 만들어서 써라." 라고 말해주는 것  

추상 클래스의 또 한가지 특징은 비록 객체는 생성할 수 없지만, 추상 클래스를 가리키는 포인터는 문제 없이 만들 수 있다는 점  

---

# C++에서의 자원 관리
C++ 에서 자원을 관리하는 방법으로 다음과 같은 디자인 패턴을 제안  
바로 RAII, 즉 '자원의 획득은 초기화다' (*Resource Acquisition Is Initialization*)  
이는 자원 관리를 스택에 할당한 객체를 통해 수행하는 것  

예외가 발생해서 함수를 빠져나가더라도, 그 함수의 스택에 정의되어 있는 모든 객체들은 빠짐없이 소멸자가 호출됨 (stack unwinding)  
물론 예외가 발생하지 않는다면, 함수가 종료될 때 당연히 소멸자들이 호출됨  

생각을 조금 바꿔서 만약에 이 소멸자들 안에 다 사용한 자원을 해제하는 루틴을 넣음  

예를 들어서 포인터 pa 의 경우 객체가 아니기 때문에 소멸자가 호출되지 않음  
그 대신에, pa 를 일반적인 포인터가 아닌, 포인터 '객체' 로 만들어서 자신이 소멸 될 때 자신이 가리키고 있는 데이터도 같이 delete 하게 하면 됨  
즉, 자원 (이 경우 메모리) 관리를 스택의 객체 (포인터 객체) 를 통해 수행하게 되는 것  

이렇게 작동하는 포인터 객체를 **스마트 포인터(smart pointer)** 라고 부름  

C++ 11 에서는 auto_ptr 를 보완한 두 가지 형태의 새로운 스마트 포인터를 제공하고 있음  
바로 **unique_ptr** 과 **shared_ptr** 입니다.  

---

## ◇ unique_ptr
C++ 에서 메모리를 잘못된 방식으로 관리하였을 때 크게 두 가지 종류의 문제점이 발생할 수 있음  

첫 번째는 메모리를 사용한 후에 해제하지 않은 경우(메모리 누수(memory leak))  
RAII 를 통해서 사용이 끝난 메모리는 항상 해제시켜 버리면 메모리 누수 문제를 사전에 막을 수 있음  

두 번째로 발생 가능한 문제는, 이미 해제된 메모리를 다시 참조하는 경우
```
Data* data = new Data();
Date* data2 = data;

delete data;

// ...

delete data2;
```

이미 소멸된 객체를 다시 소멸시키려고 하면, 메모리 오류가 나면서 프로그램이 죽게 됨  
이렇게 이미 소멸된 객체를 다시 소멸시켜서 발생하는 버그를 double free 버그라고 부름  

위와 같은 문제가 발생한 이유는 만들어진 객체의 소유권이 명확하지 않아서임  
C++ 에서는 특정 객체에 유일한 소유권을 부여하는 포인터 객체를 unique_ptr 라고 함  

```
std::unique_ptr<A> pa(new A());
```
pa 가 포인터인것처럼 사용하시면 됨  
unique_ptr 은 -> 연산자를 오버로드해서 마치 포인터를 다루는 것과 같이 사용할 수 있게 함  

또한 이 unique_ptr 덕분에 RAII 패턴을 사용할 수 있음  
pa 는 스택에 정의된 객체이기 때문에, do_something() 함수가 종료될 때 자동으로 소멸자가 호출됨  
그리고 이 unique_ptr 는 소멸자 안에서 자신이 가리키고 있는 자원을 해제해 주기 때문에, 자원이 잘 해제될 수 있었음

### ▷ unique_ptr 소유권 이전

unique_ptr 는 복사 하려하면 삭제된 함수를 사용하려 했다고 컴파일 오류가 발생함  

삭제된 함수란 `A(const A& a) = delete;` 와 같이 `= delete;` 를 사용하게 되면,  
프로그래머가 명시적으로 '이 함수는 쓰지 마' 라고 표현할 수 있음  
혹시나 사용하더라도 컴파일 오류가 발생함  

unique_ptr 도 마찬가지로 unique_ptr 의 복사 생성자가 명시적으로 삭제되었음  
그 이유는 unique_ptr 는 어떠한 객체를 유일하게 소유해야 하기 때문  

만일 unique_ptr 를 복사 생성할 수 있게 된다면, 특정 객체를 여러 개의 unique_ptr 들이 소유하게 되는 문제가 발생함  
따라서, 각각의 unique_ptr 들이 소멸될 때 전부 객체를 delete 하려 해서 앞서 말한 double free 버그가 발생하게 됨

unique_ptr 은 복사 생성자는 정의되어 있지 않지만, 이동 생성자는 가능  
왜냐하면, 마치 소유권을 이동시킨다 라는 개념으로 생각하면 되기 때문  

```
std::unique_ptr<A> pb = std::move(pa);
```

소유권이 이전된 unique_ptr 를 **댕글링 포인터(dangling pointer)** 라고 하며 이를 재 참조할 시에 런타임 오류가 발생함  
따라서 소유권 이전은, 댕글링 포인터를 절대 다시 참조 하지 않겠다는 확신 하에 이동해야 함  

### ▷ unique_ptr 를 함수 인자로 전달

만약에 다른 함수에서 unique_ptr 가 소유한 객체에 일시적으로 접근하고 싶다면,  
get 을 통해 해당 객체의 포인터를 전달하면 됨  
(레퍼런스로 전달할수도 있지만, unique_ptr 은 소유권을 의미한다는 원칙에 위배)

C++ 14 부터 unique_ptr 을 간단히 만들 수 있는 std::make_unique 함수를 제공
```
auto ptr = std::make_unique<AnyType>(Any, Thing);
```

## ◇ shared_ptr
때에 따라서는 여러 개의 스마트 포인터가 하나의 객체를 같이 소유 해야 하는 경우가 발생  
shared_ptr 로 객체를 가리킬 경우, 다른 shared_ptr 역시 그 객체를 가리킬 수 있음  

shared_ptr 들은 참조 개수가 몇 개 인지 알고 있어야만 함  
이 경우 어떻게 하면 같은 객체를 가리키는 shared_ptr 끼리 동기화를 시킬 수 있을까?  

shared_ptr 내부에 참조 개수를 저장한다면 이미 생성된 shared_ptr의 참조 포인트 개수는 건드릴 수 없음  

따라서 이와 같은 문제를 방지하기위해 처음으로 실제 객체를 가리키는 shared_ptr 가 제어 블록(control block) 을 동적으로 할당한 후,  
shared_ptr 들이 이 제어 블록에 필요한 정보를 공유하는 방식으로 구현됨  

shared_ptr 는 복사 생성할 때 마다 해당 제어 블록의 위치만 공유하면 되고,  
shared_ptr 가 소멸할 때 마다 제어 블록의 참조 개수를 하나 줄이고, 생성할 때 마다 하나 늘리는 방식으로 작동할 것  

```
std::shared_ptr<A> p1 = std::make_shared<A>();
```
make_shared 함수는 A 의 생성자의 인자들을 받아서 이를 통해 객체 A 와 shared_ptr 의 제어 블록 까지 한 번에 동적 할당 한 후에 만들어진 shared_ptr 을 리턴  

### ▷ shared_ptr 생성 시 주의 할 점
shared_ptr 은 인자로 주소값이 전달된다면, 마치 자기가 해당 객체를 첫번째로 소유하는 shared_ptr 인 것 마냥 행동함  

```
A* a = new A();
std::shared_ptr<A> pa1(a);
std::shared_ptr<A> pa2(a);
```
예를 들어 위 코드를 실행하면 두 개의 제어 블록이 따로 생성됨  

따라서 위와 같이 각각의 제어 블록들은, 다른 제어 블록들의 존재를 모르고 참조 개수를 1 로 설정하게 됨  
만약에 pa1 이 소멸된다면, 참조 카운트가 0 이 되어서 pa2 가 아직 가리키고 있지만 자신이 가리키는 객체 A 를 소멸시켜 버림  

pa2 의 참조 카운트는 계속 1 이기 때문에 자신이 가리키는 객체가 살아 있을 것이라 생각할 것  
운 좋게도 pa2 를 사용하지 않아도, pa2 가 소멸되면 참조 개수가 0 으로 떨어지고 자신이 가리키고 있는 (이미 해제된) 객체를 소멸시키기 때문에 오류가 발생  

이 문제는 enable_shared_from_this 를 통해 깔끔하게 해결할 수 있음

### ▷ enable_shared_from_this  
우리가 this 를 사용해서 shared_ptr 을 만들고 싶은 클래스가 있다면, enable_shared_from_this 를 상속 받으면 됨  

enable_shared_from_this 클래스에는 shared_from_this 라는 멤버 함수를 정의하고 있는데,  
이 함수는 이미 정의되어 있는 제어 블록을 사용해서 shared_ptr 을 생성함

따라서 이전 처럼 같은 객체에 두 개의 다른 제어 블록이 생성되는 일을 막을 수 있음

한 가지 중요한 점은 shared_from_this 가 잘 작동하기 위해서는 해당 객체의 shared_ptr 가 반드시 먼저 정의되어 있어야만 함  
즉, shared_from_this 는 있는 제어 블록을 확인만 할 뿐, 없는 제어 블록을 만들지는 않음

### ▷ 서로 참조하는 shared_ptr
각 객체가 shared_ptr 를 하나 씩 가지고 있는데, 이 shared_ptr 가 서로를 가리키고 있다면  

객체 1 이 파괴가 되기 위해서는 객체 1 을 가리키고 있는 shared_ptr 의 참조 개수가 0 이 되어야만 함  
즉, 객체 2 가 파괴가 되어야 함  
하지만 객체 2 가 파괴 되기 위해서는 마찬가지로 객체 2 를 가리키고 있는 shared_ptr 의 참조 개수가 0 이 되어야 하는데, 그러기 위해서는 객체 1 이 파괴되어야만 함

이럴경우 두 객체는 파괴될 수 없는 상태가 되어버림  

이 문제는 shared_ptr 자체에 내재되어 있는 문제이기 때문에 shared_ptr 를 통해서는 이를 해결할 수 없음  
이러한 순환 참조 문제를 해결하기 위해 나타난 것이 바로 **weak_ptr**

## ◇ weak_ptr
트리 구조를 지원하는 클래스를 만드려고 할 때,
```
class Node {
  std::vector<std::shared_ptr<Node>> children;
  /* 어떤 타입? */ parent;

 public:
  Node(){};
  void AddChild(std::shared_ptr<Node> node) { children.push_back(node); }
};
```

만약에 일반 포인터(Node *) 로 하게 된다면, 메모리 해제를 까먹고 하지 않을 경우 혹은 예외가 발생하였을 경우 적절하게 자원을 해제하기 어려움  
물론 이미 해제된 메모리를 계속 가리키고 있을 위험도 있음

하지만 이를 shared_ptr 로 하게 된다면 앞서 본 순환 참조 문제가 생김  
부모와 자식이 서로를 가리키기 때문에 참조 개수가 절대로 0 이 될 수 없음  
따라서, 이들 객체들은 프로그램 끝날 때 까지 절대로 소멸되지 못하고 남아있게 됨  

**weak_ptr** 는 일반 포인터와 shared_ptr 사이에 위치한 스마트 포인터  
스마트 포인터 처럼 객체를 안전하게 참조할 수 있게 해주지만, shared_ptr 와는 다르게 참조 개수를 늘리지는 않음  

따라서 설사 어떤 객체를 weak_ptr 가 가리키고 있다고 하더라도, 다른 shared_ptr 들이 가리키고 있지 않다면 이미 메모리에서 소멸되었을 것  

이 때문에 weak_ptr 자체로는 원래 객체를 참조할 수 없고, 반드시 shared_ptr 로 변환해서 사용해야 함  
이 때 가리키고 있는 객체가 이미 소멸되었다면 빈 shared_ptr 로 변환되고, 아닐경우 해당 객체를 가리키는 shared_ptr 로 변환됨  

weak_ptr 는 생성자로 shared_ptr 나 다른 weak_ptr 를 받음  
또한 shared_ptr 과는 다르게, 이미 제어 블록이 만들어진 객체만이 의미를 가지기 때문에,  
평범한 포인터 주소값으로 weak_ptr 를 생성할 수 는 없음  
이 작업은 lock 함수를 통해 수행할 수 있음  

weak_ptr 에 정의된 lock 함수는 만일 weak_ptr 가 가리키는 객체가 아직 메모리에서 살아 있다면 (즉, 참조 개수가 0 이 아니라면) 해당 객체를 가리키는 shared_ptr 을 반환하고,  
이미 해제가 되었다면 아무것도 가리키지 않는 shared_ptr 을 반환  
아무것도 가리키지 않는 shared_ptr 은 false 로 형변환 되므로 if 문으로 간단히 확인할 수 있음  

만약에 가리키는 shared_ptr 은 0 개 지만 아직 weak_ptr 가 남아있다고 할 때  
이 상태에서는 이미 객체는 해제 되어 있을 것이지만 제어 블록 마저 해제해 버린다면,  
제어 블록에서 참조 카운트가 0 이라는 사실을 알 수 없게 됨  

제어 블록을 메모리에서 해제해야 하기 위해서는 이를 가리키는 weak_ptr 역시 0 개여야 함  
따라서 제어 블록에는 참조 개수와 더불어 약한 참조 개수 (weak count) 기록하게 됨

---

# 쓰레드(thread)
프로세스란, 운영체제에서 실행되는 프로그램의 최소 단위  
프로세스는 CPU 의 코어에서 실행되고 있음  

프로세스에서 실행되는 흐름의 단위를 스레드(thread) 라고 부름  

한 개의 프로세스는 최소 한 개 쓰레드로 이루어져 있으며, 여러 개의 쓰레드로 구성될 수 있음  
이렇게 여러개의 쓰레드로 구성된 프로그램을 **멀티 쓰레드 (multithread)** 프로그램 이라 함

쓰레드와 프로세스의 가장 큰 차이점은 프로세스들은 서로 메모리를 공유하지 않음  
하지만 쓰레드의 경우 한 프로세스 안에 쓰레드 1 과 쓰레드 2 가 있다면, 서로 같은 메모리를 공유하게 됨  

프로그램을 멀티 쓰레드로 만드는 것이 유리할까  
1. 병렬 가능한 (Parallelizable) 작업들  
어떠한 작업을 여러개의 다른 쓰레드를 이용해서 좀 더 빠르게 수행하는 것을 **병렬화(parallelize)** 라고 함   
하지만 모든 작업들이 이렇게 병렬화가 가능한 것이 아님  

    예를 들어, 피보나치 수열을 계산하는 프로그램은 병렬화 하는 것이 매우 까다로움  
이러한 문제가 발생하는 근본적인 이유는 어떠한 연산 (연산 A) 을 수행하기 위해 다른 연산 (연산 B)의 결과가 필요하기 때문 이라 볼 수 있음  

    이와 같은 상황을 A 가 B 에 **의존(dependent)** 한다 라고 함

    프로그램 논리 구조 상에서 연산들 간의 의존 관계가 많을 수 록 병렬화가 어려워지고,  
    반대로, 다른 연산의 결과와 관계없이 독립적으로 수행할 수 있는 구조가 많을 수 록 병렬화가 매우 쉬워짐

2. 대기시간이 긴 작업들  
CPU 의 처리 속도에 비해 인터넷은 매우 느림  
ping 시간동안 CPU 코어를 비효율적으로 사용하게 되는 셈  
여러 쓰레드에서 함수를 실행시키면 컨텍스트 스위칭을 통해 기다리는 시간 없이 CPU 를 최대한으로 사용

## ◇ C++ 에서 쓰레드 생성
C++ 11 에서부터 표준에 쓰레드가 추가되면서, 쓰레드 사용이 매우 편리해짐  
```
#include <thread>
```
thread 헤더파일 추가

```
thread t(func);
```
thread 객체를 생성  
이렇게 생성된 `t`는 인자로 전달받은 함수 `func`를 새로운 쓰레드에서 실행하게 됨  

한 가지 중요한 사실은 이 쓰레드 들이 CPU 코어에 어떻게 할당되고, 또 언제 컨텍스트 스위치를 할 지는 전적으로 운영체제에게 달려있다는 점  

```
t.join();
```
`join` 은 해당하는 쓰레드들이 실행을 종료하면 리턴하는 함수

join을 하지않으면 쓰레드들의 내용이 채 실행되기 전에 main 함수가 종료되어서 쓰레드 객체들의 소멸자가 호출

C++ 표준에 따르면, join 되거나 detach 되지 않는 쓰레드들의 소멸자가 호출된다면 예외를 발생시키도록 명시되어 있음  
따라서, 우리의 쓰레드 객체들이 join 이나 detach 모두 되지 않았으므로 위와 같은 문제가 발생하게 됨  

`detach`는 해당 쓰레드를 실행 시킨 후, **잊어버리는 것**  
대신 쓰레드는 알아서 백그라운드에서 돌아가게 됨

## ◇ 쓰레드에 인자 전달하기  
쓰레드를 생성할 때 함수에 인자들을 전달하는 방법은 매우 간단  
thread 생성자의 첫번째 인자로 함수 (정확히는 Callable) 를 전달하고, 이어서 해당 함수에 전달할 인자들을 써주면 됨

각 쓰레드에는 고유 아이디 번호가 할당 됨  
만약에 지금 어떤 쓰레드에서 작업중인지 보고싶다면 `this_thread::get_id` 함수를 통해서 현재 돌아가고 있는 쓰레드의 아이디를 알 수 있음

쓰레드는 리턴값 이란것이 없기 때문에 만일 어떠한 결과를 반환하고 싶다면 포인터의 형태로 전달해야함  

## ◇ 경쟁 상태(race condtion), 뮤텍스 (mutex)
**경쟁 상태(race condtion)** 는 서로 다른 쓰레드들이 동일한 자원을 사용할 때 발생하는 문제  

한 번에 한 쓰레드에서만 코드를 실행시킬 수 있게 해주는 기능을 하는 객체가 **뮤텍스 (mutex)**  
mutex 라는 단어는 영어의 **상호 배제 (mutual exclusion)** 라는 단어에서 따온 단어  

```
std::mutex m;
```  
뮤텍스 객체 정의

```
void worker(int& result, std::mutex& m)
```
뮤텍스를 각 쓰레드에서 사용하기 위해 전달

```
m.lock();
result += 1;
m.unlock();
```
실제 사용  
`m.lock()` 은 뮤텍스 m을 내가 소유하겠다 라는 뜻  
이 때 중요한 사실은, 한 번에 한 쓰레드에서만 m 의 사용 권한을 갖는다는 것
다른 쓰레드에서 `m.lock()` 을 하였다면 m 을 소유한 쓰레드가 `m.unlock()` 을 통해 m 을 반환할 때 까지 무한정 기다리게 됨  

`m.lock()` 과 `m.unlock()` 사이에 한 쓰레드만이 유일하게 실행할 수 있는 코드 부분을 **임계 영역(critical section)** 이라고 부름  

뮤텍스를 취득한 쓰레드가 unlock 을 하지 않으므로, 다른 모든 쓰레드들이 기다리게 됨  
결국 아무 쓰레드도 연산을 진행하지 못하게 됨  
이러한 상황을 **데드락 (deadlock)** 이라고 함  

뮤텍스도 마찬가지로 사용 후 해제 패턴을 따르기 때문에 동일하게 소멸자에서 처리할 수 있음  

```
void worker(int& result, std::mutex& m) {
  for (int i = 0; i < 10000; i++) {
    // lock 생성 시에 m.lock() 을 실행한다고 보면 된다.
    std::lock_guard<std::mutex> lock(m);
    result += 1;

    // scope 를 빠져 나가면 lock 이 소멸되면서
    // m 을 알아서 unlock 한다.
  }
}
```

```
std::lock_guard<std::mutex> lock(m);
```
lock_guard 객체는 뮤텍스를 인자로 받아서 생성하게 되는데, 이 때 생성자에서 뮤텍스를 lock 하게 됨  
그리고 lock_guard 가 소멸될 때 알아서 lock 했던 뮤텍스를 unlock 하게 됨  

## ◇ 데드락 (deadlock)  
```
#include <iostream>
#include <mutex>  // mutex 를 사용하기 위해 필요
#include <thread>

void worker1(std::mutex& m1, std::mutex& m2) {
  for (int i = 0; i < 10000; i++) {
    std::lock_guard<std::mutex> lock1(m1);
    std::lock_guard<std::mutex> lock2(m2);
    // Do something
  }
}

void worker2(std::mutex& m1, std::mutex& m2) {
  for (int i = 0; i < 10000; i++) {
    std::lock_guard<std::mutex> lock2(m2);
    std::lock_guard<std::mutex> lock1(m1);
    // Do something
  }
}

int main() {
  int counter = 0;
  std::mutex m1, m2;  // 우리의 mutex 객체

  std::thread t1(worker1, std::ref(m1), std::ref(m2));
  std::thread t2(worker2, std::ref(m1), std::ref(m2));

  t1.join();
  t2.join();

  std::cout << "끝" << std::endl;
}
```
위 코드에서 뮤텍스를 얻는 순서를 보면
```
std::lock_guard<std::mutex> lock1(m1);
std::lock_guard<std::mutex> lock2(m2);
```
worker1에서는 m1 을 먼저 lock 한 후 m2 를 lock 하게 됨

```
std::lock_guard<std::mutex> lock2(m2);
std::lock_guard<std::mutex> lock1(m1);
```
worker2에서는 m2 를 먼저 lock 한 후 m1 을 lock 하게 됨  

worker1 에서 m2 를 lock 하기 위해서는 worker2 에서 m2 를 unlock 해야 됨  
하지만 그러기 위해서는 worker2 에서 m1 을 lock 해야 함  
그런데 이 역시 불가능, 왜냐하면 worker1 에 m1 을 lock 하고 있기 때문  

즉 worker1 과 worker2 모두 이러지도 저러지도 못하는 데드락 상황에 빠지게 됨  
분명히 뮤텍스를 lock 하면 반드시 unlock 한다라는 원칙을 지켰음에도 불구하고 데드락을 피할 수 없었음  

한 가지 방법으로는 한 쓰레드에게 우선권을 주는 것입니다.  
한 쓰레드에게 항상 먼저 처리하도록 우선권을 주는 것

적어도 쓰레드들이 뒤엉켜서 아무도 못하는 상황은 막을 수 있음  
하지만 한 쓰레드만 실행되고 다른 쓰레드는 실행될 수 없는 **기아 상태(starvation)** 가 발생할 수 있습니다.  

위에서 말한 해결 방식을 코드로 옮기면  
```
m1.lock();
m2.lock();
std::cout << "Worker1 Hi! " << i << std::endl;

m2.unlock();
m1.unlock();
```
일단 worker1 은 lock_guard 를 통해 구현한 부분을 그대로 옮겨왔음  
worker1 이 뮤텍스를 갖고 경쟁할 때 우선권을 가지므로 굳이 코드를 바꿀 필요가 없음  

```
while (true) {
  m2.lock();

  // m1 이 이미 lock 되어 있다면 unlock
  if (!m1.try_lock()) {
    m2.unlock();
    continue;
  }

  std::cout << "Worker2 Hi! " << i << std::endl;
  m1.unlock();
  m2.unlock();
  break;
}
```
C++ 에서는 try_lock 이라는 함수를 제공하는데, 이 함수는 만일 m1 을 lock 할 수 있다면 lock 을 하고 true 를 리턴함  
그런데 lock() 함수와는 다르게, lock 을 할 수 없다면 기다리지 않고 그냥 false 를 리턴함  

따라서 m1.try_lock() 이 true 를 리턴하였다면  
worker2 가 m1 과 m2 를 성공적으로 lock 한 상황이므로 그대로 처리하면 됨

반면에 m1.try_lock() 이 false 를 리턴하였다면  
worker1 이 이미 m1 을 lock 했다는 의미, 이 경우 worker1 에서 우선권을 줘야 하기 때문에 자신이 이미 얻은 m2 역시 worker1 에게 제공해야 함  

그 후에 while 을 통해 m1 과 m2 모두 lock 하는 것을 성공할 때 까지 계속 시도하게 되며,  
성공하게 되면 while 을 빠져나감

데드락을 해결하는 것은 매우 복잡하며 완벽하지 않음  
따라서 데드락 상황이 발생할 수 없게 프로그램을 잘 설계하는 것이 중요

데드락 상황을 피하기 위한 가이드라인

**중첩된 Lock 을 사용하는 것을 피해라**
모든 쓰레드들이 최대 1 개의 Lock 만을 소유한다면 (일반적인 경우에) 데드락 상황이 발생하는 것을 피할 수 있음  
또한 대부분의 디자인에서는 1 개의 Lock 으로도 충분함  
만일 여러개의 Lock 을 필요로 한다면 정말 필요로 하는지를 되물어보는 것이 좋음

**Lock 을 소유하고 있을 때 유저 코드를 호출하는 것을 피해라**
유저 코드에서 Lock 을 소유할 수 도 있기에 중첩된 Lock 을 얻는 것을 피하려면  
Lock 소유시 유저 코드를 호출하는 것을 지양해야 함

**Lock 들을 언제나 정해진 순서로 획득해라**
만일 여러개의 Lock 들을 획득해야 할 상황이 온다면, 반드시 이 Lock 들을 정해진 순서로 획득해야 함  
앞선 예제에서 데드락이 발생했던 이유 역시, worker1 에서는 m1, m2 순으로 lock 을 하였지만 worker2 에서는 m2, m1 순으로 lock 을 하였기 때문  
만일 worker2 에서 역시 m1, m2 순으로 lock 을 하였다면 데드락은 발생하지 않았을 것  

## ◇ 생산자(Producer) 와 소비자(Consumer) 패턴
생산자의 경우, 무언가 처리할 일을 받아오는 쓰레드를 의미  
예를 들어, 인터넷에서 페이지를 긁어서 분석하는 프로그램을 만든다고 할 때 페이지를 긁어 오는 쓰레드가 생산자

소비자의 경우, 받은 일을 처리하는 쓰레드를 의미  
앞선 예제의 경우 긁어온 페이지를 분석하는 쓰레드가 소비자  

생산자(Producer) 
```
// 웹사이트를 다운로드 하는데 걸리는 시간이라 생각하면 됨
// 각 쓰레드 별로 다운로드 하는데 걸리는 시간이 다름
std::this_thread::sleep_for(std::chrono::milliseconds(100 * index));
std::string content = "웹사이트 : " + std::to_string(i) + " from thread(" + std::to_string(index) + ")\n";

// downloaded_pages 는 쓰레드 사이에서 공유되므로 critical section 에 넣어야 함
m->lock();
downloaded_pages->push(content);
m->unlock();
```
이제 다운 받은 페이지를 작업 큐에 집어 넣어야 함  
이 때 주의할 점으로, producer 쓰레드가 1 개가 아니라 5 개나 있다는 점  
따라서 downloaded_pages 에 접근하는 쓰레드들 사이에 race condition 이 발생할 수 있음

이를 방지 하기 위해서 뮤텍스 m 으로 해당 코드를 감싸서 문제가 발생하지 않게 해줌

consumer 쓰레드의 입장에서는 언제 일이 올지 알 수 없기 때문에  
downloaded_pages 가 비어있지 않을 때 까지 계속 while 루프를 돌아야함  
한 가지 문제는 컴퓨터 CPU 의 속도에 비해 웹사이트 정보가 큐에 추가되는 속도는 매우 느리다는 점
따라서 아래와 같이 구현
```
m->lock();
// 만일 현재 다운로드한 페이지가 없다면 다시 대기.
if (downloaded_pages->empty()) {
  m->unlock();

  // 10 밀리초 뒤에 다시 확인한다.
  std::this_thread::sleep_for(std::chrono::milliseconds(10));
  continue;
}
```

`downloaded_pages->empty()` 라면, 강제로 쓰레드를 sleep 시켜서 10 밀리초 뒤에 다시 확인하는 식으로 구현

`m->unlock` 을 위 if 문 안에서 호출하지 않는다면 데드락이 발생하게 됨  

```
// 맨 앞의 페이지를 읽고 대기 목록에서 제거한다.
std::string content = downloaded_pages->front();
downloaded_pages->pop();

(*num_processed)++;
m->unlock();

// content 를 처리한다.
std::cout << content;
std::this_thread::sleep_for(std::chrono::milliseconds(80));
```

마지막으로 content 를 처리하는 과정은 front 를 통해서 맨 앞의 원소를 얻은 뒤에, pop 을 호출하면 맨 앞의 원소를 큐에서 제거함

이 때 m->unlock 을 함으로써 다른 쓰레드에서도 다음 원소를 바로 처리할 수 있도록 해야함

위 구현에서 consumer 쓰레드가 10 밀리초 마다 downloaded_pages 에 할일이 있는지 확인하고 없으면 다시 기다리는 형태를 취하고 있음  

이는 매우 비효율적  
매 번 언제 올지 모르는 데이터를 확인하기 위해 지속적으로 mutex 를 lock 하고, 큐를 확인해야 하기 때문

이보다는 producer 에서 데이터가 뜸하게 오는 것을 안다면 consumer 는 아예 재워놓고, producer 에서 데이터가 온다면 consumer 를 깨우는 방식이 효율적  
쓰레드를 재워놓게 되면, 그 사이에 다른 쓰레드들이 일을 할 수 있기 때문에 CPU 를 더 효율적으로 쓸 수 있을 것

### ▷ 조건 변수(condition_variable)
위와 같은 상황에서 쓰레드들을 10 밀리초 마다 재웠다 깨웠다 할 수 밖에 없었던 이유는 '어떠한 조건을 만족할 때 까지 자라' 라는 명령을 내릴 수 없었기 때문  

이는 **조건 변수(condition_variable)** 를 통해 해결할 수 있음  

```
condition_variable cv;
```
먼저 뮤텍스를 정의할 때와 같이 condition_variable 을 정의  

**consumer 쓰레드**
```
void consumer(std::queue<std::string>* downloaded_pages, std::mutex* m,
              int* num_processed, std::condition_variable* cv) {
  while (*num_processed < 25) {
    std::unique_lock<std::mutex> lk(*m);

    cv->wait(lk, [&] { return !downloaded_pages->empty() || *num_processed == 25; });

    if (*num_processed == 25) {
      lk.unlock();
      return;
    }

    // 맨 앞의 페이지를 읽고 대기 목록에서 제거한다.
    std::string content = downloaded_pages->front();
    downloaded_pages->pop();

    (*num_processed)++;
    lk.unlock();

    // content 를 처리한다.
    std::cout << content;
    std::this_thread::sleep_for(std::chrono::milliseconds(80));
  }
}
```

```
std::unique_lock<std::mutex> lk(*m);

cv->wait(lk, [&] { return !downloaded_pages->empty() || *num_processed == 25; });
```
condition_variable 의 wait 함수에 어떤 조건이 참이 될 때 까지 기다릴지 해당 조건을 인자로 전달해야 함  

조건 변수는 만일 해당 조건이 거짓이라면, lk 를 unlock 한 뒤에, 영원히 sleep 하게 됨  
이 때 이 쓰레드는 다른 누가 깨워주기 전까지 계속 sleep 된 상태로 기다리게 됨  
한 가지 중요한 점이라면 lk 를 unlock 한다는 점

반면에 해당 조건이 참이라면 cv.wait 는 그대로 리턴해서 consumer 의 content 를 처리하는 부분이 그대로 실행되게 됨  

```
std::unique_lock<std::mutex> lk(*m);
```

`unique_lock` 은 `lock_guard` 와 거의 동일  
다만, `lock_guard` 의 경우 생성자 말고는 따로 lock 을 할 수 없는데, `unique_lock` 은 unlock 후에 다시 lock 할 수 있음

위에서 `unique_lock` 을 사용한 이유는 `cv->wait` 가 `unique_lock` 을 인자로 받기 때문

**producer 쓰레드**
```
void producer(std::queue<std::string>* downloaded_pages, std::mutex* m,
              int index, std::condition_variable* cv) {
  for (int i = 0; i < 5; i++) {
    // 웹사이트를 다운로드 하는데 걸리는 시간이라 생각하면 된다.
    // 각 쓰레드 별로 다운로드 하는데 걸리는 시간이 다르다.
    std::this_thread::sleep_for(std::chrono::milliseconds(100 * index));
    std::string content = "웹사이트 : " + std::to_string(i) + " from thread(" +
                          std::to_string(index) + ")\n";

    // data 는 쓰레드 사이에서 공유되므로 critical section 에 넣어야 한다.
    m->lock();
    downloaded_pages->push(content);
    m->unlock();

    // consumer 에게 content 가 준비되었음을 알린다.
    cv->notify_one();
  }
}
```

```
// consumer 에게 content 가 준비되었음을 알린다.
cv->notify_one();
```

만약에 페이지를 하나 다운 받았다면, 잠자고 있는 쓰레드들 중 하나를 깨워서 일을 시킴  
(만약에 모든 쓰레드들이 일을 하고 있는 상태라면 아무 일도 일어나지 않음)  
notify_one 함수는 조건이 거짓이라 자고 있는 쓰레드 중 하나를 깨워서 조건을 다시 검사하게 해줌  
만일 조건이 참이 된다면 그 쓰레드가 다시 일을 시작함  

```
for (int i = 0; i < 5; i++) {
  producers[i].join();
}

// 나머지 자고 있는 쓰레드들을 모두 깨운다.
cv.notify_all();
```

producer 들이 모두 일을 끝낸 시점을 생각해본다면,  
자고 있는 일부 consumer 쓰레드들이 있을 것  
만약에 `cv.notify_all()` 을 하지 않는다면, 자고 있는 consumer 쓰레드들의 경우 join 되지 않는 문제가 발생함  

따라서 마지막으로 `cv.notify_all()` 을 통해서 모든 쓰레드를 깨워서 조건을 검사하도록 함  
해당 시점에선 이미 num_processed 가 25 가 되어 있을 것이므로, 모든 쓰레드들이 잠에서 깨어나 종료하게 됨