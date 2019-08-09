# Array Object

## Array.from()
- 새로운 Array 오브젝트 생성
- parameter
  - object: Array로 변환할 대상을 지정 (Array-like 또는 이터러블 오브젝트를 기재해야 함)
  - Function: 배열 엘리먼트를 읽을 때마다 호출할 함수 (선택사항)
  - Object: Function에서 this로 참조할 오브젝트를 지정 (선택사항)
  ```
  let arrayLike = {0: 10, 1: 30, length: 2};
  let values = Array.from(arrayLike, funciton(vlaue) {
    return value + this.bonus;
  }, {bonus: 100});
  console.log(values); // [100, 130]
  ```
## Array.of()
- 파라미터 값을 새로운 배열의 엘리먼트로 설정하여 반환
- parameter
  - 모든타입 가능, 배열 엘리먼트 값, 다수 지정 가능
- 반환: 생성한 Array 인스턴스
- Array.from()와의 차이점: Array.from() -> 파라미터에 Array-like or 이터러블 오브젝트 지정
                        Array.of() -> 파라미터에 값을 지정
- 호출 시, Array 오브젝트 생성 -> 파라미터에 작성한 순서대로 배열에 값을 추가 -> 반환
## Array.prototype.copyWithin()
- 인덱스 범위내의 값을 복사하여 같은 배열의 지정한 위치에 설정
- parameter
  - Number: target. 복사한 값을 설정할 시작 인덱스
  - Number: 복사 시작 인덱스, Default: 0
  - Number: 복사 끝 인덱스, Default: Array의 length
```
let one = [1, 2, 3, 4, 5];
console.log(one.copyWithin(0, 3)); // [4, 5, 3, 4, 5]
```
- Array-like는 배열이 아닌 오브젝트이므로 arrayLike.copyWithin(0,1)형태로 호출할 수 없다.
  -> call()을 함께 호출하면 호출이 가능하다
```
let arrayLike = {0: "ABC", 1: "DEF", 2: "가나다", length: 3};
let one = Array.prototype.copyWithin.call(arrayLike, 0, 1);
console.log(one); // Object {0: "DEF", 1: "가나다", 2: "가나다", length: 3};
```
## Array.prototype.fill()
- 같은 배열에서 인덱스 범위 내의 값을 지정한 값으로 바꾸어 반환
- parameter
  - all: 설정할 값
  - Number: 시작 인덱스
  - Number: 끝 인덱스
```
let one = [1, 2, 3];
console.log(one.fill(7)); // [7, 7, 7]
```
## Array.prototype.entries()
- Array오브젝트를 이터레이터 오브젝트로 생성해서 반환
- {key: value}형태로 배열의 인덱스가 key가 되고 배열의 값이 value가 된다.
## Array.prototype.keys(), Array.prototype.values()
- 전자: key만 갖는 이터레이터 오브젝트를 생성하여 반환
- 후자: value만 갖는 이터레이터 오브젝트를 생성하여 반환
```
let iterator = [10, 20, 30].keys();
for (var key of iterator){
  console.log(key, ":", iterator[key]); // 0: undefined;
};                                      // 1: undefined;
                                        // 2: undefined;
```
## Array.prototype.find(), Array.prototype.findIndex()
- 전자: 콜백함수에서 true를 반환하면 처리중인 엘리먼트 값(value)을 반환
- 후자: 콜백 함수에서 true를 반환하면 처리중인 엘리먼트의 인덱스(Index)를 반환
- parameter
  - Function: 콜백함수
  - Object: 콜백함수에서 this로 접근할 오브젝트
- 배열 엘리먼트를 하나씩 읽어가며 파라미터에 작성된 콜백 함수를 호출 -> 콜백 함수가 true를 반환하면 find()를 종료하고 현재 배열 value를 반환
```
let result = [1, 2, 3].find((value, index, allData) => value === 2);
console.log(result); // 2
```

# Class Object

## Class란
- new 키워드를 사용하여 인스턴스를 생성하여 사용한다.
- class는 hoisting되지 않으므로 객체 생성코드가 class코드보다 앞에 올 수 없다.
- constructor가 있어 객체 생성시 초기화를 담당한다.
```
  class Person{
    constructor(name, age){
      this.name = name;
      this.age = age;
    }
  }
```
- 메소드는 prototype에 정의된다.
- class는 function키워드를 사용하지 않지만 class와 메소드의 타입은 function 타입이다.
```
class Person{
    constructor(name, age){
        this.name = name;
        this.age = age;
    }

    introduce(){
       console.log(`나의 이름은 ${this.name}이고 나이는 ${this.age}입니다`);
    }
}
let tom = new Person("Tom", 15);
tom.introduce();
console.log(Person.prototype.introduce);
console.log(tom.__proto__.introduce === Person.prototype.introduce ); // true
console.log(tom.__proto__ === Person.prototype); // true
console.log(tom.__proto__.constructor === Person); // true
console.log(tom.__proto__.__proto__.constructor === Object); // true
console.log(tom instanceof Person); // true
console.log(tom instanceof Object); // true
console.log(typeof Person) // "function"
console.log(typeof Person.prototype.introduce) // "function"
```
## static 메소드
- 객체생성과 무관하게 호출이 가능하다.
```
class Person{
  static temp(){
    console.log('static method');
  }
}
Person.temp();
```
## getter, setter
- 객체의 속성은 동적으로 추가나 변경이 가능하다.
- private같은 접근제한자가 없어서 데이터가 변경되기 쉽다. -> getter, setter를 통해 데이터를 보호한다.
## 상속
- extends 키워드를 이용하여 상속받는다.
- super: 부모클래스의 생성자 함수를 호출하는 키워드. 자식클래스의 constructor에서 가장 먼저 위치해야 한다.
- constructor가 있으면 반드시 super를 넣어야 하며, constructor를 생략하면 super도 생략가능(자동으로 super가 삽입됨)
- 상속시 prototype객체가 어떻게 연결되는지 확인
```
class Person{
    constructor(name, age){
        this.name = name;
        this.age = age;
    }
    static laugh(){
    }
    introduce(){
       console.log(`나의 이름은 ${this.name}이고 나이는 ${this.age}입니다`);
    }
}

class Student extends Person{
    constructor(grade, name, age){
        super(name, age);
        this.grade = grade;
    }
}

let tom = new Student("mid-1", "tom", 15);

console.log(Student.prototype.hasOwnProperty("introduce")); // false
console.log(Person.prototype.hasOwnProperty("introduce"));  // true
```
- 하위 클래스의 생성자 함수와 상위 클래스 생성자 함수가 같다면 생략이 가능하다.
- static메소드도 상속이 된다.
```
console.log(Person.hasOwnProperty("laugh"));   // true
console.log(Student.hasOwnProperty("laugh"));  // false
console.log(Student.__proto__ === Person);     // true
console.log(Person.__proto__ === Function.prototype); // true
```
## Override
- 하위 클래스에서 상위 클래스를 메소드 이름을 같게하여 재정의 할 수 있다.
- super를 이용하면 부모클래스의 메소드를 호출할 수 있다
- static 메소드도 override가 가능하다.
## Builtin 객체 상속
- javascript에 내장된 Builtin객체 또한 상속하여 확장 가능하다.
- babel같은 트랜스파일러를 사용하면 Builtin객체를 상속하여 서브 클래스를 만들 수 없다.
```
class MyArray extends Array{
  clone(){
    return [...this];
  }
  push(...args){
    super.push(...args.filter(data=>!(data%2))); // 짝수만 push
  }
}
```
