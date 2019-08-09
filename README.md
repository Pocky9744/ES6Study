# ES6Study

##Class
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
##static 메소드
- 객체생성과 무관하게 호출이 가능하다.
```
class Person{
  static temp(){
    console.log('static method');
  }
}
Person.temp();
```
##getter, setter
- 객체의 속성은 동적으로 추가나 변경이 가능하다.
- private같은 접근제한자가 없어서 데이터가 변경되기 쉽다. -> getter, setter를 통해 데이터를 보호한다.
##상속
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
##Override
- 하위 클래스에서 상위 클래스를 메소드 이름을 같게하여 재정의 할 수 있다.
- super를 이용하면 부모클래스의 메소드를 호출할 수 있다
- static 메소드도 override가 가능하다.
##Builtin 객체 상속
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
