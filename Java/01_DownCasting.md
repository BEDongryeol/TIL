# 다운캐스팅 (DownCasting)

## 의미
- 업캐스팅된 클래스를 원래의 타입으로 다시 형변환하는 것
- 하위 클래스로의 형변환(다운캐스팅)은 명시적으로 해야한다.
- 클래스B가 클래스A에게 상속받는 경우
```
A instance = new B(); // 묵시적
B instanceB = (B)instance; //명시적
```

## 유의 사항
- 클래스 B와 C가 클래스 A에게 상속 받는 경우
```
// A의 타입으로 B, C의 인스턴스를 생성해준다.
A ins1 = new B();
A ins2 = new C();

// C타입을 B타입인 ins3로 생성을 시도한다.
// 실행전까지는 에러가 나지 않지만 실행 시 에러가 발생한다.
B ins3 = (B)ins2;
```
- 아래와 같은 방어 코드로 에러를 방지할 수 있다.
- ```instanceof``` : in2가 B클래스로 생성된 인스턴스인지 확인한다
```
if ( in2 instanceof B ){
B ins3 = (B)ins2;
}
```

