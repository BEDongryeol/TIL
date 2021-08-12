# 다형성
- 하나의 코드가 여러 자료형으로 구현되어 실행 되는 것
- 같은 코드에서 여러가지 실행 결과가 나오는 것
> 유연하고 확장성있고, 유지보수가 편리함 프로그램을 만들 수 있다.

## 개념
- 하위 클래스들을 상위 클래스로 형변환한다.
- 이를 한번에 핸들링한다.
- 각 인스턴스별로 다른 실행 결과를 얻을 수 있다.
- 결합도가 높아진다.

## 예시
<details>
<summary> 모든 코드 확인하기 </summary>

```
package polymorphismTest;


import java.util.ArrayList;


class Animal{
	// 공통된 기능을 정의 할 때 상위 클래스인 Animal에 기능을 입력한다.
	public void move() {
		System.out.println("동물이 움직입니다.");
	}
	
}

class Human extends Animal {

	@Override
	public void move() {
		System.out.println("사람이 두 발로 걷습니다.");
	}
	public void readBook() {
		System.out.println("사람이 책을 읽습니다.");
	}
}

class Tiger extends Animal {

	@Override
	public void move() {
		System.out.println("호랑이가 네 발로 뜁니다.");
	}
	
	public void hunting() {
		System.out.println("호랑이가 사냥을 합니다.");
	}
}

class Eagle extends Animal {
	
	@Override
	public void move() {
		System.out.println("독수리가 하늘을 납니다.");
	}
	
	public void flying() {
		System.out.println("독수리가 양날개를 쭉 펴고 날아다닙니다.");
	}
}


public class AnimalTest {
	 
	public static void main (String[] args) {
		// 형변환 (업캐스팅 : 상위 클래스의 타입으로 변환)
		Animal hAnimal = new Human();
		Animal tAnimal = new Tiger();
		Animal eAnimal = new Eagle();
		
		AnimalTest test = new AnimalTest();
		// 상위 클래스인 Animal로 형변환이 가능하다.
		// 각자의 가상 메서드 테이블을 갖게 된다.
		test.moveAnimal(hAnimal);
		test.moveAnimal(tAnimal);
		test.moveAnimal(eAnimal);
		
		// 세 동물을 ArrayList에 삽입하고 싶을 때
		ArrayList<Animal> animalList = new ArrayList<>();
		animalList.add(hAnimal);
		animalList.add(tAnimal);
		animalList.add(eAnimal);
		// enhanced for문을 통한 출력
		for (Animal a : animalList) {
			a.move();
		}
		
		
	}
	public void moveAnimal(Animal animal) {
		animal.move();
	}
}
```

</details>

## 사용하는 이유
-  상속과 메서드의 재정의를 활용하여 확장성 있는 프로그램을 만들 수 있다.
 - 위 예시로 보았을 때 동물을 추가하고 싶을 때 편하게 추가할 수 있다.
- 상위 클래스에 공통된 기능과 하위 클래스에 특정한 기능을 구분하여 코딩할 수 있다.

