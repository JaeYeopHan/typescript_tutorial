<div align="center">

![](./assets/typescript.png)

</div>

# Intro to TypeScript in 15 minutes
TypeScript Tutorial 문서를 저장하는 repository 입니다.

#### [TypeScript Playground](https://github.com/JaeYeopHan/typescript_playground)

### Table of All Contents
- Basic Types
  - Boolean
  - Number
  - String
  - Array
  - Tuple
  - Enum
  - Any
  - Void
  - Null, Undefined
  - Never
  - Type Assertion
- Class
  - Constructor
  - extend
  - Access Modifier
    - public
    - private
    - protected
    - readonly
    - static
- Function
  - Return type, Parameter type
  - Default Parameter / Rest Parameter
  - Optional Parameter
  - Union Type
  - Overloading
- Interface
  - Interface?
  - Useful Interface
  - Available properties
    - Optional
    - readonly
  - Interface Type
    - Function Type
    - Indexable Type
  - Class interface
- Generics
  - Generics?
  - Generics to Class
  - Generics to Function
- Decorators
  - Setup
  - Intro
  - Decorator to method
  - Decorator to class
  - Decorator with parameter
- Type System
  - TypeScript의 Type Checking System
  - Type Inference
  - Type Assertion
  - Type Guards
  - Type Compatibility

</br>

---
---

</br>

# [TS] 1. Basic Types
TypeScript에서는 JavaScript에서 사용했었던 `number`, `string`, `boolean`과 같은 데이터 타입을 기반으로 `정적 타이핑`을 지원합니다. 변수에 타입을 지정해주기 위해서 TypeScript에서는 `:`을 통해 지원합니다. 기존의 JavaScript에서 변수를 선언하면서 `:`으로 해당 변수의 타입을 지정해줍니다.

### Table of Contents
- Boolean
- Number
- String
- Array
- Tuple
- Enum
- Any
- Void
- Null, Undefined
- Never
- Type Assertion

</br>

### Boolean
```typescript
let isExist: boolean = false;
```
`boolean` 타입을 지정합니다.

### Number, 숫자
```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```
`number` 타입을 지정합니다.

### String, 문자열
```typescript
let name: string = "jbee";
let greeting: string = `Hi, I'm ${name}!`;
```
`string` 타입을 지정합니다.

### Array, 배열
```typescript
let arr: number[] = [1, 2, 3];
```
or
```typescript
let arr: Array<number> = [1, 2, 3];
```
기존에 배열 리터럴을 사용하여 배열을 정의하면서 정의하는 배열에 어떠한 데이터 타입의 원소가 들어갈 것인지를 Type을 통해 제공할 수 있습니다.

### Tuple, 튜플
```typescript
let tuple: [string, number];
tuple = ["age", 25];
tuple = ["name", "jbee];// Error
> message: 'Type '[string, string]' is not assignable to type '[string, number]'.
  Type 'string' is not assignable to type 'number'.' at: '5,1'
```
`key-value`의 형태를 저장할 때는, 위와 같이 타입을 지정해줄 수 있습니다. TSLint가 잘못된 타입의 값이 들어왔다는 것을 error message로 알려줍니다.

### Enum
```typescript
enum Color {Red, Green, Blue};
let c: Color = Color.Green;
```
마찬가지로 Java의 `enum`과 같은 구조를 갖습니다. 시작하는 멤버에 0부터 번호를 매기고, 만약 1부터 시작해야 한다면 임의적으로 시작하는 숫자를 지정할 수 있습니다. 또는 각각에게 번호를 지정할 수도 있습니다.
```typescript
enum Subject {Math = 1, Science = 3, History = 7}
console.log(Subject[3]);//Science
```
지정한 번호로 호출도 가능합니다.

`tsc` 명령어를 통해서 저희가 작성한 TypeScript 코드를 변환된 Javascript 코드로 볼 수 있습니다. TypeScript의 `Enum`은 다음과 같이 변환되는 것을 확인하실 수 있습니다.
```javascript
var Subject;
(function (Subject) {
    Subject[Subject["Math"] = 1] = "Math";
    Subject[Subject["Science"] = 3] = "Science";
    Subject[Subject["History"] = 7] = "History";
})(Subject || (Subject = {}));
```
자바스크립트의 다섯 줄이 타입스크립트 한 줄로 작성되었습니다 :)

### Any
코드를 작성하면서 사용되는 변수에 알맞은 데이터 타입을 설정하는 것은 중요하지만 데이터 타입이 동적으로 결정되는 변수도 존재하게 됩니다. 이럴 때 사용할 수 있는 타입이 `any`입니다.
```typescript
let notSureVar: any;
notSureVar = 3;
notSureVar = `hi`;
notSureVar = [1,2,3];
```
`any`로 타입을 지정하면 Compile Time에서 `Type checking`을 하지 않습니다.
```typescript
let notSureVar; // == let notSureVar: any;
```
`any`로 타입을 지정하는 것과 위의 자바스크립트는 동치라고 볼 수 있습니다.
```typescript
let arr: any[] = [1, `jbee`, true];
```
여러 가지 타입의 요소가 포함되는 배열을 정의할 때도 `any`를 사용할 수 있습니다.
`any`의 역할이 다른 언어에서의 `Object`와 같은 역할을 하는 느낌이 드는데요, 실제로 2.2version에서 `Object`타입이 추가되었습니다. 일단 코드를 통해 확인해보겠습니다.
```typescript
let user = {
    getName() {
        console.log(`hi`);
    },
    name : "jbee"
}
let notSureObj: Object;
notSureObj = user;
notSureObj.getName(); // error
notSureObj.name; // error
//Property 'getName' does not exist on type 'Ojbect'

let notSureVar: any;
notSureVar = user;
notSureVar.getName(); // success
notSureVar.name; // "jbee"
```
변수의 type을 `Object`로 지정하고, 실제 Object를 정의하여 변수에 할당했습니다. 그리고 할당한 Ojbect에 존재하는 메소드를 호출했더니 에러가 발생합니다. `getName()`이라는 프로퍼티가 없다고 하네요. 분명 `getName()`메소드가 존재하는 Ojbect를 할당했는데 말이죠. `name` 프로퍼티도 마찬가지입니다. 하지만 `any` 타입으로 지정했을 때는 메소드를 호출할 수 있고, 프로퍼티를 찾을 수 있습니다. 이 `Object` 타입은 할당만 가능할 뿐, 메소드나 프로퍼티에 접근할 수 없습니다.
[About Object Type](https://blog.mariusschulz.com/2017/02/24/typescript-2-2-the-object-type)

### Void
값을 반환하지 않는 함수의 return type을 지정할 때 사용할 수 있습니다.
```typescript
function greeting(): void {
    console.log(`hi`);
}
```
물론 변수의 타입에도 사용할 수 있습니다.
```typescript
let foo: void;
foo = undefined;
foo = null;
foo = `foo`; //error
> Type 'string' is not assignable to type 'void'
```
하지만 `void`로 선언된 변수에는 `undefined`와 `null` 값만 할당할 수 있습니다.

### Null, Undefined
기본적으로 `null`과 `undefined`는 모든 타입들의 서브타입이라고 할 수 있습니다. 즉 다른 타입으로 지정된 변수에도 `null`과 `undefined`를 할당할 수 있습니다.
> `--strictNullChecks` flag를 사용하게 되면 `null`과 `undefined`는 `void`타입의 변수에만 할당할 수 있습니다. TypeScript에서는 해당 flag사용을 권장하고 있습니다.

### Never
`never` 타입은 발생하지 않는 경우에 대한 타입을 대표하는 타입입니다. 코드를 통해 살펴보겠습니다.
```typescript
// Function returning never must have unreachable end point
function error(message: string): never {
    throw new Error(message);
}

// Inferred return type is never
function fail() {
    return error("Something failed");
}

// Function returning never must have unreachable end point
function infiniteLoop(): never {
    while (true) {
    }
}
```
위의 예제와 같이 `throw`를 하거나 error를 발생시키는 function의 return type으로 설정합니다.
[About Never Type](https://blog.mariusschulz.com/2016/11/18/typescript-2-0-the-never-type)

### Type Assertions
타입 어설션이란 개발자가 타입스크립트에게 "내가 무슨 짓을 하고 있는지 아니까, 나를 믿어줘!"하고 메세지를 보내는 것입니다. 주로 엔티티의 타입을 보다 구체적으로 설정할 때 사용합니다.
```typescript
let greet: any = `Hello!`;
let lengthOfGreet: number;
lengthOfGreet = greet.length;
lengthOfGreet = (<string> greet).length;
lengthOfGreet = (greet as string).length;
```
`any`타입에 대해서 `형변환(type casting)`하는 것처럼 보이지만 실제로는 특별한 검사나 데이터 재구성을 하지 않습니다. 이 타입 어설션은 컴파일 타임에만 영향을 미칠뿐 런타임시에는 아무런 영향을 주지 않습니다. 보다 데이터 타입을 구체화 시킬 때 사용합니다.
`<>`를 사용하는 방법과 `as`를 사용하는 두 가지 방법이 존재합니다. 선호도에 따라 어떻게 사용할지 결정할 수 있지만 `JSX`와 함께 사용하는 경우에는 `as`를 사용하는 방법만 허용됩니다.


</br>

<sup>[(목차로 돌아가기)](#intro-to-typescript-in-15-minutes)</sup>

---
---

</br>

# [TS] 2. Class in TypeScript
### Table of contents
- Constructor
- extend를 통한 상속
- Access Modifier
  - public
  - private
  - protected
  - readonly
  - static

자바스크립트 개발자라고 하더라도 ES6덕분에 우리는 클래스에 이미 많이 익숙해져 있습니다. 그러나 ES6의 클래스는 뭔가가 조금 아쉬웠는데요, 이 부분을 TypeScript가 채워줄 예정입니다. 일단 ES6에서 클래스를 살펴봅니다.


_ES6 code_
```js Person.js
class Person {
  constructor(firstName, lastName) {
    this._firstName = firstName;
    this._lastName = lastName;
  }

  get fullName() {
    return `${this._firstName} ${this._lastName}`;
  }
}

const p1: Person = new Person("Jbee", "Han");
console.log(p1.fullName); // Jbee Han
```
익숙한 클래스의 형태라고 생각합니다. 익숙하지 않으시다면 [ES6 Class에 대한 포스팅](https://jaeyeophan.github.io/2017/04/18/ES6-6-Class-sugar-syntax/)을 참고해주세요. TypeScript에서의 클래스에는 ES6 코드와 비교해봤을 때, 이것 저것 키워드가 많이 추가가 됬는데요, 하나씩 알아보기로 합시다.


## 1. Constructor
```ts
class Person {
  name: string;

  constructor(name, gender) {
    // ...
    this.gender = gender; // Error!
  }
}
```
ES6에서는 constructor로 받은 인자를 해당 클래스에 멤버로 등록하기 위해 this로 등록을 해줬습니다. 그 제한 또한 없었습니다. 하지만 타입스크립트에서는 해당 클래스의 프로퍼티로 등록되지 않은 속성에 대해 constructor에서 받을 수 없습니다. 위 코드에서는 `gender`라는 프로퍼티를 정의하지 않았기 때문에 constructor에서 인자로 받을 수 없습니다.

> 프로퍼티(or filed member)로 미리 정의되지 않은 인자를 constructor에서 받을 수 없습니다.

</br>

## 2. extend를 통한 상속
TypeScript에서도 ES6와 마찬가지 방식으로 상속을 구현할 수 있습니다. `public`, `protected` 접근제어자로 정의된 프로퍼티와 메소드에 접근할 수 있습니다. (접근제어자에 대해서는 바로 다음 section에서 알아봅니다.)
```ts
class Person {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sayName() {
    console.log(this.name);
  }
}

class Developer extends Person {
  constructor(name: string) {
    super(name);
  }
}

const devPerson: Person = new Developer("jbee");
devPerson.sayName(); // jbee
```
`sayName`은 `Person`클래스에만 정의되어 있는 메소드입니다. `Person`에서 정의한 대로 `this.name`을 console에 출력하는 것을 확인할 수 있습니다.

> extends 키워드를 통해서 클래스를 상속받아 부모 클래스의 메소드와 프로퍼티에 접근할 수 있습니다.

#### super
자식 클래스에서 constructor를 정의하려면 `super()`를 꼭 호출해줘야 합니다. (이는 ES6 문법과 동일합니다.) `super()`로 넘어가게 되는 인자 또한 부모 클래스에서 정의한 signature와 동일해야 합니다. 물론 별도로 정의하지 않으면 부모 클래스의 constructor를 따라갑니다.

### Override method
`Developer`클래스에서 해당 메소드 구현하게 되면 부모 클래스의 메소드를 override 하게 됩니다.
```ts
class Developer extends Person {
  constructor(name: string) {
    super(name);
  }

  sayName() {
    cosole.log(`I'm developer, ${this.name}`);
  }
}

const devPerson: Developer = new Developer("jbee");
devPerson.sayName(); // I'm developer, jbee
```
`sayName`이라는 메소드는 `Developer`클래스에서 재정의된 형태로 console에 출력합니다.

> 부모 클래스를 override하게 되면 부모 클래스에서 정의된 메소드를 재정의할 수 있습니다.

한 가지 짚고 넘어가자면, 자바에서 지원되는 overloading이 지원되지 않습니다. 따라서 다음과 같은 메소드는 추가할 수 없습니다.
```ts
class Developer extends Person {
  // ...
  sayName(position) { // Error!
    // ...
  }
}
```
`[!] Types of property 'sayName' are incompatible.`라는 에러가 발생합니다. 자식 클래스에서 부모 클래스와 메소드의 이름은 동일하고 signature가 다른 메소드는 정의할 수 없습니다. 이미 부모 클래스에 `sayName`이라는 메소드가 존재하기 때문에 해당 메소드와 동일한 signature로 override하지 않는 이상, 다른 signature로 또다른 메소드를 정의할 수 없습니다.

> **[!]** 하지만 `any`라는 타입과 함께 오버로딩을 메소드 내부에서 if 문 또는 switch 문으로 분기하여 비슷하게 구현할 수 있습니다. **바로 다음에 다룰 Function 포스팅에서 다루겠습니다.**

</br>

## 3. Access Modifier
TypeScript에서는 클래스의 프로퍼티 또는 메소드에 접근제어자를 추가할 수 있습니다.

### public (by default)
타입스크립트에서 아무 접근 제어자를 추가하지 않으면 기본적으로 `public` 접근제어자와 동일하게 동작합니다. ES6에서와 마찬가지로 클래스 내부, 외부에서 모두 접근할 수 있는 프로퍼티를 정의할 때 사용합니다. `getter`, `setter`를 별도로 만들지 않아도 접근이 가능합니다. 위에서 정의한 `Person`클래스를 사용하여 예시를 보겠습니다.
```ts
const p1 = new Person("jbee");
console.log(p1.name); // "jbee"
```
프로퍼티에 바로 접근할 수 있는 것을 확인할 수 있습니다.

</br>

### private
`private` 접근 제어자로 정의된 멤버는 클래스 밖에서 접근할 수 없습니다. 이번 예시 코드에서는 `getter`, `setter`를 추가해주기 위해 프로퍼티 명 앞에 `_`(언더바)를 추가했습니다.
```ts
class Person {
  name: string;
  private _job: string;

  constructor(name, job) {
    // ...
  }
}

const p1: Person = new Person("jbee", "Developer");
console.log(p1._job); // Error!
```
`Property '_job' is private and only accessible within class 'Person'.`이라는 에러가 발생합니다. 클래스 내부가 아닌 외부에서 `private`으로 정의된 프로퍼티에 접근하려고 했기 때문에 발생하는 에러입니다. 해당 프로퍼티에 접근하기 위해 `getter`를 추가하겠습니다.
```ts
class Person {
  // ...
  get job() {
    return this._job;
  }
}
const p1: Person = new Person("jbee", "Developer");
console.log(p1.job); // Developer
```
이렇게 접근할 수 있게 됩니다. `getter`, `setter`를 추가하는 것보다 접근제어자를 `public`하는 것이 맞는 것 같습니다.
> 프로퍼티 또는 메소드에 `private`이라는 접근 제한자를 붙이게 되면 클래스 내에서만 사용할 수 있는 프로퍼티, 그리고 메소드가 됩니다.

</br>

### protected
기본적으로는 `private` 접근 제어자와 동일하게 동작합니다. 하지만 어느 한 곳에서는 접근이 가능한데요, 바로 해당 클래스를 상속한 클래스에서 접근이 가능합니다.
```ts
class Perosn {
  protected isWorking: boolean;
  // ...
}

const p1: Person = new Person(true);
console.log(p1.isWorking);// Error!
```
`[!] Property 'isWorking' is protected and only accessible within class 'Person' and its subclasses.`라는 에러가 발생합니다. `private` 접근제어자와 마찬가지로 클래스 외부에서 접근할 수 없음을 뜻합니다.

```ts
class Developer extends Person {
  constructor(isWorking) {
    super(isWorking);
  }

  isWork() {
    console.log(this.isWorking);
  }
}

const devPerson: Developer = new Developer(true);
devPerson.isWork(); // true
```
위 `Person`클래스를 상속한 클래스에서는 `protected`로 정의된 프로퍼티에 접근할 수 있습니다.
> 프로퍼티 또는 메소드에 `protected`이라는 접근 제한자를 붙이게 되면 클래스 내에서와 해당 클래스를 상속한 클래스 안에서만 사용할 수 있게 됩니다.


이 접근 제어자는 `constructor`에도 추가될 수 있습니다. `constructor`에 해당 접근제어자를 추가하는 경우, 해당 클래스는 바로 인스턴스화될 수 없으며 이 클래스를 상속받은 클래스에서 `super`라는 키워드로 호출이 가능합니다.

```ts
class Perosn {
  protected constructor() {
    // ...
  }
}
class Developer extends Person {
  constructor() {
    super();
  }
}

const p1: Person = new Person(true); // Error!
const devPerson: Developer = new Developer(); // OK!
```
`[!] Constructor of class 'Person' is protected and only accessible within the class declaration.`라는 에러가 발생하며 상속받은 클래스만 인스턴스화 시킬 수 있습니다. 추상클래스(Abstract class)도 마찬가지로 상속을 하기 위한 클래스인데요, 추상 클래스는 구현되지 않은 메소드가 존재하는 반면 이 방식은 모든 메소드가 구현되어야 합니다.


#### ES.NEXT
[tc39/proposal-class-field](https://github.com/tc39/proposal-class-fields)를 참고해보시면, ECMAScript에서도 `private`에 대한 스펙이 논의 중이며(stage-3) 해당 스펙은 `private`이라는 키워드 대신 `#`이라는 키워드로 스펙이 논의 중입니다.

</br>

### readonly
자바를 공부하셨던 분이라면 `final`키워드를 아실텐데요, TypeScript에서는 읽기 전용 프로퍼티를 설정하기 위해 `readonly`프로퍼티를 사용합니다.
```ts
class Person {
  public readonly age: number;

  // ...

  set setAge(age: number) {
    this.age = age; // Error!
  }
}

const p1: Person = new Person(25);
p1.age = 20; // Error!
```
`[!] Cannot assign to 'age' because it is a constant or a read-only property.`라는 에러가 발생합니다! `readonly`키워드가 추가되면 상수(constant)로 인식하게 됩니다.
> `readonly`와 함께 정의된 프로퍼티는 `constructor`에서 한 번 결정되면 수정할 수 없습니다.


</br>

### static
`static` 키워드는 ES6에서와 동일하게 사용되며 사용할 수 있습니다. 다만 ES6에서는 메소드에만 해당 키워드를 추가하는 것이 가능했는데요, TypeScript에서는 `static` 키워드를 프로퍼티에도 추가할 수 있습니다. 해당 키워드를 프로퍼티 또는 메소드에 추가하게 되면 인스턴스를 생성하지 않고 접근하거나 생성할 수 있습니다.
_ES6 code_
```js
class Circle {
  // ...
}
Circle.center = [0,0]
```
_TypeScript code_
```ts
class Circle {
  static center: number[];
  //...
}

```


### 마무리
TypeScript에서 사용하는 클래스에 대해 알아봤습니다. 잘 사용하던 ES6에서의 클래스가 조금 덜떨어져 보일 수 있습니다!

해당 포스팅 외 다른 타입스크립트 포스팅은 [여기](https://github.com/JaeYeopHan/typescript_tutorial_docs)에서 보실 수 있으며 예제에 사용된 코드는 [여기](https://github.com/JaeYeopHan/typescript_playground)에서 확인하실 수 있습니다.
감사합니다.

</br>

<sup>[(목차로 돌아가기)](#intro-to-typescript-in-15-minutes)</sup>

---
---

</br>

# [TS] 3. Function in TypeScript
TypeScript에서 함수를 정의하는데 있어서 몇 가지 추가된 기능에 대해 살펴봅니다.

#### Table of Contents
* Return type, Parameter type
* Default Parameter / Rest Parameter
* Optional Parameter
* Union Type
* Overloading

## 인자와 반환값의 타입을 설정한다.
함수 또는 메소드를 정의할 때, 타입을 정의해줍니다.
_ES6 code_
```js
function getMonthFromString(dateOfStringFormat) {
  const monthOfNumberFormat = parseInt(dateOfStringFormat.substring(4, 6), 10);
  return monthOfNumberFormat;
}
```
위 함수는 '201712'이라는 문자열을 받아서 해당하는 월을 반환하는 함수입니다. (반환값의 형식을 명시하기 위해 바로 return하지 않고 변수에 임시로 받아준 뒤 반환합니다.) 위와 같이 String 타입의 인자를 받아야 한다는 것을 명시해줘야 하기 때문에 변수명부터 굉장히 이상해집니다. 자바스크립트에는 타입이라는 것이 없기 때문에 메소드명 또는 변수명에 타입을 명시할 수 밖에 없습니다.

만약 Number 타입의 201712를 인자로 넘겨준다면 Number에는 substring이라는 함수가 없기 때문에 에러가 발생합니다. 위 함수를 보다 안정적으로 작성하기 위해서는 다음과 같은 if 문이 필요하게 됩니다.
```js
function getMonthFromString(dateOfStringFormat) {
  if (typeof dateOfStringFormat !== "string") {
    throw Error("Invalid format of parameter");
  }
  const monthOfNumberFormat = parseInt(dateOfStringFormat.substring(4, 6), 10);
  return monthOfNumberFormat;
}
```
기본적으로 함수가 수행해야하는 비즈니스 로직 외 불필요한 방어코드가 코드를 더럽히고 있습니다. 위 함수를 TypeScript 함수로 변경해보겠습니다.

```ts
const getMonth = (date: string): number => {
  return parseInt(date.substring(4, 6), 10);
}
```
인자와 반환값에 타입을 설정하여 메소드 이름과 변수명이 훨씬 짧아졌습니다. 그럼에도 불구하고 해당 함수가 하는 역할을 ES6로 작성했을 때보다 명확해졌으며, 불필요한 방어코드 마저 사라졌습니다.
[wtfjs](https://github.com/denysdovhan/wtfjs)에서 확인하실 수 있지만 자바스크립트에서는 타입이 멋대로(사실은 매우 다양한 규칙을 기반으로) 캐스팅되는 경우가 많은데요, 이를 방지하기 위해 우리는 불필요한 방어코드를 작성해왔습니다. 타입을 지정함으로써 이러한 작업을 최소화 할 수 있습니다.

</br>

## Default Parameter, Rest Parameter
해당 스펙은 ES6 표준 스펙에서도 지원하고 있는 스펙이므로 구체적은 설명은 넘어가겠습니다. 자세한 내용은 첨부하는 포스팅을 확인해주세요.
* [ES6. Rest Parameter](https://jaeyeophan.github.io/2017/04/18/ES6-4-Spread-Rest-parameter/)
* [ES6. Default Parameter](https://jaeyeophan.github.io/2017/04/18/ES6-5-Destructuring-and-Default-Parameter/)

TypeScript에서도 해당 스펙을 지원합니다.
_Default parameter TypeScript code_
```ts
const getRandomNumber = (min: number = 0, max: number = 10): number => {
    return Math.floor(Math.random() * (max - min)) + min;
}

console.log(getRandomNumber(1)); // OK!
```
위 코드에서는 인자가 넘겨지지 않았을 경우(`undefined`), 지정해준 값으로 인자를 설정합니다. 명시적으로 `null`을 인자로 넘겨주면 default로 설정된 parameter를 무시하고 `null`을 인자로 넘깁니다. 지정한 인자를 모두 넘기지 않으면 에러를 뱉던 TypeScript도 default parameter가 지정되어 있으면 에러를 발생시키지 않습니다.

_Rest parameter TypeScript code_
```ts
export const setSkills = (...skills: string[]): void => {
    // ...
}
```
`rest parameter`의 타입은 배열(array)이므로 인자에 해당하는 타입을 설정해줍니다.

</br>

## Optional Parameter
TypeScript에서는 default parameter 없을 경우, signature에서 정의한대로 인자를 넘겨주지 않으면 에러가 발생합니다. 하지만 파라미터를 넘겨주지 않아도 되도록 설정할 수 있습니다.
```ts
const setSpec = (major: string, option?: string): void => {
  console.log(major);
  console.log(option);
}
setSpec("Computer Science");
// console> Computer Science
// console> undefined
```

> `?`를 통해서 함수에 optional한 parameter를 지정할 수 있습니다.

</br>

## Union Type
파라미터에 타입을 지정할 때, 두 가지 이상의 타입이 지정할 필요가 있을 수 있는데요, 그럴 때 Union type을 통해서 파라미터의 타입을 지정해줄 수 있습니다.
```ts
sayName(position: string | boolean | number): void {
  if (typeof position === "string") {
    console.log(`string type position`);
  } else if (typeof position === "boolean") {
    console.log(`boolean type position`)
  } else {
    console.log(`else`);
  }
}
```
위 코드에서는 `position`이라는 파라미터가 `string`, `boolean`, `number` 세 가지의 타입일 수 있다고 signature를 지정했습니다. (예제가 송구스럽네요)

</br>

## Overloading
바로 이전 [Class 포스팅](https://jaeyeophan.github.io/2017/12/13/TS-2-Class/)에서 자바와 같은 오버로딩을 지원하지 않는다고 했는데요, `optional parameter`와 `union type` 그리고 `any`라는 타입을 사용하면 자바에서 구현하는 것과는 조금 다르지만 오버로딩을 구현할 수 있습니다.

```ts Person.ts
class Person {
  //..
  sayName(position: string, option?: string): void;
  sayName(position: boolean, option: string): void;
  sayName(position: string | boolean, option: any): any {
    if (typeof position === "string") {
      console.log(`string type position`);
    } else if (typeof position === "boolean") {
      console.log(`boolean type position`)
    } else {
      console.log(`else`);
    }
  }
  //..
}
```
위와 같이 동일한 메소드 명에 대해 여러 Signature를 정의할 수 있습니다. 위 코드에서는 `sayName`이라는 메소드의 Signature가 세 개이며 마지막 메소드에서만 이를 구현하고 있습니다. 그리고 메소드 body에서는 parameter의 타입으로 분기를 하여 로직을 수행하고 있습니다.
```ts
const person: Person = new Person();

person.sayName("FrontEnd");
person.sayName("FrontEnd", "optional);
person.sayName(false, "option required");
// person.sayName(true); Error! (1)
// person.sayName(1); Error! (2)
```
`sayName`을 호출하게 되면, 메소드를 **호출하는 시점에서** 각 상황에 맞는 signature가 적용됩니다. `Error (1)`을 보면 `boolean` 타입이 인자로 넘어갔을 경우의 signature에 따라 `option`이 `required` 인자이므로 에러가 발생합니다. `Error (2)`는 어느 signature와도 일치하지 않으니 에러가 발생합니다. 이렇게 TypeScript에서는 여러 signature를 정의한 뒤 메소드 내에서 이를 분기하여 오버로딩을 구현할 수 있습니다.

</br>

### 마무리
TypeScript 좀 더 안정성 있는, 간결한, 가독성이 좋은 함수를 작성할 수 있게 되었습니다. 어떻게 보면 타이핑이 길어지는 결과처럼 보일 수 있겠지만 타입의 명시가 필요한 부분에 있어서는 오히려 코드가 더 짧아지게 되었습니다.


</br>

<sup>[(목차로 돌아가기)](#intro-to-typescript-in-15-minutes)</sup>

---
---

</br>

# [TS] 4. Interface in TypeScript
`interface`는 자바스크립트 개발자에게 친숙하지 않은 용어일꺼라고 생각됩니다. 하지만 정적 타이핑에 있어서 큰 부분을 차지하고 있는 syntax에 대해 알아봅니다.

#### Contents
- Interface?
- Useful Interface
- Available properties
  - Optional
  - readonly
- Interface Type
  - Function Type
  - Indexable Type
- Class interface

</br>

## Interface?
`interface`란 **객체의 껍데기 또는 설계도** 라고 할 수 있을 것 같습니다. 자바스크립트에서는 클래스도 함수도 결국 모두 객체인데요, 클래스 또는 함수의 '틀'을 정의할 때 사용할 수 있는 것이 인터페이스 입니다.

여러 함수가 특정한 시그니처를 동일하게 가져야 할 경우 또는 여러 클래스가 동일한 명세를 정의해야하는 경우 인터페이스를 통해서 정의할 수 있습니다. 인터페이스는 특정 타입으로서 사용될 수 있으며, `implements`의 대상이 될 수 있습니다. 객체에 인터페이스를 적용하는 경우 또는 반환값 등을 설정할 때 타입으로 이용될 수 있습니다.

</br>

## Useful Interface
함수에서 어떤 객체를 받아야 할 경우, 해당 type을 어떻게 정의할 수 있을까요?
```ts
// AjaxUtils.ts
export async function fetchBasic(param: {url: string}): Promise<Response> {
  const response = await fetch(param.url);
  return response.json();
}
```
위와 같이 `param`이라는 인자의 타입을 literal 형식으로 정의할 수 있습니다. 그런데 그 객체의 프로퍼티가 많아지는 경우에는 어떻게 정의할까요? (반환값 형식에 `Promise<?>`형식으로 지정을 해줬는데요, 이는 Generics 부분에서 다룰 예정입니다.)

```ts
export async function fetchData(param: {baseUrl: string, type: string, subject?: string}): Promise<Response> {
  const {baseUrl, type, subject} = param;
  const response = await fetch(`${baseUrl}/${subject}`);
  const contentType = response.headers.get("content-type");

  if (response.ok && contentType && contentType.includes(type)) {
    return response.json();
  }
  throw Error("Invalid baseUrl or subject");
}
```
literal로 인자의 타입을 정의하다보니 함수의 signature가 너무 길어졌습니다. 그런데 여기서 `Promise<Response>` 이 부분도 정의가 필요합니다. 따라서 다음과 같이 길어집니다.
```ts
export async function fetchData(param: {
  baseUrl: string,
  type: string,
  subject?: string
}): Promise<{
  id: number;
  name: string;
  company: string;
}[]> {
  //...
}
```
뭔가 최대한 가독성을 좋게 하기 위해서 개행을 했지만 성공하진 못한 것 같습니다. 문제는 여기서 끝이 아닙니다. 이 함수를 호출하는 곳에서는 다음과 같은 일이 벌어집니다.

```ts
private async ajax(): Promise<{id: number; name: string; company: string;}[]> {
  const data: {id: number; name: string; company: string;}[] = await fetchData({
    baseUrl: "http://localhost:3000",
    subject: "users",
    type: "application/json"
  });
  // ...do something
  return data;
}
```
이 혼란의 상황을 interface를 통해 깔끔하게 정리할 수 있습니다. 인자와 반환값을 interface를 통해 정리해보겠습니다.

```ts
// interfaces.ts
interface Character {
  id: number;
  name: string;
  company: string;
}

export interface dataFormat {
  charaters: Character[];
}

export interface fetchDataParam {
  baseUrl: string;
  type: string;
  subject: string;
}
```
위와 같이 `interfaces.ts`라는 파일을 생성하여 인터페이스들을 정의할 수 있습니다.

```ts
// AjaxUtils.ts
import { fetchDataParam, dataFormat } from "./interfaces";
export async function fetchData(param: fetchDataParam): Promise<dataFormat> {
  // ...
}
```
`AjaxUtils.ts`에서는 정의한 인터페이스를 import하여 인자와 반환값에 해당 인터페이스를 통해 타입을 정의할 수 있습니다.

```ts
// Controller.ts
import { fetchDataParam, dataFormat } from "./interfaces";

private async ajaxCall(): Promise<dataFormat> {
  const param: fetchDataParam = {
    baseUrl: "http://localhost:3000",
    subject: "users",
    type: "application/json"
  };
  const data: dataFormat = await fetchData(param);
  console.log(data);
  return data;
}
```
호출하는 부분에서도 마찬가지로 `interfaces.ts`파일에서 필요한 인터페이스를 import하여 타입을 지정해줍니다. 예시 코드에서 처럼 인터페이스를 정의하여 인자에 정의하던 타입들을 깔끔하게 정리할 수 있습니다. 또한 다른 파일에서 해당 함수를 정의하는 부분과 호출하는 부분이 다를 때, 하나의 인터페이스를 공유할 수 있습니다. 인터페이스를 통일시키는 것이 중요할 때 매우 유용하게 사용할 수 있습니다.

</br>

## Available properties
인터페이스에도 클래스와 동일하게 optional하게 property를 지정할 수 있으며 readonly 타입으로 property를 지정할 수 있습니다.

### Optional properties
```ts
interface WeatherSpec {
  type: string;
  amount?: number;
}
const rainfall: WeatherSpec = {
  type: "rainfall",
};
```
optional property로 지정한 `amount`에 대해서는 구현하지 않아도 에러가 발생하지 않는 것을 확인할 수 있습니다.

</br>

### Readonly properties
```ts
export interface WeatherSpec {
  readonly type: string;
  amount: number;
}
const rainfall: WeatherSpec = {
  type: "rainfall",
  amount: 24,
}
rainfall.type = "snow"; // Error!
rainfall.amout += 3; // OK!
```
에러 메시지는 다음과 같습니다. `[!] Error: Cannot assign to 'type' because it is a constant or a read-only property.` 위와 같이 interface에서 readonly로 지정한 프로퍼티에 대해서는 그 값을 바꿀 수 없습니다. 이는 변수를 사용할 때 사용하는 `const`키워드와 동일한 역할을 수행한다고 이해할 수 있습니다.

</br>

## Interface Types
인터페이스에 프로퍼티를 정의할 때 여러 가지 형식으로 정의할 수 있습니다.

### Function Type
인터페이스의 프로퍼티로 함수의 시그니처를 정의할 수 있습니다. 반환하는 형식 또는 그 값이 다르지만 시그니처를 통일시켜야 하는 경우가 존재할 수 있습니다. 그럴 경우 다음과 같이 interface를 설계하여 함수를 구현할 수 있습니다.
```ts
interface TimeFunc {
  (hour: number, minutes: number): any;
}

const buildTimeStamp: TimeFunc = (hour, minutes): number => {
  if (minutes < 10) {
    return Number(`${hour}0${minutes}`);
  }
  return  Number(`${hour}${minutes}`);
}

const buildTimeText: TimeFunc = (hour, minutes): string => {
  if (minutes < 10) {
    return `${hour}시 0${minutes}분`;
  }
  return `${hour}시 ${minutes}분`;
}

buildTimeStamp(12, 33); //1233
buildTimeText(12, 33); //12시 33분
```
반환 타입을 제외하고 동일한 형식의 함수를 정의했습니다. 이것은 특정 콜백 함수를 받는 함수를 구현할 때 그 유용성이 더 빛을 발합니다.

```ts
const buildTime = (timeText: string, cb: TimeFunc) => {
  const hour: number = Number(timeText.split(":")[0]);
  const minutes: number = Number(timeText.split(":")[1]);

  return cb(hour, minutes);
}
console.log(buildTime("12:33", buildTimeStamp)); //1233
console.log(buildTime("12:33", buildTimeText)); //12시 33분
```
콜백 함수를 인자로 받을 때 해당하는 시그니처가 통일되어야 하는 부분을 인터페이스를 통해 해결할 수 있습니다.

</br>

### Indexable Types
자바스크립트에서 다음과 같은 코드는 매우 자연스럽습니다.
```js
const obj = {
  first: 1,
  second: 2,
};
Object.keys(obj).forEach(key => console.log(obj[key]));
```
즉, 객체의 프로퍼티에 접근할 때, 동적으로 생성된 `key`를 통해 객체의 프로퍼티에 `[]` 표기법으로 접근하는 경우입니다. 하지만 이 코드는 타입스크립트에서 동작하지 않습니다.

`[!] Element implicitly has an 'any' type because type '{ first: number; second: number; }' has no index signature.`이란 에러를 발생시킵니다. 왜냐하면 정의한 `obj`라는 객체에 index signature가 없기 때문입니다. 따라서 이 에러는 다음과 같이 해결할 수 있습니다.
```ts
interface Indexable {
  [key: string]: any;
};
const obj: Indexable = {
  first: 1,
  second: 2,
};

Object.keys(obj).forEach((key: string) => obj[key]);
```
`Indexable`이란 이름의 인터페이스를 정의해준 다음, `string` 타입의 key에 `any` 타입을 지정해줍니다. 이 인터페이스를 통해서 객체를 생성하면 `[]` 표기법을 통해 객체의 프로퍼티에 접근할 수 있습니다.


</br>

## Class interface
인터페이스를 클래스에서도 사용할 수 있습니다. 상위 클래스를 `extends`라는 키워드로 상속하듯이 `implements`라는 키워드로 인터페이스를 구현할 수 있습니다. 클래스는 인터페이스를 `implements`하면서 인터페이스에 명세되어 있는 기능들을 구현해야 하는 의무를 갖게 됩니다.

인터페이스에서는 클래스의 프로퍼티(필드 멤버), 메소드 등을 정의할 수 있습니다. 또한 optional 한 명세 또한 정의할 수 있습니다.
```ts
interface Movable {
  velocity: number;
  move(time: number): Position;
  startPos?: Position;
}

class BMWCar implements Movable {

}
```
여기까지 입력했을 때 나타나는 에러 메세지는 다음과 같습니다. `[!] Class 'Car' incorrectly implements interface 'Movable'.`. 특정 인터페이스를 구현한 클래스는 인터페이스에 정의된 명세를 구현해야 합니다.

즉 여기서는 `velocity`라는 프로퍼티를 포함해야 하며, `move`라는 메소드를 구현해야만 합니다. 이 `BMWCar` 클래스는 생성할 때 다음과 같이 생성할 수 있습니다.
```ts
class BMWCar implements Movable {
  velocity: number;
  constructor(velocity) {
    this.velocity = velocity;
  }
  
  move(time: number): Position {
    //... do something
  }
}

const bmw: Movable = new BMWCar();
```
`BMWCar`라는 타입 말고도 해당 클래스에서 구현한 인터페이스인 `Movable` 타입으로 지정할 수 있습니다.

### Public Property
TypeScript Official Document에 `Interfaces describe the public side of the class, rather than both the public and private side.` 이런 말이 나옵니다. `인터페이스`를 통해 구현해야 함을 명시하는 메소드는 `private` 접근 제어자로 정의될 메소드가 아니라 `public` 접근 제어자로 정의될 메소드이어야 한다고 합니다.

즉, 인터페이스를 통해 명세를 정의할 때는 `private` 속성말고 `public` 속성에 대해 정의합니다.

_end_

</br>

<sup>[(목차로 돌아가기)](#intro-to-typescript-in-15-minutes)</sup>

---
---

</br>

# [TS] 5. Generics in TypeScript
`Generics`는 자바스크립트 개발자에게 친숙하지 않은 용어일꺼라고 생각됩니다. 하지만 정적 타이핑에 있어서 큰 부분을 차지하고 있는 Generics syntax에 대해 알아봅니다.

#### Contents
- Generics?
- Generics to Class
- Generics to Function

</br>

## Generics?
이전에 다뤘던 인터페이스가 어떠한 '틀'을 지정하여 함수 또는 클래스를 정의했다면, 제네릭을 통하여 함수 또는 클래스에 '틀'을 '주입'하여 그 확장성을 보다 높일 수 있습니다. 즉, 제네릭의 목적은 재사용성(reusable)을 높이고 함수나 클래스의 확장성을 높여 중복된 코드를 방지하기 위함이라고 할 수 있습니다.

</br>

## Generics to Class
어떠한 장바구니(Basket)에 물건(Item)을 넣어야 한다고 가정을 해봅시다. 이 때, 하나의 장바구니에는 모두 동일한 타입의 물건이 들어있어야 합니다. 이 경우 모든 경우의 수에 해당하는 장바구니를 각각의 클래스로 만들어줘야 하는 문제점이 발생합니다.
```ts
class BookBasket { ... }
class CupBasket { ... }
class DollBasket { ... }
//...
```
이럴 경우, 제네릭을 사용하여 하나의 클래스에서 타입을 **주입받아** 모든 경우의 수에 해당하는 장바구니를 만들 수 있습니다.
```ts
class Basket<T> {
  private item: T[];

  getItem(index: number): T {
    return this.item[index];
  }
}
```
아주 간단한 `Basket`클래스를 만들어 보았습니다. 일반 클래스를 정의할 때와는 다르게 `<T>`라는 것이 클래스 이름 옆에 추가된 것을 확인하실 수 있는데요, 해당 클래스 내에서 사용할 타입을 `T`라는 값으로 받을 수 있게 되는 것입니다. 여기서 `T`는 별 뜻이 있는게 아니라 Type의 약자입니다. 이렇게 정의한 클래스를 이용하여 여러 경우에 해당하는 장바구니를 생성할 수 있습니다.
```ts
const bookBasket = new Basket<Book>();
const cupBasket = new Basket<Cup>();
const dollBasket = new Basket<Doll>();
```
`new` 키워드를 통해 인스턴스를 생성할 때 정의할 때와 마찬가지로 `<>`와 함께 타입을 지정하여 인스턴스를 생성해줍니다. (`Book`, `Cup`, `Doll`에 대한 인터페이스 정의는 생략합니다.)

</br>

## Generics to Function
제네릭스는 클래스 뿐만 아니라 함수에도 적용될 수 있는데요, 이전에 살펴보았던 AjaxUtils를 예제로 제네릭에 대해 알아보겠습니다.
```ts
export async function fetchData(param: fetchDataParam): Promise<DataFormat> {
    //...
}
```
위 함수는 `fetchDataParam`이라는 타입의 인자만 받을 수 있으며 `Promise<DataFormat>`이라는 타입의 반환만 할 수 있게 설계되어 있습니다. 그러나 함수의 body가 여러 곳에서 중복된다면 `fetchDataParam` 또는 `DataFormat`을 위 함수에 주입하여 여러 상황에 대응할 수 있는 함수로 변환시킬 수 있습니다. 이 때, 제네릭이 사용됩니다.

위 함수에 제네릭을 적용하여 함수의 재사용성을 높여보겠습니다.
```ts
async function fetchDataOf<T, U>(param: T): Promise<U> {
    const response = await fetch(`${param}`);
    return response.json();
}
```
arrow function에도 마찬가지로 적용할 수 있습니다.
```ts
const fetchDataOf = async <T, U>(param: T): Promise<U> => {
    const response = await fetch(`${param}`);
    return response.json();
}
```
이렇게 정의를 하면 다음과 같이 호출할 수 있습니다. 여기서 T와 U는 그냥 type의 종류를 나타내는 character라고 생각하시면 됩니다.

```ts
const data: DataFormat = await fetchDataOf<string, DataFormat>(baseUrl);
```
`param`으로 넘겨주게 되는 인자의 타입이 다르거나, 반환하게 되는 값의 타입이 다를 경우에도 `<>`에 해당 타입을 지정하여 하나의 함수를 사용할 수 있게 됩니다.

물론 <any>의 형식으로도 사용할 수 있지만(이럴바에 차라리 자바스크립트를 사용하는 것이 생산성 측면에서 더 좋을 것 같습니다.) 특정 타입을 받아 해당 타입으로 반환한다는 측면에 있어서 안정성과 확장성 두 마리 토끼를 모두 잡을 수 있습니다.

</br>

### 마무리
실제 개발해서보다는 라이브러리를 개발할 때 많이 사용할 수 있는 Generics에 대해 알아봤습니다.

</br>

---
---

</br>


---
title: '[TS] 6. Decorator'
date: 2018-01-09 09:59:14
categories: TypeScript
tags: ts
thumnails: /images/typescript.png
---
![](/images/typescript.png)

# [TS] 6. Decorators

이번 포스팅에서는 현재 JavaScript에서도 [ts39/proposal stage-2](https://github.com/tc39/proposal-decorators)에 올라와있는 `Decorator`에 대해 알아보겠습니다.

### Table of contents
- Setup
- Intro
- Decorator to method
- Decorator to class
- Decorator with parameter

## Setup
자바스크립트 babel환경에서 데코레이터를 테스트해보기 위해서는 babel 플러그인이 추가적으로 필요합니다.
```bash
$ npm install babel-core babel-plugin-transform-decorators-legacy --save-dev
```
`babel-core`를 기본으로 하며, babel-plugin을 추가적으로 설치해줍니다.
```json .babelrc
{
  //...
  "plugins": ["transform-decorators-legacy"]
}
```
해당 프로젝트의 babel설정을 담고 있는 `.babelrc`파일에 설치한 플러그인을 추가해줍니다. 보다 구체적인 해당 개발환경은 [여기](https://github.com/JaeYeopHan/esnext_labs)를 참고해주세요.

TyeScript에서는 `tsconfig.json`의 `compilerOption`을 다음과 같이 변경해줍니다.
```json tsconfig.json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true
    }
}
```

</br>

## Intro
TypeScript(JavaScript)에서 `@`이라는 character로 사용하는 문법을 `Decorator(데코레이터)`라고 합니다. 자바를 경험해보신 분이라면 `Annotation`인가? 라고 생각하기 쉬운데요, 조금 다릅니다. 데코레이터는 **함수** 라고 할 수 있습니다. 데코레이터는 말 그대로 코드 조각을 장식해주는 역할을 하며 타입스크립트에서는 그 기능을 함수로 구현할 수 있습니다.
 
Decorator는 클래스 선언, 메서드, 접근 제어자, 속성 또는 매개 변수에 첨부 할 수 있는 특별한 종류의 선언입니다. 데코레이터는 `@expression` 형식을 사용하는데, expression은 데코레이팅 된 선언에 대한 정보와 함께 존재하며 이는 **런타임에** 호출됩니다.

#### 참조(reference)
데코레이터는 `@decorator`과 같이 사용할 수 있으며 `@[name]`의 형식일 때 `name`에 해당하는 이름의 함수를 참조하게 됩니다.

#### 실행 시점(execute time)
이렇게 데코레이터로 정의된 함수는 데코레이터가 적용된 메소드가 실행되거나 클래스가 `new`라는 키워드를 통해 인스턴스화 될 때가 아닌 런타임 때 실행됩니다. 즉, 매번 실행되지 않습니다.


_그럼 데코레이터가 메소드에 적용되는 경우, 클래스에 적용되는 경우, 프로퍼티에 적용되는 경우 이렇게 세 가지로 나누어 코드를 살펴보겠습니다._


</br>


## Decorator to Method
> 메소드에 적용되는 경우

우선 데코레이터로 사용할 `chaining`이라는 함수를 정의해줍니다.
```ts decorator.js
function chaining(target: any, key: string, descriptor: PropertyDescriptor): any {
  console.log(target); // {bark: f, constructor: f}
  console.log(key); // bark
  console.log(descriptor); // {value: f, writable: true, enumerable: true, configurable: true}
}
```
위 함수는 추후 메소드에 `@chaining` 형식으로 사용될 함수입니다. `@`과 함께 함수가 호출되는 경우 받게 되는 파라미터는 다음과 같습니다.
> - target : 속성을 정의하고자 하는 객체
> - name : 속성의 이름
> - descriptor : 새로 정의하고자 하는 속성에 대한 설명

이는 `Object.defineProperty()`를 통해 이를 정의하고 있기 때문입니다.

`target`은 해당 메소드가 속해있는 클래스 프로토타입을 가리키게 되며 `Pet`의 프로토타입에는 `constructor`와 `bark`메소드가 있는 것을 확인할 수 있습니다. `name`은 데코레이터가 적용된 메소드의 이름이 됩니다. `descriptor`는 `defineProperty`에서 정의할 수 있는 각각의 속성값들이 됩니다.

Object의 `defineProperty`에 해당하는 보다 자세한 내용은 [여기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)에서 살펴보실 수 있습니다. 그럼 각각을 활용해서 `chaining` 기능을 구현해보겠습니다.

```ts
// Decorator to method
function chaining(target: any, key: string, descriptor: PropertyDescriptor): any {
  const fn: Function = descriptor.value;

  descriptor.value = function(...args: any[]) {
    fn.apply(target, args);
    return target;
  }
}
```
descriptor의 `value`가 데코레이터가 적용된 함수, 즉 실행 대상이라고 할 수 있습니다. `descriptor.value`를 재정의(override)하기 전에 `fn`이라는 변수로 caching해둔 다음, 호출한 후의 일을 정의하기 위해 위와 같이 재정의 해줍니다. 재정의 하기 전 caching 해둔 함수를 호출하기 위해서 `apply` 함수를 사용했습니다. 어떠한 변수가 얼만큼 전달될지 모르니 rest parameter를 통해 `fn`을 호출해주는 코드입니다.

위와 같이 `descriptor.value`가 재정의 되면 `chaining`이 적용된 메소드는 재정의된대로 호출되게 됩니다.

> `apply` 함수에 대한 내용은 [여기](https://www.google.co.kr/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwj9wKqt1cDYAhUBtpQKHQNvC34QFggmMAA&url=https%3A%2F%2Fdeveloper.mozilla.org%2Fko%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FFunction%2Fapply&usg=AOvVaw2tqAzWzgnT49rXId1wsV11)를 참고해주세요.

```ts
class Pet {
  @chaining
  bark() {
  }
}
```
위와 같이 적용해보겠습니다.
```ts
const pet = new Pet();
```
`Pet`클래스에서 `bark`라는 메소드는 `Pet.prototype.bark`로 됩니다. class syntax 내부에서 위 코드에서는 `bark`라는 메소드가 `Pet`의 prototype의 프로퍼티로 추가되기 전에 `decorate` 함수가 실행되어 본래 `bark`라는 메소드에서 정의된 것에 추가적인 **'장식'** 을 더해 prototype에 추가되도록 합니다.

> 만약 compile target이 ES5보다 낮다면 `PropertyDescriptor` 값으로 `undefined`이 전달됩니다.

```ts
pet.bark().bark();
```
위 데코레이터의 효과로 `return this;`를 해주지 않아도 chaining 기능을 사용하여 메소드를 호출할 수 있습니다.

</br>



## Decorator to Class
하지만 데코레이터가 class에 적용되었을 때는 그 signature가 조금 달라집니다. 클래스 데코레이터는 클래스 선언 바로 전에 선언됩니다. 클래스 데코레이터는 클래스 생성자에 적용되며 클래스 정의를 관찰, 수정 또는 대체하는 데 사용할 수 있습니다.

클래스 데코레이터가 값을 반환하면 클래스 선언을 제공된 생성자 함수로 바꿉니다.

```ts
function component(target, name, descriptor) {
  console.log(target); // ...
  console.log(name); // undefined
  console.log(descriptor); //undefined
}
```
메소드에 데코레이터를 적용하듯이 데코레이터 함수를 선언하면 올바른 선언을 할 수 없습니다. 클래스에 적용되는 데코레이터 함수에 전달되는 인자는 `constructor`하나입니다. 제대로 된 데코레이터 선언은 다음과 같습니다.
```ts
function classDecorator<T extends {new(...args:any[]):{}}>(constructor:T) {
  return class extends constructor {
    newProperty = "new property";
  }
}
```
클래스에 적용되는 데코레이터 함수 내에서 새로운 생성자 함수를 반환하면 원래 프로토타입을 유지해야 합니다. 런타임에 데코레이터를 적용하는 로직은 이를 수행하지 않기 때문입니다. 위 코드에서는 기존의 프로토타입을 유지하기 위해 적용되는 클래스의 `constructor`를 `extends` 합니다.

```ts
@classDecorator
class Pet {
  constructor(name: string) {
    this.name = name;
  }
}

const pet = new Pet("async");
console.log(pet.newProperty); // new Property
```
`classDecorator` 데코레이터가 적용된 `Pet` 클래스의 인스턴스에는 `newProperty`가 존재하지 않지만 데코레이터 함수에서 해당 클래스의 constructor를 재정의했기 대문에 `newProperty`에 접근할 수 있습니다.

</br>

## Decorator with parameter
> 파라미터를 받는 데코레이터

데코레이터 함수에 인자를 넘겨줄 수 있습니다. 이 인자는 무엇이든 될 수 있습니다. 예제 코드로 descriptor의 `enumerable` 속성을 변경하는 데코레이터를 만들어보겠습니다.

```ts
function enumerableToFalse(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  descriptor.enumerable = false;
};
```
이렇게 정의하면 `enumerableToFalse`이 적용된 메소드의 enumerable 속성은 false가 됩니다. 위 `enumerableToFalse` 함수를 한 번 감싸서 반환하는 함수를 만들면 다음과 같습니다.

```ts
function enumerable(value: boolean) {
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    descriptor.enumerable = value;
  };
}
```
이제 이 함수를 데코레이터 함수로 사용할 수 있습니다. `value`에 해당하는 값으로 데코레이터를 적용하는 메소드의 `enumerable`속성을 제어할 수 있습니다.

```ts
class Pet {
  @enumerable(false)
  bark() {
  //...
  }
}
```
위와 같이 `false`라는 인자를 받는 데코레이터를 정의했습니다. 저 인자에는 함수도 들어갈 수 있으며 데코레이터도 들어갈 수 있습니다.


### 마무리
target을 ES5로 지정해야 제대로 된 데코레이터를 사용할 수 있어서 아직 한계가 있는 Decorator지만 React에서는 HOC(High-Order-Component)에 많이 사용하고 있는 Decorator 였습니다!


</br>

<sup>[(목차로 돌아가기)](#intro-to-typescript-in-15-minutes)</sup>

---
---

</br>

# [TS] 7. Typescript's Type System

### Table of Contents
- TypeScript의 Type Checking System
- Type Inference
- Type Assertion
- Type Guards
- Type Compatibility

</br>

## TypeScript의 Typing Checking System
`TypeScript`에서의 Type System에 대한 이해를 하기 전, 기존 프로그래밍 언어의 큰 두 축인 정적 언어와 동적 언어에 대한 정의를 다시 한 번 살펴볼 필요가 있습니다.

### 정적언어 (Static Language)
- 변수(variables) 또는 함수(function)의 `Type`을 **미리** 지정해야 한다.
- 컴파일되는 시점에 Type Check를 수행한다.

### 동적 언어 (Dynamic Language)
- 변수(variables) 또는 함수(function)의 `Type`을 지정하지 않는다.
- Type Check는 런타임(runtime) 환경에서나 알 수 있다.

### Duck Typing
덕 타이핑(Duck Typing)이라고 많이 들어보셨을 텐데요, 현재 이 덕 타이핑 체계를 기반으로 동적 언어에 타입을 추론하는 언어는 GoLang과 Python 등이 있습니다. 하지만 TypeScript는 이 덕타이핑과는 조금 다른 체계로 **Typing**을 하고 있습니다.
> 덕 타이핑에 대한 보다 자세한 내용은 [여기](https://www.google.co.kr/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&ved=0ahUKEwjj5dzEl8PYAhXCNJQKHfG9DdYQFggxMAE&url=http%3A%2F%2Fwww.popit.kr%2Fgolang%25EC%259C%25BC%25EB%25A1%259C-%25EB%25A7%258C%25EB%2582%2598%25EB%25B3%25B4%25EB%258A%2594-duck-typing%2F&usg=AOvVaw1dXNKFpMsVofdtJ-QOglzu)를 참고해주세요.

### Structural typing
TypeScript는 **Structural typing** (구조적 타이핑)을 기반으로 타입 시스템을 갖추고 있습니다. 구조적 타이핑이란, `멤버`에 따라 타입을 **연관짓는** 방법을 말합니다. 구조적 타이핑과 반대인 방법으로 `nominal typing`이 있습니다. 우리가 알고 있는 일반적인 정적 언어인 C#, Java는 이 `nominal typing` 방식으로 **type checking**이 이루어집니다.

_Official Document_에 나온 예제를 통해 설명드립니다.
```ts
interface Named {
  name: string;
}

class Person {  
  name: string;
}

let p: Named;
p = new Person(); // Ok, because of structural typing
```
위 코드를 C# 또는 Java의 문법에 맞게 변경한다면 동작하지 않는 잘못된 코드가 됩니다. 하지만 TypeScript에서는 정상적으로 동작합니다. `Named`와 `Person` 두 가지는 오로지 `name`이라는 프로퍼티(or 멤버)만 갖고 있는 타입이므로 서로 `compatibility` 하다고 볼 수 있습니다. 때문에 위 코드는 문제되지 않습니다. (compatibility에 대해서는 뒤에서 알아봅니다.)

</br>

## Type Inference
'누가봐도 이 변수는 이 타입이다.'라는 것에 대해 TypeScript가 지원해주는 것을 **타입 추론** 이라고 합니다.
```ts
let name = `jbee`;
```
`name`이라는 변수는 `string` 타입의 변수가 됩니다. 그렇기 때문에 굳이 `: string`이라고 타입을 지정해주지 않아도 다음과 같이 에러가 발생합니다.
```ts
name = 1; // Error: Type '1' is not assignable to type 'string'.
```
그렇다면 다음과 같은 경우에는 어떻게 추론될까요?
```ts
const mixedArr = [0, 1, `jbee`];
```
`number`에 해당되는 value와 `string`에 해당하는 value가 공존하기 때문에 위 코드에서 `mixedArr`은 `(number | string)[]`의 타입을 갖게 됩니다. 이렇게 여러 타입이 공존하는 경우에 추론하여 지정되는 타입을 **Best common type**이라고 합니다.

</br>


## Type Assertion
이 변수의 타입은 분명 `A`인데 TypeScript에서 보수적으로 처리하여 에러를 발생시키는 경우가 있습니다. 이럴 경우 해당 변수를 `A`라고 명시하여 에러를 사라지게 할 수 있습니다.
```ts
export type Todo = {
  id: number;
  text: string;
  completed: boolean;
};

export type Todos = Todo[];
```
위와 같이 `Todo` 타입과 `Todos` 타입을 지정한 상황이라고 했을 때를 예로 들어보겠습니다.

```ts
const initialState: Todos = [
  {
    id: 0,
    text: 'Study RxJS',
    completed: false,
  }
];
```
위 코드에서 `Todos`에 해당하는 타입을 제대로 지정해줬지만 뭔가 아쉬움이 남을 수 있는데요, 이 때 두 가지 방법을 사용할 수 있습니다.

```ts
const initialState: Todos = [
  <TODO>{
    id: 0,
    text: 'Study RxJS',
    completed: false,
  }
];
```
위 코드처럼 `<>`을 사용하거나
```ts
const initialState: Todos = [
  {
    id: 0,
    text: 'Study RxJS',
    completed: false,
  } as Todo
];
```
위 코드처럼 `as` 키워드를 사용할 수 있습니다. 두 가지 모두 동일하지만 `tsx`와 함께 사용하기 위해서는 `as` 키워드를 사용하는 것이 좋습니다.

> [!] 이 Type Assertion은 Type Casting과는 다릅니다. 자세한 내용은 [DailyEngineering - 타입 추론과 타입 단언](https://hyunseob.github.io/2017/12/12/typescript-type-inteference-and-type-assertion/)을 참고해주세요!


</br>

## Type Guards
자바스크립트에서는 `typeof` 또는 `instanceof`와 같은 오퍼레이터가 타입을 확인해주는 역할을 했었습니다.
```ts
if (typeof this.state[key] !== typeof newData) {
    return ;
}
```
하지만 이 방법은 런타임에서 타입 체크를 수행하게 됩니다. 따라서 컴파일 시점에서는 올바른 타입인지 알 수 없습니다. TypeScript에서는 컴파일 시점에서 타입 체크를 수행할 수 있도록 `Type Guard`를 지원합니다.

</br>

### `typeof`, `instanceof`
TypeScript에서도 마찬가지로 `typeof`와 `instanceof` 오퍼레이터를 지원합니다.
```ts
function setNumberOrString(x: number | string) {
  if (typeof x === 'string') {
    console.log(x.subtr(1)); // Error
    console.log(x.substr(1)); // OK
  } else {
    console.log(typof x); // number
  }
  x.substr(1); // Error
}
```
위 코드에서 `if-block`내에서의 `x`변수의 타입은 `string`일 수 밖에 없다는 것을 컴파일 시점에 체크하여 transpiler가 Error를 발생시키는 경우입니다. 이 예제와 비슷한 방법으로 `instanceof` 오퍼레이터를 사용할 수 있습니다. `instanceof`는 클래스를 기반으로 생성된 인스턴스의 타입을 판단하는데 사용됩니다.

```ts
class Pet {
  name = 123;
  common = '123';
}

class Basket {
  size = 123;
  common = '123';
}

function create(arg: Pet | Basket) {
  if (arg instanceof Pet) {
    console.log(arg.name); // OK
    console.log(arg.size); // Error!
  }
  if (arg instanceof Basket) {
    console.log(arg.name); // Error!
    console.log(arg.size); // OK
  }

  console.log(arg.common); // OK
  console.log(arg.name); // Error!
  console.log(arg.size); // Error!
}
```
`typeof`와 마찬가지로 `instanceof`로 필터링 된 block 내부에서 **Type checking**이 이루어집니다.

</br>

### `in`
```ts
interface A {
  x: number;
}
interface B {
  y: string;
}

function execute(q: A | B) {
  if ('x' in q) {
    // q: A
  } else {
    // q: B
  }
}
```
`A` 또는 `B`를 유니온 타입으로 받을 수 있는 `execute` 함수 내에서 각각에 대해 다른 처리를 할 경우, `in`이라는 오퍼레이터를 사용할 수 있습니다. 해당 오퍼레이터는 check하고자 하는 타입에 해당 프로퍼티가 존재하는지의 유무를 판단할 수 있습니다.

</br>

### `.kind` Literal Type Guard
사용자에 의해 `type`으로 정의된 타입에 대해서, 즉 TypeScript 내부에서 지원하는 primitive type이 아닌 **사용자 정의 타입에 대해서 타입 검사를 수행할 때**, `.kind`를 사용할 수 있습니다. 
```ts
type Foo = {
  kind: 'foo', // Literal type 
  foo: number
}
type Bar = {
  kind: 'bar', // Literal type 
  bar: number
}

function execute(arg: Foo | Bar) {
  if (arg.kind === 'foo') {
    console.log(arg.foo); // OK
    console.log(arg.bar); // Error!
  }
}
```
위 코드에서 처럼 `type` 키워드를 사용하여 별도 타입을 지정할 때, `kind`라는 프로퍼티를 추가하여 타입 검사를 수행할 수 있습니다.

</br>

### User Defined Type Guards
메소드를 별도로 분리하여 Type Guard를 지정할 수 있습니다. 아까 지정한 interface A로 만들어보겠습니다.
```ts
interface A {
  x: number;
}
// Define Type Guard
function isA(arg: any): arg is A {
  return arg.x !== undefined;
}
```
`arg is A`라는 타입으로 `isA` 메소드가 Type Guard의 역할을 수행한다는 것을 명시할 수 있습니다. if 내부에 들어가는 로직을 별도로 추출하여 보다 가독서이 좋은 코드를 작성할 수 있습니다.

</br>


## Type Compatibility
한국어로 번역하게 되면 **타입 호환성** 정도로 할 수 있겠는데요, TypeScript는 위에서 언급했듯이 **Structural subtyping**을 기반으로 Type checking을 하기 때문에 이를 기반으로 타입 간의 호환성을 고려할 수 있습니다.

#### 1. Comparing two Objects
```ts
let x = {name: `Jbee`};
let y = {name: `James`, age: 34};

x = y; // OK!
y = x; // Error!
```
위 코드에서 `x`는 `{name: string}` 타입으로 추론되며, `y`는 `{name: string, age: number}`로 추론됩니다. 이 경우 `x = y`는 `y`에 `name`이라는 속성이 있으므로 가능하지만 `y = x`의 경우, `x`에는 `age`라는 속성이 없으므로 에러가 발생합니다.

#### 2. Comparing two functions
```ts
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; // OK
x = y; // Error
```
함수일 경우에는 객체인 경우와 조금 다른 것을 볼 수 있습니다. 위 코드에서 두 `x`, `y`함수는 parameter 만 다르게 정의되어 있습니다. 이 경우, `x` 함수에 전달할 수 있는 parameter의 경우의 수가 y에 모두 해당하므로 `y = x`가 정상적으로 동작합니다. 하지만 그 반대인 `x = y`는 `y` 함수에 전달할 수 있는 parameter를 `x`가 모두 포용할 수 없으므로 에러가 발생합니다.
```ts
let x = () => ({name: `Jbee`});
let y = () => ({name: `James`, age: 34});

x = y; // OK!
y = x; // Error!
```
이번에는 return value의 type이 다른 경우입니다. 이 경우에는 함수의 경우를 따르지 않고 객체인 경우를 따르게 됩니다.

## 마무리
TypeScript의 단순한 문법을 조금 넘어서 어떻게 Type Checking이 이루어지는지 살펴봤습니다.
감사합니다.


</br>

<sup>[(목차로 돌아가기)](#intro-to-typescript-in-15-minutes)</sup>

---
---

</br>

### Reference
- [TypeScript Deep Dive](https://www.gitbook.com/book/basarat/typescript)
- [TypeScript Official Document - Basic Types](https://www.typescriptlang.org/docs/handbook/basic-types.html)
- [TypeScript Official Document - Function](https://www.typescriptlang.org/docs/handbook/functions.html)
- [TypeScript Official Document - Interface](https://www.typescriptlang.org/docs/handbook/interfaces.html)
- [TypeScript Official Document - Generics](https://www.typescriptlang.org/docs/handbook/generics.html)
- [TypeScript Official Document - Class](https://www.typescriptlang.org/docs/handbook/classes.html)
- [TypeScript Official Document - Generics](https://www.typescriptlang.org/docs/handbook/generics.html)
- https://github.com/wycats/javascript-decorators
- https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841
- https://www.sitepoint.com/javascript-decorators-what-they-are/
- https://cabbageapps.com/fell-love-js-decorators/
- https://javarouka.github.io/blog/2016/09/30/decorator-exploring/#class-il-gyeongu
- https://github.com/jayphelps/core-decorators
- [TypeScript Official Document - Type Inference](https://www.typescriptlang.org/docs/handbook/type-inference.html)
- [TypeScript Official Document - Type Compatibility](https://www.typescriptlang.org/docs/handbook/type-compatibility.html)
- [Golang으로 만나보는 duck typing](http://www.popit.kr/golang%EC%9C%BC%EB%A1%9C-%EB%A7%8C%EB%82%98%EB%B3%B4%EB%8A%94-duck-typing/)
- [Type Systems: Structural vs Nominal typing explained](https://medium.com/@thejameskyle/type-systems-structural-vs-nominal-typing-explained-56511dd969f4)
- [TypeScript Deep dive - Type Guard](https://basarat.gitbooks.io/typescript/docs/types/typeGuard.html)

</br>

## Bookmarks
* [TypeScript 개발환경 세팅하기](https://github.com/JaeYeopHan/typescript_playground)
* [0. Quick Start](https://github.com/JaeYeopHan/typescript_tutorial_docs/blob/master/00_quick_start.md)
* [1. Basic Types](https://github.com/JaeYeopHan/typescript_tutorial_docs/blob/master/01_basic_types.md)
* [2. Class](https://github.com/JaeYeopHan/typescript_tutorial_docs/blob/master/02_class.md)
* [3. Function](https://github.com/JaeYeopHan/typescript_tutorial_docs/blob/master/03_Function_in_TypeScript.md)
* [4. Interface](https://github.com/JaeYeopHan/typescript_tutorial_docs/blob/master/04_interface.md)
* [5. Generics](https://github.com/JaeYeopHan/intro_to_typescript/blob/master/05_generics.md)
* [6. Decorators](https://github.com/JaeYeopHan/intro_to_typescript/blob/master/06_decorators.md)
* [7. TypeScript's type system](https://github.com/JaeYeopHan/intro_to_typescript/blob/master/07_type_system.md)

## Author
[Jbee](http://friendly-belief.surge.sh/)

## LICENSE
<a rel="license" href="http://creativecommons.org/licenses/by/4.0/"><img alt="크리에이티브 커먼즈 라이선스" style="border-width:0" src="https://i.creativecommons.org/l/by/4.0/88x31.png" /></a>
