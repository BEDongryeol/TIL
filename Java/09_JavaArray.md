# Array 구현
- jdk 클래스 : ArrayList, Vector
## 구현 함수
- 배열은 크기를 미리 정해주어야 한다.
> - ARRAY_SIZE = 배열의 크기 
> - count = 배열 내 요소 개수
> - addElement = 요소 추가 함수
> - insertElement = 요소 삽입 함수
> - removeElement = 요소 제거 함수
> - getSize = 배열의 크기
> - getElement = 검색 함수
> - printAll = 모든 요소 출력
> - removeAll = 모든 요소 삭제

<details>
<summary>예제코드 확인하기</summary>

```java
public class Test {
    int[] intArr;
    int count;

    public int ARRAY_SIZE;
    public static final int ERROR_NUM = -999999999;

    public Test() {
        count = 0;
        ARRAY_SIZE = 10;
        intArr = new int[ARRAY_SIZE];
    }

    public Test(int size) {
        count = 0;
        ARRAY_SIZE = size;
        intArr = new int[ARRAY_SIZE];
    }

    public void addElement(int num) {
        if (count >= ARRAY_SIZE) {
            System.out.println("Not enough Memory");
            return;
        }
        intArr[count++] = num;
    }

    public void insertElement(int index, int num) {

        int i = count - 1;

        if (count >= ARRAY_SIZE) {
            System.out.println("Not enough Memory");
            return;
        }
        if (index < 0 || index > count) {
            System.out.println("Insert Error");
            return;
        }

        while (i > index) {
            intArr[i + 1] = intArr[i];
            i--;
        }

        intArr[index] = num;
        count++;
    }

    public int removeElement(int index) {
        int ret = ERROR_NUM;

        if (isEmpty()) {
            System.out.println("There is no element");
            return ret;
        }

        if (index < 0 || index >= count){
            System.out.println("index Error");
            return ret;
        }
        ret = intArr[index];

        for (int i = index; i<count-1; i++){
            intArr[i] = intArr[i+1];
        }
        count--;
        return ret;
    }

    private boolean isEmpty() {
        if (count == 0) {
            return true;
        }
        else return false;
    }

    public int getSize(){
        return count;
    }

    public int getElement(int index){
        if (index < 0 || index > count-1){
            System.out.println("검색 위치 오류입니다. 현재 리스트의 개수는 " + count + "입니다.");
            return ERROR_NUM;
        }
        return intArr[index];
    }

    public void printAll(){
        if (count == 0) {
            System.out.println("출력할 값이 없습니다.");
            return;
        }

        for (int i = 0 ; i < count; i++){
            System.out.println(intArr[i]);
        }
    }

    public void removeAll(){
        for (int i = 0; i < count; i++){
            intArr[i] = 0;
        }
        count = 0;
    }
}

```

