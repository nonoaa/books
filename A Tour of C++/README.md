# A Tour of C++ : Third Edition
## 1장 기초 쌓기
- 프로그램
	- C++는 컴파일 언어이다.
	- ISO C++ 표준은 두 종류의 엔티티를 정의한다.
		- 내장 타입(예: char와 Int)과 루프(예: for문과 while문) 같은 핵심 언어 기능
		- 컨테이너(예: vector와 map)와 I/O 연산(예: <<와 getline()) 같은 표준 라이브러리 컴포넌트
	- C++는 동적 타입(statically typed) 언어다. 즉, 모든 엔티티(예: 객체, 값, 이름, 식)의 타입을 사용 시점에 컴파일러에게 알려야 한다.
- Hello, World!
	- 모든 C++ 프로그램은 main()이라는 전역 함수를 딱 하나만 포함해야 한다.
	- import std;
		- 선언한 표준 라이브러리를 사용할 수 있도록 준비하라고 컴파일러에게 지시한다.
		- import 디렉티브는 C++20부터 새로 생겼으며 모든 표준 라이브러리를 모듈 std로 나타내는 방식은 아직 표준이 아니다.
		- 기존 관습대로 #include \<iostream\>과 같이 사용해도 된다.
- 타입과 변수, 산술 연산
	- 선언은 프로그램에게 엔티티를 알리고 타입을 명시하는 명령문이다.
		- 타입은 가능한 값 집합과 연산 집합(객체일 때)을 정리한다.
		- 객체는 어떤 타입의 값을 저장하는 메모리 영역이다.
		- 값은 타입에 따라 다르게 해석되는 비트 집합이다.
		- 변수는 명명된 객체이다.
	- char는 일반적으로 8-bit 바이트이다.
	- sizeof(int)는 보통 4다. 다른 크기의 타입을 사용하려면 int32_t 같은 표준 라이브러리 타입 에일리어스를 이용한다.
	- 수는 부동소수점이거나 정수일 수 있다.
		- 부동소수점 리터럴은 소수점(예: 3.14)이나 지수(예: 314e-2)로 인식된다.
		- 정수 리터럴은 기본적으로 십진수이다. 0b 접두사는 2진수 정수 리터럴을 가리킨다. 0x접두사는 16진수 정수 리터럴을 가리킨다. 0접두사는 8진수 정수 리터럴을 가리킨다.
		- 긴 리터럴을 보다 편하게 읽으려면 작은따옴표(')를 숫자 구분 기호로 사용한다. 예를 들어 파이는 3.14159'26535'89793'23846이다.
- 산술 연산
	- x.y, x->y, x(y), x[y], x<<y, x&&y, x||y는 왼쪽에서 오른쪽으로 평가한다.
	- 최적화와 관련된 이슈로 인해 다른 식(예: f(x)+g(y))과 함수 인수(예: h(f(x), g(y)))는 안타깝게도 평가 순서가 불확정이다.
- 초기화
	- 객체를 사용하려면 먼저 값을 넣어야 한다.
	- =를 사용하거나 보다 보편적인 방법으로 중괄호로 구분한 초기자 리스트를 사용한다.
		```cpp
		double d1 = 2.3;
		double d2 {2.3};
		double d3 = {2.3};

		complex<double> z = 1;
		complex<double> z2 {d1, d2};
		complex<double> z3 = {d1, d2};

		vector<int> v {1, 2, 3, 4, 5, 6};
		```
	- = 형식은 예전부터 C에서 써 왔으나 의심스러우면 보편적인 {}-리스트 형식을 사용하자. 적어도 정보를 잃는 변환은 발생하지 않는다.
		```cpp
		int i1 = 7.8; // i1은 7이 된다.
		int i2 {7.8}; // 오류: 부동소수점에서 정수로 변환
		```
	- 불행히도 double에서 int, int에서 char처럼 정보를 잃는 축소 변환이 일어날 수 있으며, =를 사용하면 암묵적으로 적용된다. 하지만 {}를 사용할 때는 아니다.
	- 변수를 정의할 때 초기자로부터 타입을 추론할 수 있으면 명시적으로 타입을 적지 않아도 된다.
		```cpp
		auto b = true;	//bool
		auto ch = 'x';	//char
		auto d = 1.2;	//double
		auto bb {true};	//bool
		```
	- auto를 사용하면 골치 아픈 타입 변환이 일어날 수 있어 대개 =를 선호한다.
	- 타입을 명시적으로 언급할 특별한 이유가 없으면 auto를 사용한다. 특별한 이유란 다음과 같다.
		- 정의가 광범위해서 코드의 독자에게 타입을 분명히 알리고 싶을 때
		- 초기자의 타입이 분명하지 않을 때
		- 변수의 범위나 정밀도를 분명히 밝히고 싶을 때(예: float 대신 double)
- 범위와 수명
	- 선언은 어떤 범위에 그 이름을 알린다.
		- 지역 범위: 함수나 람다에 선언된 이름을 지역명이라 부른다. 블록은 {}쌍으로 구분한다. 함수 인수명은 지역명으로 간주된다.
		- 클래스 범위: 클래스 안이나 함수, 람다, enum class 밖에 정의된 이름은 멤버명이라 부른다. 선언을 감싸는 여는 {부터 닫는}까지가 범위이다.
		- 네임스페이스 범위: 함수느 람다, 클래스, enum class 밖 네임스페이스 안에 정의된 이름을 네임스페이스 멤버명이라 부른다. 선언 지점부터 그 네임스페이스 끝까지가 범위이다.
	- 어떤 구조체 안에도 선언되지 않은 이름을 전역명이라 부르며, 전역 네임스페이스에 속한다고 말한다.
	- 이 밖에 new로 생성한 템포러리와 객체처럼 이름이 없는 객체도 있다.
	- 객체는 사용하기 전에 반드시 생성돼야(초기화돼야) 하며 범위의 끝에서 소멸된다. 네임스페이스 객체의 경우 소멸 지점은 프로그램의 끝이다. 객체의 멤버라면 소멸 지점은 그 객체가 소멸되는 시점으로 결정된다.
- 상수
	- C++는 두가지 불변성 표기법을 지원한다.
		- const: "이 값을 바꾸지 않겠다고 약속해" 정도로 이해하면 된다. 인터페이스를 명시할 때 주로 사용한다. 수정될 염려 없이 포인터와 참조를 사용해 함수에 데이터를 전달하기 위해서다. 컴파일러는 const로 명시한 약속을 이행한다. const의 값은 런타임에 계산될 수 있다.
		- constexpr: "컴파일 타임에 평가된다" 정도로 이해하면 된다. 주로 상수를 명시하거나 (손상될 가능성이 거의 없는) 읽기 전용 메모리 내 데이터를 넣거나 성능을 높이기 위해 쓴다. constexpr의 값은 컴파일러가 계산해야 한다.
		```cpp
		int var;
		const double sqv = sqrt(var);		// sqv는 명명된 상수이며 런타임에 계산될 수 있다.

		double sum(const vector<double>&);	// sum은 인수를 수정하지 않는다.

		vector<double> v {1.2, 3.4, 4.5};	// v는 상수가 아니다.
		const double s1 = sum(v);			// OK: sum(v)은 런타임에 평가된다.
		constexpr double s2 = sum(v);		// error: sum(v)은 상수식이 아니다.
		```
	- 상수식, 즉 컴파일러가 평가할 식에 함수를 사용하려면 그 함수를 constexpr이나 consteval로 정의해야 한다.
		```cpp
		constexpr double square(double x) { return x*x; }
		constexpr double max1 = 1.4*square(17);		// OK: 1.4*square(17)는 상수식이다.
		constexpr double max2 = 1.4*square(var);	// error: var는 상수가 아니므로 square(var)는 상수가 아니다.
		
		const double max3 = 1.4*square(var);		// OK
		```
		- 함수를 컴파일 타임 평가에만 사용하려면 constexpr 대신 consteval로 선언한다.
		```cpp
		consteval double square2(double x) { return x*x; }
		constexpr double max1 = 1.4*square(17);		// OK: 1.4*square(17)는 상수식이다.
		const double max3 = 1.4*square(var);		// error: var는 상수가 아니다.
		```
	- constexpr이나 consteval로 선언한 함수는 C++ 버전의 순수 함수 개념이다. 부수 효과가 발생하지 않고 인수로 전달되는 정보만 이용할 수 있다.
- 포인터, 배열, 참조
	- char v[6]; // 문자 6개짜리 배열
	- char* p; // 문자로의 포인터
	- char* p = &v[3]; p는 v의 네 번째 원소를 가리킨다.
	- char x = *p;
	- 단항 접두사 *는 "~에 담긴 내용", 단항 접두사 &는 "~의 주소"를 뜻한다.
	- 범위 기반 for문
		```cpp
		void print()
		{
			int v[] = {0, 1, 2, 3, 4, 5};

			for (auto x : v)
				cout << x << '\n';

			for (auto x : {10, 21, 32, 43, 54})
				cout << x << '\n'; 

			for (auto& x : v)
				++x;
		}
		```
		- 첫 번째 for문은 "v의 각 원소를 처음 원소부터 마지막 원소까지 x에 하나씩 복사한 후 출력하라"라고 읽는다.
		- 복사하지 않고 싶으면 3번째 for문처럼 사용한다.
- 널 포인터
	- 가리키는 객체가 없거나 "어떤 객체도 사용할 수 없다"는 개념을 나타내려면 포인터에 nullptr값을 할당한다. 모든 포인터 타입에 nullptr을 공통적으로 사용한다.
	- 기존 코드에서는 전형적으로 0이나 NULL을 사용했다. 하지만 nullptr을 사용하면 정수와 포인터 간 잠재적 혼란이 발생하지 않는다.
- 하드웨어와의 매핑
	- 할당
		```cpp
		int x = 2;
		int y = 3;
		x = y;	// x는 3이 되므로 x == y다
		```
		***
		```cpp
		int x = 2;
		int y = 3;
		int *p = &x;
		int *q = &y;	// p!=q이고 *p!=*q이다
		p = q;		// p는 &y가 된다. 이제 p==q이므로 *p==*q이다
		```
		***
		```cpp
		int x = 2;
		int y = 3;
		int& r = x;	// r은 x를 참조
		int& r2 = y;	// r2는 y를 참조
		r = r2;		// r2를 통해 읽어 r을 통해 쓰면 x는 3이 된다.
		```
	- 초기화
		- 초기화는 할당과 다르다. 초기화는 초기화하지 않은 메모리 조각을 유효한 객체로 만드는 작업이다. 거의 모든 타입에서 초기화하지 않은 변수를 읽거나 쓸 때 어떤 일이 일어날지 확실하지 않다. 참조를 생각해보자
		```cpp
		int x = 7;
		int& r {x};	// r을 x에 바인딩한다(r은 x를 참조한다)
		r = 7;		// r이 참조하는 객체에 할당한다

		int& r2;	// 오류: 초기화하지 않은 참조
		r2 = 99;	// r2가 참조하는 객체에 할당한다
		```
		다행히 초기화하지 않은 참조는 생성할 수 없다. 생성했다면 r2=99가 불특정 메모리 위치에 99를 할당해 잘못된 결과로 이어졌을 것이다.
## 2장 사용자 정의 타입
- 구조체
- 클래스
	- 클래스는 타입의 인터페이스(누구나 접근하는 부분)와 구현(그 밖에 접근 불가능한 데이터에 접근하는 부분)을 구분하도록 되어 있다.
	- 클래스는 멤버들의 집합이며, 멤버는 데이터나 함수, 타입 멤버일 수 있다.
	- 클래스의 인터페이스는 public 멤버로 정의되며, private 멤버는 인터페이스로만 접근할 수 있다.	
	- 클래스 선언에서 public과 private의 순서는 중요하지 않으나 표현을 강조하고 싶을 때를 제외하고는 관례상 public을 먼저, private을 나중에 선언한다.
	- struct와 class는 기본적으로 차이가 없다. struct는 기본값으로 public 멤버를 갖는 class일 뿐이다.
- 열거
	- enum class
		```cpp
		enum class Color {red, blue, green};
		enum class Traffic_light {green, yellow, red};

		Color col = Color::red;
		Traffic_light light = Traffic_light::red;
		```
		- enum 뒤에 나오는 class는 열거가 강형(strongly-typed)임을 명시하고 열거자의 범위를 지정한다.
		- enum class는 우연히 상수를 잘못 사용하는 일이 없도록 방지한다. 즉, Color과 Traffic_light이 섞이지 않는다.
		```cpp
		Color x1 = red;					// 오류: 어떤 red인가?
		Color y2 = Traffic_light::red;	// 오류: 이 red는 Color가 아니다.
		Color z3 = Color::red;			// OK
		auto x4 = Color::red;			// OK
		```
		- 마찬가지로 Color와 정수 값도 암묵적으로 섞이지 않는다.
		```cpp
		int i = Color::red;		// 오류: Color::red는 int가 아니다.
		Color c = 2;			// 초기화 오류: 2는 Color가 아니다.
		```
		- enum으로 변환하려는 시도를 잡아내 오류를 방지하면 좋겠지만 대개 enum의 내부 타입(기본값은 int) 값으로 enum을 초기화하려 하므로 허용되며, 내부 타입을 enum으로 명시적으로 변환할 때도 마찬가지이다.
		```cpp
		Color x = Color{5};			// OK
		Color y {6};				// OK

		int x = int(Color::red);	// OK
		```
		- Traffic_light라는 열거 이름을 반복해서 쓰기 귀찮으면 특정 범위 안에서 줄여 쓸 수 있다.
		```cpp
		Traffic_light& operator++(Traffic_light& t)
		{
			using enum Traffic_light; //여기서는 Traffic_light를 사용한다.

			switch (t) {
				case green: return t=yellow;
				case yellow: return t=red;
				case red: return t=green;
			}
		}
		```
		- 열거자 이름을 명시적으로 한정하고 싶지 않고 (명시적 변환이 필요 없도록) 열거자 값을 int로 두려면 enum class에서 class를 빼고 "일반적인" enum으로 만든다.
		- "일반적인" enum은 C++ 초창기부터 지원한 기능이라 동작이 매끄럽지 않은데도 요즘 코드에 흔히 보인다.
- 공용체(union)
	- 공용체는 모든 멤버를 같은 주소에 할당한 struct로서 그 union에서 가장 큰 멤버의 크기만큼 공간을 차지한다.
	- 다시 말해 union에는 1번에 딱 한 멤버의 값만 저장할 수 있다.
	```cpp
	enum class Type {ptr, num};

	struct Entry{
		string name;
		Type t;
		Node* p;	// t==Type::ptr이면 p를 사용한다
		int i;		// t==Type::num이면 i를 사용한다
	}

	void f(Entry* pe)
	{
		if (pe->t == Type::num)
		{
			cout << pe->i;
			// ...
		}
	}
	```
	- 멤버 p와 i를 동시에 저장할 일이 없으니 이렇게 하면 공간이 낭비된다. 아래처럼 union 멤버로 명시하면 문제가 쉽게 풀린다.
	```cpp
	union Value {
		Node* p;
		int i;
	}
	```
	- Value::p와 Value::i는 같은 Value 객체의 메모리 주소에 저장된다.
	- std::variant라는 표준 라이브러리 타입을 이용하면 공용체를 직접 사용하지 않아도 된다.
	- variant는 여러 타입 집합 중 하나의 값을 저장한다. 예를 들어 variant\<Node*, int>에 Node*나 int를 저장할 수 있다.
	```cpp
	struct Entry{
		string name;
		variant<Node*, int> v;
	};

	void f(Entry* pe)
	{
		if (holds_alternative<int>(pe->v))	// *pe가 int를 저장하는가?
			cout << get<int>(pe->v);		// int를 가져온다
		// ...
	}
	```
	- 많은 경우 union보다 std::variant가 사용하는 편이 더 간단하고 안전하다.

## 3장 모듈성
- 분리 컴파일
	- C++는 사용자 코드에 사용할 타입과 함수의 선언만 보여지는 분리 컴파일개념을 지원한다.
		- 헤더 파일: 헤더파일이라는 별개의 파일에 선언을 넣은 후, 선언이 필요한 곳에서 헤더 파일명으로 헤더 파일을 #include한다.
			- 헤더 파일과 #include는 모듈성을 시뮬레이션하는 아주 오래된 방법이며, 몇 가지 심각한 단점을 지닌다.
				- 컴파일 시간: 101개의 번역 단위에서 header.h를 #include하면 컴파일러는 header.h의 텍스트를 101번 처리한다.
				- 순서 종속성: header2.h보다 header1.h를 먼저 #include하면 header1.h에 들어있는 선언과 매크로가 header2.h 내 코드의 의미를 바꿀 수 있다. 
				반대로 header1.h보다 header2.h를 먼저 #include하면 header2.h가 header1.h 내 코드에 영향을 미칠 수 있다.
				- 비일관성: 타입이나 함수 같은 엔티티를 한 파일에 정의한 후 조금 다르게 또 다른 파일에 정의하면 고장이나 알아채기 어려운 오류로 이어질 수 있다. 
				이러한 문제는 우연히 혹은 고의로 엔티티를 한 헤더가 아니라 두 소스 파일에 별도로 선언하거나 헤더 파일 간 순서 종속성을 통해 선언할 때 발생한다.
				- 이행성: 헤더 파일 내 선언 표현에 필요한 모든 코드는 그 헤더 파일에 제시해야 한다. 이렇게 하지 않으면 헤더 파일이 다른 헤더를 #include해 코드가 거재해지고, 이로 인해 
				의도했든 우연이었든 헤더 파일의 사용자는 이러한 세부 구현에 의존할 수밖에 없게 된다.
		- 모듈: module 파일을 정의해 별도로 컴파일한 후, 필요한 곳에서 import한다. 명시적으로 export한 선언만이 그 module을 import한 코드에 보여진다.
			- C++20부터 드디어 언어 단에서 모듈성을 직접적으로 표현하는 방법을 지원하기 시작했다.
			```cpp
			export module Vector;
			
			export class Vector {
			public:
				Vector(int s);
				double& operator[](int i);
			private:
				double *elem;
			};

			Vector::Vector(int s):elem{new double[s]}
			{
			}

			double& Vector::operator[](int i)
			{
				return elem[i];
			}

			export bool operator==(const Vector& v1, const Vector& v2)
			{
				if (...)
					return false;
				return true;
			}
			```
			- 위 코드는 Vector라는 모듈을 정의한다. 위 module을 사용하려면 필요한곳에서 import하면 된다.
			```cpp
			import Vector;
			#include <cmath>
			```
			- 헤더와 모듈의 차이점
				- 모듈은 딱 한 번 컴파일된다.(그 모듈이 쓰이는 번역 단위마다 컴파일되지 않는다.)
				- 두 모듈은 의미에 영향을 주지 않으면서 어떤 순서로든 임포트할 수 있다.
				- 모듈로 import하거나 #include하면 모듈의 사용자는 암묵적으로 그 모듈에 접근할 수 없다.(또한 그 모듈에 신경 쓰지 않는다.) 즉, import는 이행적이 아니다.
			- 모듈은 유지 보수성과 컴파일 타임 성능에 어마어마한 영향을 끼치기도 한다. 예를 들어 import std;을 사용해 "Hello, World!" 프로그램을 측정하면,
			#include \<iostream>을 사용한 버전보다 10배 더 빨리 컴파일된다. std 모듈이 \<iostream> 헤더보다 정보가 10배 이상 많은 표준 라이브러리 전체를 포함하는데도 더 빠르다. 
			헤더는 직접적으로 혹은 간접적으로 포함하는 것을 전부 컴파일러에게 전달하는 반면, 모듈은 인터페이스로만 익스포트하기 때문이다. 덕분에 너무나 많은 헤더 중에 무엇을 #include해야 
			하는지 외우지 않고도 큰 모듈을 사용할 수 있다. 하지만 안타깝게도 module std는 C++20에 없다.. 부록 A에서 표준 라이브러리 구현에서 module std를 제공하지 않더라도 가져오는 방법을 설명하겠다.
			- 모듈을 정의할 때는 선언과 정의를 별도의 파일로 분리하지 않아도 된다. 소스 코드 구성이 더 나아진다면 모를까 굳이 할 필요 없다. Vector 모듈을 간단히 정의해보겠다.
				```cpp
				export module Vector;

				export class Vector{
					//...
				};

				export bool operator==(const Vector& v1, const Vector& v2)
				{
					//...
				}
				```
				- 이제 프로그램 조각을 그림으로 나타내 보겠다.
					- user.cpp: [import Vector; 벡터를 사용한다] -> [Vector 모듈의 익스포트된 인터페이스] <- Vector.cpp: [export module Vector; Vector를 정의한다]
					- 컴파일러는 export 지정자(specifier)로 명시한 모듈의 인터페이스와 세부 구현을 분리한다. 즉, Vector 인터페이스는 컴파일러가 생성하지만 사용자가 명시적으로 명명하지는 않는다. module을 사용하면 사용자에게 세부 구현을 숨기느라 코드가 복잡해지지 않는다. module은 export된 선언으로의 접근 권한만 부여한다.
- 네임스페이스
	- 함수, 클래스, 열거 외에 C++는 여러 선언을 한데 묶어 서로의 이름이 충돌하지 않도록 표현하는 메커니즘인 네임스페이스를 지원한다.
	- 이름을 반복적으로 한정하면 장황하거나 산만해지므로 using 선언으로 그 이름을 어떤 범위 안으로 가져올 수 있다.
	```cpp
	using std::swap
	swap(x,y);			// std::swap()
	other::swap(x,y);	// 다른 swap()
	```
	- 표준 라이브러리 네임스페이스 내 이름에 접근하려면 using 디렉티브를 사용하자.
	```cpp
	using namespace std;
	```
	- using 디렉티브를 사용하면 그 티렉티브를 둔 범위 안에서는 한정자 없이 명명한 네임스페이스의 이름에 접근할 수 있다. 즉, std에 using 디렉티브를 사용하면 std::cout 대신 간단히 cout으로 작성해도 된다. 하지만 using 디렉티브를 사용하면 네임스페이스 내 이름을 선택적으로 사용할 수 없으므로 신중해야 한다.
- 함수 인수와 반환값
	- 반환 타입 추론
		- 함수의 반환값으로부터 반환 타입을 추론할 수 있다.
		```cpp
		auto mul(int i, double d) { return i * d; }
		```
	- 후위 반환 타입
		- 인수를 보고 결과 타입을 결정해야 할 때도 있다. 반환 타입을 명시하고 싶으면 인수 목록 뒤에 반환 타입을 추가하자. 그럼 auto는 "반환 타입을 나중에 언급하거나 추론하겠다"로 해석된다.
		```cpp
		auto mul(int i, double d) -> double {return i * d; } // 반환 타입은 "double" 이다.

		auto next_elem() -> Elem*;
		auto exit(int) -> void;
		auto sqrt(doube) -> double;
		```
		- 이러한 표기로 이름을 더 깔끔하게 정렬할 수 있다.
	- 구조적 바인딩
		- 함수는 값 하나만 반환할 수 있으나 그 값이 멤버를 여러 개 포함하는 클래스 객체여도 상관 없다. 이러한 방법으로 많은 값을 간결하게 변환할 수 있다.
		```cpp
		struct Entry {
			string name;
			int value;
		}

		Entry read_entry(istream& is)
		{
			string s;
			int i;
			is >> s >> i;
			return {s, i};
		}
		```
		- 여기서 {s, i}는 Entry 반환값을 구성하는 데 쓰인다. 비슷하게 Entry의 멤버를 지역변수로 꺼낼 수 있다.
		```cpp
		auto [n,v] = read_entry(is);
		cout << n << "," << v;
		```
		- auto [n,v]는 두 지역변수 n과 v를 선언하는데, 각각의 타입은 read_entry()의 반환 타입으로부터 추론한다. 클래스 객체의 멤버에 지역명을 부여하는 이러한 메커니즘을 구조적 바인딩이라 부른다.
		- 언제나처럼 auto를 const와 &로 장식할 수 있다.
		```cpp
		map<string, int> m;
		for (const auto [key, value] : m)
			// ...
		
		void incr(map<string,int>& m)
		{
			for (auto& [key, value] : m)
				++value;
		}
		```
		- 프라이빗 데이터가 없는 클래스에 구조적 바인딩을 사용할 때 바인딩되는 방법은 아주 간단하다. 클래스 객체 내 데이터 멤버 수와 바인딩에 정의된 이름 수가 같으며, 바인딩에 넣은 각 이름이 해당하는 멤버를 명명한다. 명시적으로 복합 객체를 사용했을 때와 비교해 객체 코드 품질에 전혀 차이가 없다. 특히 구조적 바인딩을 사용한다고 해서 struct가 복사 되지는 않는다.

## 4장 오류 처리
- 예외
	- Vector의 범위 밖의 원소에 접근하려 할 때 처리
		```cpp
		double& Vector::operator[](int i)
		{
			if (!(0<=i && i<size()))
				throw out_of_range{"Vector::operator[]"};
			return elem[i];
		}
		```
		- throw는 out_of_range 타입의 예외를 처리하는 핸들러를 찾아 제어를 넘긴다.
	- try
		```cpp
		void f(Vecotr& v)
		{
			//...
			try {
				compute1(v);
				Vector v2 = compute2(v);
				compute3(v2);
			}
			catch (const out_of_range& err){
				//...
				cerr << err.what() << '\n';
			}
		}
		```
		- 예외 처리 코드는 try블록에 넣는다. catch절에서 out_of_range 타입의 예외를 처리한다.
		- 예외 처리 메커니즘을 사용함으로써 오류 처리가 더 간단해지고 더 체계적으로 바뀌며 더 읽기 쉬워지지만, 그렇다고 try문을 남용하지 말자.
		- 많은 프로그램에서 throw와 던져진 예외를 아주 적절히 처리하는 함수 사이에서 전형적으로 수십 개의 함수를 호출한다. 즉, 대부분의 함수는 단순히 예외를 호출 스택 위로 전달할 수 있어야 한다.
- 불변
	- 예제에서는 Vector 타입의 객체에 대해 operator[]()를 수행하므로 Vector의 멤버가 "적절한"값이 아니면 무엇도 계산할 수 없다.
	- 클래스에서 참으로 가정하는 것에 대한 이러한 진술을 클래스 불변 또는 간단히 불변(invariant)이라 부른다. 클래스의 불변을 정립하고 함수가 종료될 때까지 멤버 함수가 그 불변을 유지하도록 하는 것이 생성자의 역할이다.
	- 잘 디자인된 코드에는 try 블록이 거의 없다. RAII 기법을 체계적으로 사용함으로써 남용을 막자.
- 오류 처리 대안
	- 함수는 다음의 방법으로 할당된 태스크를 수행할 수 없음을 표시한다.
		- 예외 던지기
		- 어떻게 해서든 실패를 뜻하는 값 반환하기
		- 프로그램 종료하기(terminate()나 exit(), abort() 같은 함수 호출)
	- 다음의 상황에서 오류 지시자("오류 코드")를 반환한다.
		- 실패가 정상이고 예상된다. 예를 들어 파일 열기 요청의 실패는 정상적으로 일어날 수 있다.
		- 중간 호출자가 분명 실패를 처리할 것이다.
		- 병렬 작업 중 하나에서 오류가 발생하면 어떤 작업에서 실패했는지 알아야 한다.
		- 시스템 메모리가 거의 고갈돼 필수 기능보다 런타임 예외 지원이 우선시된다.
	- 다음의 상황에서 예외를 던진다.
		- 오류가 거의 발생하지 않아 프로그래머가 검사를 잊기 쉽다. 가령 printf()의 반환값을 언제 마지막으로 확인했는지 기억하는가?
		- 중간 호출자가 오류를 처리할 수 없다. 오류를 호출 사슬 위 "궁극적 호출자"에게 다시 전달해야 한다. 예를 들어 애플리케이션 내 모든 함수가 모든 할당 실패와 네트워크 단절을 확실히 처리하기란 불가능하다. 오류 코드를 반복적으로 검사하면 장황해지고 비용도 크며 오류가 발생하기 쉽다. 오류 테스트와 오류 코드를 반환값으로 전달하면 함수의 핵심 로직을 이해하기 어려워진다.
		- 새로운 종류의 오류를 애플리케이션의 저수준 모듈에 추가하면 고수준 모듈에 이러한 오류를 처리하는 코드를 작성하지 않아도 된다. 예를 들면 단일 스레드였던 애플리케이션을 멀티 스레드를 사용하도록 수정하거나 네트워크를 통해 원격으로 자원에 접근해야 할 때이다.
		- 오류 코드를 반환할 적절한 경로가 없다. 예를 들어 생성자에 "호출자"가 검사할 반환값이 없다. 연산자에도 분명한 오류 코드 반환 경로가 없다. 예를 들어 a*b+c/d
		- 값과 오류 지시자를 둘 다 반환해야 하면 함수의 반환 경로가 복잡해지거나 비용이 더 커지므로 출력 매개변수, 비지역 오류 상태 지시자, 그 외의 다른 해결책을 사용해야 한다.
		- 오류 복구는 여러 함수 호출의 결과에 따라 달라지며, 호출과 복잡한 제어 구조 간 지역 상태를 유지해야 한다.
		- 오류를 찾은 함수는 콜백이므로 중간 호출자는 어떤 함수가 호출됐는지조차 모를 수 있다.
		- 오류는 일종의 "되돌리기 동작"이 필요함을 암시한다.
	- 다음의 상황에서 종료시킨다.
		- 복구할 수 없는 유형의 오류일 때. 예를 들어 전부는 아니지만 많은 시스템에서 메모리 고갈을 복구할 적당한 방법이 없다.
		- 분명하지 않은 오류가 감지될 때마다 스레드나 프로세스, 컴퓨터 재시작에 기반해 오류를 처리하는 시스템일 때
	- 확실히 종료시키는 한가지 방법은 함수에 noexcept를 추가해 함수 구현 내 어디에서든 throw가 terminate()로 바뀌도록 하는 것이다. 범용 라이브러리는 무조건적으로 종료돼서는 안 된다.
- 어설션
	- assert()
		- 표준 라이브러리는 디버그 매크로인 assert를 제공해 런타임에 참이어야 하는 조건을 표명한다.
		```cpp
		void f(const char* p)
		{
			assert(p!=nullptr); // p는 nullptr이어서는 안 된다.
		}
		```
		- "디버그 모드"에서 assert()의 조건이 실패하면 프로그램은 종료된다. 디버그 모드가 아니면 assert()는 검사되지 않는다. 조잡하고 유연하지 못하나 안하는 것보다는 낫다.
	- 정적 어설션
		- 예외는 런타임에 찾은 오류를 보고한다. 컴파일 타임에 찾을 수 있는 오류라면 미리 찾는 편이 좋다. 그래서 사용자 정의 타입의 인터페이스를 명시하는 타입 시스템과 기능이 존재하는 것이다.
		```cpp
		static_assert(4<=sizeof(int), "integers are too small"); // 정수 크기 검사
		```
		- 위 코드는 4<=sizefof(int)가 참이 아니면, 즉 이 시스템에서 int가 최소 4바이트를 갖지 않으면 integers are too small이라고 출력한다. 이러한 기대문을 어설션이라고 부른다.
		- static_assert 메커니즘은 상수 표현식으로 표현할 수 있는 무엇에든 사용할 수 있다.
		- static_assert(A, S)는 A가 참이 아니면 S를 출력하는데, 메시지를 출력하고 싶지 않으면 S를 빼고 컴파일러가 기본 메시지를 제공하게 둔다.
	- noexcept
		- 예외를 던지면 안 되는 함수는 noexcept로 선언한다.
		- 좋은 의도와 계획이 모두 실패해 예외를 던지면 std::terminate()가 호출돼 프로그램이 즉시 종료된다.
		- noexcept를 남용하면 위험하다. noexcept 함수가 호출한 함수가 사실은 예외를 잡아서 처리해주길 기대하며 예외를 던지는 함수였는데, noexcept는 치명적 오류로 바꿔버린다. 또한 noexcept는 작성자가 복잡하고 율가 발생하기 쉽고 비용이 클 수 있는 형태의 오류 코드로 오류를 처리하게 유도한다.

## 5장 클래스
### 구체 타입
- 구체 클래스(concrete class)의 기본 개념은 "내장 타입과 완전히 똑같이" 동작하는 것이다.
- 예를 들어 복소수 타입과 무한 정밀도 정수는 그 타입만의 시맨틱과 연산 집합이 있으나 내장 int와 매우 비슷하고, vector와 string은 내장 배열과 매우 비슷하지만 더 유연하고 더 매끄럽다.
- 구체 타입의 특징을 정의한다는 것은 표현을 정의에 넣는다는 뜻이다. vector 등 많은 중요한 타입에서 표현은 어딘가 저장된 데이터로의 하나 이상의 포인터일 뿐이지만 그 표현은 구체 클래스의 각 객체 내에 존재한다. 구체적으로 다음을 할 수 있다.
	- 구체 타입의 객체를 스택, 정적으로 할당된 메모리, 다른 객체 내에 둘 수 있다.
	- 객체를 (포인터나 참조를 통해서만이 아니라) 직접 참조할 수 있다.
	- 객체를 즉시 그리고 완전하게 초기화할 수 있다.
	- 객체를 복사하고 이동할 수 있다.
- 표현은 프라이빗을 수 있고 멤버 함수를 통해서만 접근할 수 있으나 분명 존재한다. 따라서 표현이 눈에 띄게 바뀌면 사용자는 다시 컴파일해야 한다. 구체 타입을 완전히 내장 타입처럼 동작시키는 대가이다.
#### 산술 타입
- "전형적인 사용자 정의 산술 타입"이 바로 complex이다.
```cpp
class complex {
	double re, im;	// 표현: double 2개
public:
	// ...
	complex& operator+=(complex z)
	{
		re+=z.re;		// re와 im을 더한다
		im+=z.im;
		return *this;	// 결과를 반환한다
	}

	complex& operator-=(complex z)
	{
		re-=z.re;
		im-=z.im;
		return *this;
	}
	//...
}
```
- 위 코드는 표준 라이브러리 complex를 단순화한 버전이다. 클래스 정의 자체는 표현에 접근하는 데 필요한 연산만 포함한다. 표현은 간단하고 매우 평범하다.
#### 컨테이너
- 컨테이너(container)는 원소 묶음을 담아두는 객체다. Vector 타입의 객체가 컨테이너이므로 Vector 클래스를 컨테이너라 부른다.
- Vector 는 치명적 결함이 있다. 원소를 new로 할당하고는 절대 해제하지 않는다. C++는 가비지 컬렉터를 제공하지 않으므로 매우 치명적이다.
- 생성자가 할당한 메모리를 반드시 해제할 메커니즘이 필요하다. 이러한 메커니즘이 바로 소멸자이다.
- 생성자는 원소를 할당하고 Vector의 멤버를 적절히 초기화한다. 소멸자는 원소를 해제한다. 이 기법을 자원획득은 초기화(Resource Acquisition Is Initialization), RAII라고 부른다.
#### 컨테이너 초기화
- 원소를 컨테이너에 편하게 넣을 방법이 필요하다.
	- 초기화 리스트 생성자(initializer-list constructor): 원소 리스트로 초기화한다.
	- push_back(): 새 원소를 시퀀스 끝에 추가한다.
- 두방법은 다음과 같이 선언한다.
```cpp
class Vector {
public:
	Vector();	// 기본적으로 "빈 상태"로 초기화한다. 즉, 어떤 원소로도 초기화하지 않는다
	Vector(std::initializer_list<double>);	// double 리스트로 초기화한다
	// ...
	void push_back(double);	// 크기를 1씩 증가시키며 끝에 원소를 추가
}
```
- 초기자 리스트 생성자에 쓰인 std::initializer_list는 컴파일러가 아는 표준 라이브러리 타입이다. 다음과 같이 사용할 수 있다.
```cpp
Vector v1 = {1, 2, 3, 4, 5};
Vector v2 = {1.23, 3.45, 6.7, 8};

// 정의는 다음과 같이 할 수 있다.
Vector::Vector(std::initializer_list<double> lst) : elem{new double[lst.size()]}, sz{static_cast<int>(lst.size())}
{
	copy(lst.begin(), lst.end(), elem);	// lst에서 elem으로 복사한다.
}
```
### 추상 타입
- Complex와 Vector처럼 정의에 표현을 포함하는 타입을 구체 타입(concrete type)이라 부른다. 이러한 면에서 내장 타입과 비슷하다.
- 대조적으로 추상 타입(abstract type)은 세부 구현을 사용자에게 완전히 감춘다. 즉, 표현에서 인터페이스를 분리하고 지역변수를 사용하지 않는다.
- 사용자는 추상타입의 표현을(심지어 그 크기조차) 전혀 모르므로 객체를 자유 저장소에 할당한 후 참조나 포인터로 접근해야 한다.
```cpp
class Container {
public:
	virtual double& operator[](int) = 0;	// 순수 가상 함수
	virtual int size() const = 0;			// const 멤버 함수
	virtual ~Container() {}
}
```
- 순수 가상 함수로 이뤄진 클래스를 추상 클래스라 부른다.
- operator[], size() 등 오버라이딩을 할 때는 명시적으로 override 키워드를 사용하자

### 가상 함수
- 어떻게 알맞는 가상 함수를 찾을까? 일반적으로는 컴파일러가 가상 함수의 이름을 함수 포인터로 이뤄진 테이블의 인덱스로 변환하도록 구현된다. 이러한 테이블을 가상 함수 테이블(virtual function table) 또는 줄여서 vtbl이라 부른다.
- 호출자는 객체의 크기와 데이터 레이아웃을 몰라도 vtbl 내 함수를 통해 객체를 올바르게 사용할 수 있다. 호출자의 구현에서는 Container의 vtbl 포인터 위치와 각 가상 함수의 인덱스만 알면 된다.
- 이러한 가상 호출 메커니즘은 "일반적인 함수 호출" 메커니즘과 효율성이 거의 비슷하다(25% 이내이며 같은 객체를 반복 호출할 때는 훨씬 저렴하다). 공간 면에서는 가상 함수를 포함하는 각 클래스마다 vtbl 하나 그리고 그 클래스의 각 객체마다 포인터 하나의 오버헤드가 든다.

### 클래스 계층 구조
- 클래스 계층 구조에는 두가지 이점이 있다.
	- 인터페이스 상속: 파생된 클래스의 객체를 기반 클래스의 객체가 필요한 곳에 사용할 수 있다. 즉, 기반 클래스는 파생된 클래스의 인터페이스처럼 동작한다. 이러한 클래스는 대개 추상 클래스이다.
	- 구현 상속: 기반 클래스는 파생된 클래스의 구현을 간소화해주는 함수나 데이터를 제공한다. 이러한 기반 클래스는 대개 데이터 멤버와 생성자이다.
- 클래스 계층 구조 탐색이 불가피하면 dynamic_cast를 사용한다.
	- 필요한 클래스를 찾는 데 실패해서는 안되면 참조 타입에 dynamic_cast를 사용한다.
	- 필요한 클래스를 찾는데 실패해도 유효하면 포인터 타입에 dynamic_cast를 사용한다.
- new를 사용해 생성된 객체를 반드시 delete하려면 unique+ptr이나 shared_ptr을 사용한다.

## 6장 필수 연산
- 타입의 생성자, 소멸자, 복사와 이동 연산은 논리적으로 별개가 아니다. 서로 연관된 집합으로 정의하지 않으면 논리적 문제나 성능 문제를 겪게 된다.
- 다음의 다섯 가지 경우에 객체를 복사하거나 이동시킨다.
	- 할당의 소스로서
	- 객체 초기자로서
	- 함수 인수로서
	- 함수 반환값으로서
	- 예외로서
- 할당은 복사나 이동 할당 연산자를 이용한다. 원친적으로 나머지 경우에는 복사나 이동 생성자를 이용한다. 하지만 일반적으로는 타깃 객체에서 바로 초기화하는 객체를 생성하도록 최적화해서 복사나 이동 생성자 호출을 제거한다.
	- 예제
	```cpp
	X Make(Sometype);
	X x = make(value);
	```
	- 일반적으로 컴파일러는 x 내 make()로부터 바로 X를 생성해 복사를 제거(생략)한다.
- "일반적인 생성자"를 제외하고 이러한 특수 멤버 함수는 필요 시에 컴파일러가 생성한다. 기본 구현 생성을 다음과 같이 명시적으로 할 수도 있다.
	```cpp
	class Y{
	public:
		Y(Sometype);
		Y(const Y&) = default;	// 기본 복사 생성자와
		Y(Y&&) = default;		// 기본 이동 생성자가 정말 필요하다
	}
	```
	- 몇가지를 기본으로 명시하면 다른 기본 정의는 생성되지 않는다.
- 일반적으로 클래스가 포인터 멤버를 포함할 때는 보갓와 이동 연산을 명시적으로 하는 편이 좋다. 클래스가 delete해야 할 무언가를 포인터가 가리킬 수 있는데, 기본 멤버 단위 복사가 틀릴 수도 있기 때문이다. 혹은 클래스가 delete하지 말아야 할 무언가를 가리킬 수도 있다.
- 경험상 좋은 방법은 필수 연산을 전부 정의하거나 하나도 정의하지 않는 것이다(모든 연산에 default 사용).
	```cpp
	struct Z{
		Vector v;
		string s;
	}
	Z z1;		// z1.v와 z1.s를 기본 초기화한다.
	Z z2 = z1;	// z1.v와 z1.s를 기본 복사한다.
	```
	- 위 코드에서 컴파일러는 필요 시에 올바른 시맨틱으로 멤버 단위의 기본 생성, 복사, 이동을 만들어낸다.
- =default를 보완하기 위해 연산을 생성하지 말라는 의미로 =delete를 사용한다.
	```cpp
	class Shape{
	public:
		Shape(const Shape&) =delete;	// 복사 금지
		Shape& operator=(const Shape&) =delte;
		//...
	}

	void copy(Shape& s1, const Shape& s2)
	{
		s1 = s2;	// 오류: Shape 복사는 금지됐다
	}
	```
	- =delete로 선언된 함수를 사용하려 하면 컴파일 타임 오류가 발생한다. =delete는 기초 멤버 함수뿐만 아니라 모든 함수를 막는 데 사용할 수 있다.
- 변환
	- 인수를 하나만 받는 생성자는 그 인수 타입으로부터의 변환을 정의한다. 예를들어 complex는 double로부터의 생성자를 제공한다.
	```cpp
	complex z1 = 3.14;	// z1은 {3.14, 0.0}이 된다.
	complex z2 = z1*2;	// z2는 z1 * {2.0, 0} == {6.28, 0.0}이 된다.
	```
	- 이러한 암묵적 변환이 이상적일 때도 있으나 늘 그렇진 않다. 예를 들어 Vector는 int로부터의 생성자를 제공한다.
	```cpp
	Vector v1 = 7;	// OK: v1의 원소는 7개다.
	```
	- 일반적으로 위 코드는 적절하지 않으므로 표준 라이브러리 vector는 int에서 vector로의 "변환"을 허용하지 않는다.
	- 문제를 방지하려면 명시적 "변환"만 허용한다고 선언하면 된다. 즉 생성자를 다음과 같이 정의한다.
	```cpp
	class Vector{
	public:
		explicit Vector(int s);	// int에서 Vector로 암묵적 변환되지 않는다
		// ...
	}
	// 이제 다음과 같이 바뀐다.
	Vector v1(7);	// OK: v1의 원소는 7개다.
	Vector v2 = 7;	// 오류: int에서 Vector로 암묵적 변환되지 않는다.
	```
	- 다른 분명한 이유가 있지 않은 한 인수 하나를 받는 생성자에는 explicit을 사용하자.
- 멤버 초기자
	- 클래스의 데이터 멤버를 정의할 때 기본 멤버 초기자(default member initializer)라는 기본 초기자를 제공할 수 있다. 다음은 complex를 변경한 코드다(5.2.1절).
	```cpp
	class complex{
		double re = 0;
		double im = 0;	// 표현: 기본값이 0.0인 double 2개
	public:
		complex(double r, double i) :re{r}, im{i} {}	// {r, i}
		complex(double r) : re{r} {}					// {r, 0}
		complex() {}									// {0, 0}
	}
	```
	- 생성자가 값을 제공하지 않으면 항상 기본값이 쓰인다. 코드를 간소화해주며 실수로 멤버를 초기화하지 않는 경우에 대비해준다.

### 복사와 이동
- 컨테이너 복사
	- 클래스가 자원 핸들(resource handle)인 경우, 즉 클래스가 포인터로 접근하는 객체를 처리하는 경우 기본 멤버 단위 복사는 사실상 재앙에 가깝다. 멤버 단위 복사가 자원 핸들의 불변을 위반하기 때문이다. 예를 들어 기본 복사는 원래의 원소를 똑같이 참조하는 Vector의 복사본을 생성한다.
	- 클래스의 객체 복사는 복사 생성자(copy constructor)와 복사 할당(copy assignment)이라는 두 멤버로 정의된다.
	```cpp
	class Vector{
		// ...
		Vector(const Vector& a);			// 복사 생성자
		Vector& operator=(const Vector& a);	// 복사 할당
		// ...
	}
	```
	- 깊은 복사가 필요하다
- 컨테이너 이동
	- 컨테이너가 크면 복사 비용도 커진다. 참조를 사용하면 복사 비용이 줄어드나 지역 객체의 참조를 결과로 반환할 수는 없다.
	```cpp
	Vector operator+(const Vector& a, const Vector& b)
	{
		Vector res(a.size());

		for (int i = 0; i != a.size(); ++i)
			res[i] = a[i] + b[i];
		return res;
	}
	```
	- +로부터 반환한다는 것은 지역변수 res 결과를 갖고 나와 호출자가 접근할 수 있는 어딘가로 복사한다는 의미이다. +는 다음과 같이 쓰인다.
	```cpp
	void f(const Vector& x, const Vector& y, const Vector& z)
	{
		Vector r;
		// ...
		r = x + y + z;
		// ...
	}
	```
	- 예제에서는 Vector를 최소 2번 복사한다(+를 사용할 때마다 1번씩). Vector가 크면 상당히 곤란하다.
	- 가장 당혹스러운 부분은 operator+()내 res를 복사하고는 다시 사용하지 않는다는 점이다. 복사를 원한 적도 없고 결과를 함수 밖으로 가져오고 싶었을 뿐이다. 즉, Vector를 복사하는 것이 아니라 이동시키고 싶었다. 다행히 이러한 의도를 명시할 수 있다.
	```cpp
	class Vector{
		//...
		Vector(const Vector& a);			// 복사 생성자
		Vector& operator=(const Vector& a);	// 복사 할당

		Vector(Vector&& a);					// 이동 생성자
		Vector& operator=(Vector&& a);		// 이동 할당
	}
	```
	- 위와 같이 정의하면 컴파일러는 이동 생성자를 골라 함수로부터의 반환값 전달을 구현한다. 즉, r=x+y+z는 Vector를 전혀 복사하지 않는다.
	- &&는 "rvalue참조"라는 의미로 rvalue에 바인딩할 수 있는 것을 참조한다. "rvalue"는 "lvalue"를 보완하는 용어로서 "lvalue"는 "할당문 왼편에 넣을 수 있는 것" 정도로 이해하면 된다.
	- 즉, rvalue 참조는 누구도 할당할 수 없는 것을 참조하므로 안전하게 그 값을 "가져올" 수 있다. Vector의 operator+() 내 res 지역변수가 그 예이다.
	- 이동 생성자는 const 인수를 받지 못한다. 다시 말해 이동 생성자는 인수에서 값을 제거하는 용도이다. 이동 할당(move assignment)도 비슷하게 정의한다.
	- 이동 후 값이 옮겨진 객체는 소멸자를 실행시킬 수 있는 상태여야 한다. 또한 일반적으로 옮겨진 객체로의 할당도 허용한다.

### 전통적 연산
- 비교: ==, !=, <, <=, >, >=, <=>
	- 우주선 연산자 <=>
		```cpp
		bool b1 = (r1<=>r2) == 0;	// r1 == r2
		bool b2 = (r1<=>r2) < 0;	// r1 < r2
		bool b3 = (r1<=>r2) > 0;	// r1 > r2
		```
		- C의 strcmp() 처럼 <=>는 3자간 비교를 구현한다. 음수 반환값은 "~보다 적다", 0은 "같다", 양숫값은 "~보다 크다"는 뜻이다.
		- <=>를 non-default로 정의하면 ==는 암묵적으로 정의되지 않지만 <와 나머지 관계 연산자는 정의된다!
		```cpp
		struct R2{
			int m;
			auto operator<=>(const R2& a) const { return a.m == m ? 0 : a.m < m ? -1 : 1; }
		};

		void user(R2 r1, R2 r2)
		{
			bool b4 = (r1 == r2);	// 오류: non-default인 ==가 없다
			bool b5 = (r1<r2);		// OK
		}
		```
		- 일반적이지 않은 타입에서는 다음과 같은 형태로 정의된다.
		```cpp
		auto operator<=>(const R3& a, const R3& b) { /*...*/ }
		bool operator==(const R3& a, const R3& b) { /*...*/ }
		```
		- string과 vector 같은 대부분의 표준 라이브러리 타입은 위 형태를 따른다. 타입에 비교할 원소가 둘 이상일 때 기본 <=>는 한 번에 하나씩 사전순으로 검사하기 때문이다. 이럴 때는 <=>가 세 가지 가능성을 밝히기 위해 모든 원소를 검사해야 하므로 별도의 최적화된 ==를 추가로 제공하면 좋다.

- 컨테이너 연산: size(), begin(), end()
- 반복자와 "스마트 포인터": ->, *, [], ++, --, +, -, +=, -=
- 함수 객체: ()
- 입력과 출력 연산: >>, <<
- swap()
- 해시 함수: hash<>
	- 표준 라이브러리 unordered_map\<K, V>는 K가 키 타입이고 V가 값 타입인 해시 테이블이다. 타입 X를 키로 사용하려면 hash\<X>를 정의해야 한다. sdt::string 같은 일반적인 타입은 표준 라이브러리에 이미 hash<>가 정의되어 있다.
### 사용자 정의 리터럴
- 내장 타입은 다음과 같은 리터럴을 지원한다.
	- 123은 int다.
	- 0xFF00u는 unsigned int다.
	- 123.456은 double이다.
	- "surprise!"는 const char[10]이다.
- 사용자 정의 타입도 이러한 리터럴을 지원하면 유용하다. 다음과 같이 리터럴에 적절한 접미사를 붙여 그 의미를 정의하면 된다.
	- "surprise!"s는 std::string이다.
	- 123s는 second다.
	- 12.7i은 12.7i+47이 complex수여야 하니 imaginary이다.
- 표준 라이브러리 예제
	- \<chrono> : std::literals::chrono_literals : h, min, s, ms, us, ns
	- \<string> : std::literals::string_literals : s
	- \<string_view> : std::literals::string_literals : sv
	- \<complex> : std::literals::complex_literals : i, il, if
- 사용자 정의 접미사가 붙은 리터럴을 사용자 정의 리터럴(User-Defined Literal) 또는 UDL이라 부른다. 이러한 리터럴은 리터럴 연산자(literal operator)를 사용해 정의된다. 리터럴 연산자는 인수 타입의 리터럴과 그 뒤에 오는 첨자를 반환 타입으로 변환한다. 예를 들어 imaginary의 접미사 i를 다음과 같이 구현한다.
	```cpp
	constexpr complex<double> operator""i(long double arg) { // imaginary 리터럴
		return {0, arg};
	}
	```
	- operator""는 리터럴 연산자를 정의하겠다는 뜻이다.
	- 리터럴 지시자(literal indicator)인 ""뒤에 나오는 i는 연산자가 의미를 부여하는 접미사다.
	- 인수 타입 long double은 접미사(i)를 부동소수점 리터럴에 정의하겠다는 뜻이다.
	- 반환 타입 complex\<double>은 결과 리터럴의 타입을 명시한다.
	- 이제 다음과 같이 작성할 수 있다.
	```cpp
	complex<double> z = 2.7182818+6.283185i;
	```
	- 접미사 i와 +의 구현은 둘 다 constexpr이므로 z의 값은 컴파일 타임에 계산된다.

## 7장 템플릿
- 제한된 템플릿 인수
	- 대부분의 경우 템플릿은 일정 기준에 부합하는 템플릿 인수만 허용한다. 예를 들어 Vector는 일반적으로 복사 연산을 제공하므로 원소가 복사 가능해야 한다. 즉, Vector의 인수는 단순히 typename이 아니라 Element여야 하며, "Element"는 원소가 될 수 있는 타입의 요구 사항을 명시해준다.
	```cpp
	template<Element T>
	class Vector{
	private:
		T* elem;	// elem은 타입 T의 원소 sz개로 된 배열을 가진다.
		int sz;
		// ...
	}
	```
	- 앞에 붙인 template\<Element T> 는 수학에서 말하는 "Element(T)인 모든 T에 대해"를 C++로 표현한 것이다. 즉, Elment는 T가 Vector에서 요구하는 특성을 만족하는지 검사하는 프레디킷이다. 이러한 프레디킷을 콘셉트(concept)라 부른다. 콘셉트를 명시한 템플릿 인수를 제한된 인수(constrained argument)라 부르며 인수가 제한된 템플릿을 제한된 템플릿(constrained template)이라 부른다.
	- 표준 라이브러리 원소의 타입 요구 사항은 약간 복잡하나 예제의 간단한 Vector에서 Element는 표준 라이브러리 콘셉트 copyable 정도일 것이다. 이 요구 사항에 부합하지 않는 타입의 템플릿을 사용하려 하면 컴파일 타임 오류가 발생한다. 예제로 보자
	```cpp
	Vector<int> v1;		// OK: int를 복사할 수 있다.
	Vector<thread> v2;	// 오류: 표준 스레드를 복사할 수 없다.
	```
	- 즉, 콘셉트에 기반해 컴파일러가 사용 시점에 타입 검사를 수행하므로 제한되지 않은 템플릿 인수를 사용할 때보다 훨씬 일찍 유용한 오류 메시지를 제공할 수 있다. C++ 20 이전에는 공식적으로 콘셉트를 지원하지 않았으므로 예전 코드는 제한되지 않은 템플릿 인수를 사용하는 대신 요구 사항을 설명서로 남겼다.
	- 콘셉트 검사는 전적으로 컴파일 타임 메커니즘이고 생성된 코드는 제한되지 않은 템플릿으로 만들어진 코드와 동일하다.
- 값 템플릿 인수
	- 템플릿은 타입 인수 외에 값도 인수로 받는다.
	```cpp
	template<typename T, int N>
	struct Buffer{
		constexpr int size() { return N; }
		T elem[N];
		// ...
	};
	```
- 템플릿 인수 추론
	- 인수가 많으면 템플릿 인수 타입을 일일이 명시하기 번거롭다. 다행히도 많은 경우에 pair의 생성자가 초기자로부터 템플릿 인수를 추론할 수 있다.
	```cpp
	pair<int, double> p = {1, 5.2};
	pair p = {1, 5.2};	// p는 pair<int, double>이다
	```
	- 하지만 추론도 혼란을 일으킬 수 있다.
	```cpp
	Vector<string> vs {"Hello", "World"};	// OK: Vector<string>
	Vector vs1 {"Hello", "World"};			// OK: Vector<const char*>으로 추론(의외인가?)
	Vector vs2 {"Hello"s, "World"s};		// OK: Vector<string>으로 추론
	Vector vs3 {"Hello"s, "World"};			// 오류: 초기자 리스트가 통일되지 않았다
	Vector<string> vs4 {"Hello"s, "World"};	// OK: 원소 타입을 명시했다
	```
### 매개변수화 연산
- 템플릿의 용도는 단순히 원소타입으로 컨테이너를 매개변수화하는데 그치지 않는다. 표준 라이브러리의 타입 및 알고리듬을 매개변수화하는 데 광범위하게 쓰인다.
- 다음 세 방법으로 타입이나 값으로 매개변수화한 연산을 표현한다.
	- 함수 템플릿
	- 함수 객체: 데이터를 운반하는 객체로서 함수처럼 호출한다.
	- 람다식: 함수 객체의 축약형
- finally
	- 소멸자는 더 이상 사용하지 않는 객체를 해제하는 일반적이고 암묵적인 메커니즘을 제공하지만 단일 객체이거나 (C 프로그램과 공유된 타입이라) 소멸자가 없는 객체와 관련되지 않으면 어떻게 해제해야 할까?
	- 범위가 종료될 때 필요한 동작을 실행하는 finally() 함수를 정의해 해결한다.
	```cpp
	void old_style(int n)
	{
		void* p = malloc(n*sizeof(int));	// C 방식
		auto act = finally([&]{free(p);});	// 범위 종료 시 람다를 호출한다.
	} // 범위 종료 시 p는 암묵적으로 해제된다
	```
- Attribute
	- [[nodiscard]] : C++17
		- 이 attribute가 붙은 함수나 attibute가 붙은 enum, class등을 리턴하는 함수를 만들었다면 해당 함수를 호출할때는 리턴값을 받아야 한다고 컴파일러에 알려주는 것이다. 이는 컴파일 시점에 워닝으로 알려준다.
		- [[nodiscard("string")]] - C++ 20은 워닝 메시지에 "string" 부분도 같이 남겨준다.
		```cpp
		[[nodiscard]] int strategic_value(int x, int y)
		{
			return x^y;
		}

		int main()
		{
		strategic_value();			// warning
		auto v = strategic_value();	// ok

		return 0;
		}
		```
	- [[deprecated]] / [[deprecated("string")]] - C++14
		- 해당 attribute가 붙은 함수를 사용하면 사용은 가능하지만 앞으로 제거될 수 있음을 워닝으로 알려준다. 더 이상 사용되지 않을 함수에게 붙인다.
		```cpp
		[[deprecated]]
		void TriassicPeriod() {
			std::clog << "Triassic Period: [251.9 - 208.5] million years ago.\n";
		}
		
		int main()
		{
			TriassicPeriod(); // warning
			return 0;
		}
		```
	- [[fallthrough]] - C++17
		- switch / case 문에서 break문을 중간에 넣지 않았을 경우 컴파일러가 워닝을 발생시킬 수도 있다. 이것이 의도적인 상황일 경우 [[fallthrough]]를 사용하여 워닝을 제거할 수 있다.
		```cpp
		int main()
		{
			int A = 0;
			
			switch(A)
			{
			case 0:
				A += 0;
				[[fallthrough]];
			case 1:
				A += 1;
				[[fallthrough]];
			default:
				A += 2;
				break;
			}

			return 0;
		}
		```
- 컴파일 타임 if
	- 일반성과 성능 최적화가 매우 중요한 기초 코드에서 slow_and_safe라는 일반적 연산 함수와 simple_and_fast 함수를 제공할 수 있다.
	```cpp
	template<typename T>
	void update(T& target)
	{
		// ...
		if constexpr(is_trivially_copyable_v<T>)
			simple_and_fast(target);	// "보통의 기존 데이터"에 쓰임
		else
			slow_and_safe(target);		// 좀 더 복잡한 타입에 쓰임
		// ...
	}
	```
	- is_trivially_copyable_v\<T>는 단순하게 타입을 복사할 수 있는지 알려주는 타입 프레디킷이다.

## 8장 콘셉트와 제네릭 프로그래밍
### 콘셉트
- sum()을 예제로 보자
	```cpp
	template<typename Seq, typename Value>
	Value sum(Seq& s, Value v)
	{
		for (const auto& x : s)
			v+=x;
		return v;
	}
	```
- 위 sum()의 요구 사항은 다음과 같다.
	- 첫 번째 템플릿 인수는 원소 시퀀스여야 하고,
	- 두 번째 템플릿 인수는 수여야 한다.
- 구체적으로 sum()은 다음의 인수 쌍으로 호출된다.
	- 범위 기반 for문이 동작하도록 begin()과 end()를 지원하는 시퀀스 Seq
	- 시퀀스 내 원소를 더할 수 있도록 +=를 지원하는 산술 타입 Value
- 위와 같은 요구 사항을 콘셉트라고 부른다.
- 위처럼 단순화시킨 시퀀스 요구 사항을 충족하는 타입 예가 표준 라이브러리의 vector, list, map이다. 산술 타입 요구사항을 충족하는 타입 예는 int, double, Matrix다.
- sum() 알고리듬은 원소(시퀀스)를 저장할 데이터 구조의 타입과 원소의 타입이라는 두 가지 차원에서 제네릭이라 말할 수 있다.

## 9장 라이브러리 훑어보기
### 표준 라이브러리 구성
- 표준 라이브러리의 모든 기능은 네임스페이스 std에 들어 있으며, 모듈이나 헤더 파일을 통해 사용자가 이용할 수 있다.
- 네임스페이스
	- 표준 라이브러리는 std에 몇 가지 하위 네임스페이스를 제공하며, 명시적 동작을 통해서만 접근할 수 있다.
		- std::chrono: std::literals::Chrono_literals를 포함한 chrono의 모든 기능
		- std::literals::chrono_literals: 연도에는 접미사 y, 날짜에는 d, 시간에는 h, 분에는 min, 밀리초에는 ms, 나노초에는 ns, 초에는 s, 마이크로초에는 us
		- std::literals::complex_literals: double 허수에는 접미사 i, float 허수에는 if, long double 허수에는 il
		- std::literals::string_literals: 문자열에는 접미사 s
		- std::literals::string_view_literals: 문자열 뷰에는 접미사 sv
		- 수학 상수에는 std::numbers
		- 다형 메모리 자원에는 std::pmr
	- 하위 네임스페이스의 접미사를 사용하려면 어떤 네임스페이스에 들어 있는지 알려야 한다.
		```cpp
		auto z1 = 2+3i;	// 오류: 'i'라는 접미사가 없음

		using namespace literals::complex_literals;	// 복소수 리터럴임을 보였다
		auto z2 = 2+3i;	// ok: z2는 complex<double>이다
		```
	- ranges 네임스페이스
		- 표준 라이브러리는 sort()와 copy()같은 알고리즘을 두가지 버전으로 제공한다.
			- 반복자 쌍을 받는 전통적인 시퀀스 버전. 예를 들어 sort(begin(v), v.end());
			- 범위 하나를 받는 범위 버전. 예를 들어 sort(v);
		- 이상적으로는 별다른 구분 없이 위 두 버전을 완전히 오버로딩할 수 있어야 하지만 그렇지 못하다.
		```cpp
		using namespace std;
		using namespace ranges;

		void f(vector<int>& v)
		{
			sort(v.begin(), v.end());	// 오류: 모호함
			sort(v);					// 오류: 모호함
		}
		```
		- 기존의 제한되지 않은 템플릿을 사용하는 경우 표준에서는 명시적으로 표준 라이브러리 알고리듬의 범위 버전을 스코프 안으로 명시적으로 넣게 한다.
		```cpp
		using namespace std;

		void g(vector<int>& v)
		{
			sort(v.begin(), v.end());		// OK
			sort(v);						// 오류: (std 안에) 일치하는 함수가 없음
			ranges::sort(v);				// OK
			using ranges::sort;				// 여기서부터는 sortv(v) OK
			sort(v);						// OK
		}
		```
	- 모듈
		- 표준 라이브러리 모듈은 현재 존재하지 않는다. C++23에서 누락된 이 부분을 개선할 가능성이 높다. 우선은 namespace std의 모든 기능을 제공하면서 표준이 될 가능성이 높은 module std를 사용하겠다.

## 10장 문자열과 정규식
### 문자열
- string 구현
	- 근래에는 string을 대개 짧은 문자열 최적화(short-string optimization)로 구현한다. 즉 짧은 문자열 값은 string 객체 자체에 보관하고, 긴 문자열은 자유 저장소로 보낸다.
	- "짧은" 문자열이란 구현에서 정의하기 나름이지만 "대략 14개" 정도이다.
	- string의 실제 성능은 런타임 환경에 따라 크게 좌우된다. 특히 다중 스레드 구현에서는 메모리 할당 비용이 상대적으로 크다. 길이가 다른 문자열을 여러 개 사용할 경우 메모리 단편화가 발생하기도 한다. 그래서 대부분의 구현에 짧은 문자열 최적화를 사용한다.
### 문자열 뷰
- 문자 시퀀스는 일반적으로 어떤 함수에 전달돼 읽힌다. string을 값으로 전달하거나 문자열로의 참조로 전달하거나 C 스타일 문자열로 전달한다. 표준에서 제공하지 않는 문자열 타입 등의 다른 형태로 전달하는 시스템도 많다. 이를 해결하기 위해 표준 라이브러리는 string_view를 제공한다. string_view는 간단히 말해 문자 시퀀스를 나타내는 (포인터, 길이) 쌍이다.
- 가리키는 문자들을 소유하지 않는다는 점에서 string_veiw는 포인터나 참조와 비슷하다.
```cpp
string cat(string_view sv1, string_view sv2)
{
	string res {sv1};	// sv1로 초기화한다.
	return res += sv2;	// sv2를 이어 붙여서 반환한다.
}
```
- 이제 cat()을 호출해보자
```cpp
string king = "Harold";
auto s1 = cat(king, "William");				// HaroldWilliam: string과 const char*
auto s2 = cat(king, king);					// HaroldHarold: string과 string
auto s3 = cat("Edward", "Stephen"sv);		// EdwardStephen: const char*와 string_view
auto s4 = cat("Canute"sv, king);			// CanuteHarold
auto s5 = cat({&king[0],2}, "Henry"sv);		// HaHenry
auto s6 = cat({&king[0],2}, {&king[2],4});	// Harold
```
- 위 cat()은 const String&를 인수로 받는 함수보다 세 가지 면에서 낫다.
	- 여러 다양한 방법으로 처리되는 문자 시퀀스에 사용할 수 있다.
	- 부분 문자열을 전달하기 쉽다.
	- C 스타일 문자열 인수를 전달할 때 string을 생성하지 않아도 된다.
- 위 코드에서 sv라는 접미사를 사용했다. 이렇게 하려면 선언부터 해야 한다.
```cpp
using namespace std::literals::string_view_literals;
```
- 왜 접미사를 붙일까? "Edward"를 전달할 때 const char*로부터 string_view를 생성해야 하는데 이때 문자수를 세어야 하기 때문이다. "Stephen"sv의 길이는 컴파일 타임에 계산된다.
- string_view는 범위를 정의하므로 뷰 내 문자를 순회할 수 있다.
```cpp
void print_loswer(string_view sv1)
{
	for (char ch : sv1)
		cout << toloswer(ch);
}
```
- 다만 string_view에는 문자들의 읽기 전용 뷰라는 중대한 제약이 따른다. 예를 들어 인수를 소문자로 수정하는 함수에는 문자를 string_view로 전달할 수 없다. 대신 span을 사용하자. string_view는 일종의 포인터라서 사용하려면 무언가를 가리켜야 한다.
```cpp
string_view bad()
{
	string s = "Once upon a time";
	return {&s[5], 4};	// 오류: 지역변수를 가리키는 포인터를 반환한다.
}
```

### 정규식
- 표준 라이브러리는 \<regex\>에서 다음과같이 정규식을 지원한다.
	- regex_match(): 정규식을 문자열과 대조해본다.
	- regex_search(): 데이터 스트림에서 정규식에 부합하는 문자열을 찾는다.
	- regex_replace(): 데이터 스트림에서 정규식에 부합하는 문자열들을 찾아 치환한다.
	- regex_iterator: 부합하는 부분과 부분 부합하는 부분을 순회한다.
	- regex_token_iterator: 부합하지 않는 부분을 순회한다.
- 검색
	- 패턴을 사용하는 가장 간단한 방법은 스트림 검색이다.