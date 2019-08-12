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
- 관례적으로 첫 글자는 대문자로 작성한다.
- new 키워드를 사용하여 인스턴스를 생성하여 사용한다.
- class는 hoisting되지 않으므로 객체 생성코드가 class코드보다 앞에 올 수 없다.
- strict 모드에서 실행된다.("use strict"를 선언하지 않아도 strict 모드에서 실행된다)
- class의 type은 function이다.(console.log(typeof class); // function)
- function은 window 오브젝트에 설정되지만 클래스는 그렇지않다.
  - window.Classname 으로 클래스에 접근시 undefined가 반환
- class의 프로퍼티는 for문 으로 열거할 수 없음
- constructor가 있어 객체 생성시 초기화를 담당한다.
  - new로 객체 생성시 Class.prototype.constructor 호출됨
  - 인스턴스틑 생성하는 역할(new 연산자는 constructor를 호출하여 파라미터를 넘겨주기만 함) -> this 사용 가능
```
  class Person{
    constructor(name, age){
      this.name = name;
      this.age = age;
    }
  }
```
- 메소드는 prototype에 정의된다. (메소드 선언시 Class.prototype.method 형태로 정의됨)
  - 클래스 밖에서 클래스에 메서드 추가하려면 Class.prototype.method = function(){}; 형태로 작성해야 함
  - 클래스 밖에서 메서드 추가시, 이미 생성된 인스턴스에서 공유하도록 메서드가 처리하므로 부하가 걸림

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
  - 메서드 명 앞에 get 혹은 set을 작성.
```
 class Member{
  get getName() {
    return "이름";
  }
 }
```
## computed name
- 클래스 내 메서드의 이름을 조합하여 작성할 수 있음
```
let type = "Type";
class Sports {
  static ["get" + type](kind){
    return kind ? "스포츠" : "음악";
  }
}
console.log(Sports["get" + type](1)); // 스포츠
```
## Generator
- 클래스 내에 generator 함수를 작성할 수 있다.
  - generator 참고: https://blog.naver.com/wlwl16/221224324899
## new.target
- meta 프로퍼티로 생성자 함수와 클래스에서 constructor를 참조함 -> new 연산자로 인스턴스를 생성해야만 참조가 가능해진다.
- new.target.name 으로 클래스명을 출력할 수 있음
- 
## 상속
- extends 키워드를 이용하여 상속받는다.
- super: 부모클래스의 생성자 함수를 호출하는 키워드.
  - 생성자의 경우자식클래스의 constructor에서 가장 윗줄에 위치해야 한다.
  - sub class와 super class에 같은 이름의 메서드가 존재하면 super class의 메서드가 호출되지 않음 -> super.methodName()으로 호출 가능
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
- ES5에서 상속을 구현하는 법
```
function Sports(member){ //new 연산자로 인스턴스를 생성하는 함수(대문자로 작성)-> 생성자 함수
  this.member = member;
};
Sports.prototype.setItem = function(item){ // Sports의 prototype에 메서드 연결
  this.item = item;
};
function Soccer(member){ // Soccer()가 호출되면 Sports()를 호출
  Sports.call(this, member); // 인스턴스에 초기값만 설정
};
Soccer.prototype = Object.create(Sports.prototype, { // Sports.prototype을 사용하여 인스턴스를 생성
  setGround: { // setGround를 Sports.prototype에 첨부 -> Soccer.prototype.__proto__에 첨부
    value: function(ground){
      this.gorund = ground;
    }
  }
});
Soccer.prototype.constructor = Soccer;

var obj = new Soccer(11);
obj.setItem("축구");
obj.setGround("상암");
console.log(obj.member); // 11
console.log(obj.item); // 축구
console.log(obj.ground); // 상암
```
## Override
- 하위 클래스에서 상위 클래스를 메소드 이름을 같게하여 재정의 할 수 있다.
- super를 이용하면 부모클래스의 메소드를 호출할 수 있다
- static 메소드도 override가 가능하다.
## Builtin 객체 상속
- javascript에 내장된 Array 등의 Builtin객체 또한 상속하여 확장 가능하다.
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
- Object.setPrototypeOf(super,sub): sub._ _proto_ _에 super 오브젝트의 프로퍼티가 첨부됨
## Image 오브젝트 상속
- DOM에서 제공하는 Image, Audio등의 인터페이스 등을 상속받을 수 있다.
```
class ExtendsImage extends Image{
  constructor(){
    super();
  }
  setProperty(image){
    this.src = image.src;
    this.alt = image.alt;
    this.title = image.title;
  }
};
let imageObj = new ExtendsImage(0;
let properties = {
  src: "file/rainbox.png"
  alt: "나무와 집이 있고 그 위에 무지개가 있는 모습",
  title: "무지개"
};
imageObj.setProperty(properties);
document.querySelector("body").appendChild(imageObj); // body의 자식으로 img를 첨부함
```
