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
	- variant는 여러 타입 집합 중 하나의 값을 저장한다. 예를 들어 variant<Node*, int>에 Node*나 int를 저장할 수 있다.
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