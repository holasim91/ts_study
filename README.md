# 기본 문법 익히기

## 2.1 변수, 매개변수, 반환값에 타입을 붙이면 된다.
TS를 사용할 때 어떤 값에 타입을 부여할지 알아야한다.
기본적으로 변수, 함수의 매개변수와 리턴값에 타입을 부여한다.
이런 행위를 타이핑(typing) 이라고 표현한다.
기본 타입으로는 
* string(문자)
* number(숫자)
* boolean
* null
* undefined
* symbol
* bigint
* object(객체)
이렇게 모두 JS의 자료형과 대응이 된다. 즉, 함수와 배열도 object타입 이라는 것.

타입을 표기할 땐 아래와 같이 변수 뒤에 콜론을 붙여서 표시해준다.
```ts
const str:string = 'hello';
const num:number = 1;
const bool:boolean = true;
const n:null= null;
const u:undefined = undefined;
const sym:symbol = Symbol('sym');
const bigI:bigint = 10000000n;
const obj:object = {hola:'world'};
```
함수에서 타이핑을 할 땐 아래와 같이 파라미터가 있다면 파라미터 뒤에 콜론을 붙여서 타이핑을 하고, 리턴 값의 타입은 파라미터 소괄호 뒤에 표기한다.
```ts
function plus(x:number,y:number):number{
    return x+y
};

const minos = (x:number,y:number):number => x-y;
```
## 2.2 타입 추론을 적극 활용하자.
위의 함수를 다시 보자.
```ts
function plus(x:number,y:number):number{
    return x+y
};
const result1:number = plus(1,2);
const result2=plus(3,4) //const result2: number

function plus2(x:number,y:number){
    return x+y
};
/*
* function plus2(x: number,y: number): number
* */
```
두 매개변수의 타입이 숫자형이고, 해당 숫자를 더하는 연산을 리턴한다면 당연히 해당 값도 숫자형일 것이다.
이럴때 굳이 리턴값에 타입을 명시적으로 선언해주지 않아도 TS는 이미 타입이 어떤건지 알고있다.

그러나 매개변수에는 반드시 타입을 부여해야한다. 
왜? 뭐가 들어올지 모르기 때문.
그러나 항상 TS가 옳은 추론을 하는게 아니다. 그렇다면 어떻게해야 타입 추론을 잘 활용할 수 있을까?

**TS가 타입을 제대로 추론한다면 그대로 쓰고, 틀리면 그때 타입을 표기하자.**

아래의 예를 보자.
```ts
const str = 'hello' //const str: "hello" -> 마우스 오버를 했을 때 추론되는 타입
const num = 123 //const num: 123
const bool = true; //const bool: true
const n= null; //const n: null
const u = undefined;//const u: undefined
const sym = Symbol('sym'); //const sym: unique symbol
const bigI = 10000000n; //const bigI: 10000000n
const obj = {hola:'world'}; //const obj: { hola: string }
```
위의 예시는 맨 처음 있던 예에서 타입 선언을 뺀 것이다. 
각 변수들에 마우스오버를 하면 처음 예시와 다른 타입들로 추론한다는 것을 알 수 있다.
하지만 이 타입들이 좀 더 **정확한** 타입이다.

여기서 두 가지를 알고 넘어가자.
1. 타입을 표기할 땐 'hello', 123, false같은 정확한 값을 입력 할 수 있고, 이런 타입을 **리터럴 타입** 이라고 부른다.
2. 타입을 표기할 때 더 넓은 타입으로 표기해도 문제가 되지 않는다. 예를 들면...
```ts
const str1:'hello' = 'hello'
const str2:string = 'hello'
const str3:{} = 'hello' // {}타입은 null과 undefined을 제외한 모든 타입을 의미
```
이렇게 타입을 선언해도 전혀 문제가 없다. 하지만 이 3개의 변수에서 가장 정확한 타입은 첫 번째 변수이고 나머지 것들은 개발자가 조금 부정확한 타입을 쓴 것이다.
TS가 추론해준 타입을 쓰지 않는다면 위의 같은 실수가 발생 할 수 있기에

**TS가 타입을 제대로 추론한다면 그대로 쓰고, 틀리면 그때 타입을 표기하자.**

라는 원칙을 세우고 코딩을 하는게 좋다.

const 대신 let을 사용하는 경우도 한 번 보고 넘어가자.
```ts
let str = 'hello' //let str: string -> 마우스 오버를 했을 때 추론되는 타입
let num = 123 //let num: number
let bool = true; //let bool: boolean
let n= null; //let n: any
let u = undefined;//let u: any
let sym = Symbol('sym'); //let sym: symbol
let bigI = 10000000n; //let bigI: bigint
let obj = {hola:'world'}; //let obj: { hola: string }
```
보다시피 let을 사용하게 되면 const와 전혀 다르게 타입이 추론된다.
왜냐하면 let으로 선언된 변수는 다른 값을 대입할 수 있어서 타입을 넓게 추론 하는 것이다.
이런 현상을 **타입 넓히기** 라고 부른다.  
다른 값을 대입하더라도 데이터 타입을 바꾸는 경우는 많지 않기에 처음 입력한 값을 기준으로 타입을 추론한다.  
또 독특한 현상이 몇 가지가 있다. **null, undefined일 때는 any로 추론된다**는 것.  
그리고 symbol타입일 때 const는 typeof sym이었는데 symbol로 추론 된다는 점. typeof sym은 고유한 symbol을 의미하고, 이를 unique symbol이라고 표현한다.  
unique symbol일때는 서로 비교를 할 수 없지만, 일반 symbol과는 비교가 가능하다. (해당 이슈는 JS에선 전혀 문제가 없다!)
```ts
const sym1 = Symbol.for('sym') //unique symbol
const sym2 = Symbol.for('sym') //unique symbol
let sym3 = Symbol.for('sym')
let sym4 = Symbol.for('sym')

if(sym1 === sym2){} // This comparison appears to be unintentional because the types 'typeof sym1' and 'typeof sym2' have no overlap.
if(sym3 === sym4){}
if(sym2 === sym3){}
```
obj는 const이나 let일때 모두 같은 타입으로 추론이 된다. 이 두 키워드로 선언한 객체의 요소는 변경할 수가 있기 때문에 string으로 추론한 것이다.

## 2.3 값 자체가 타입인 리터럴 타입이 있다.
리터럴 타입은 타입 자리에 리터럴 값을 표기하면 된다.
```ts
let str:'hello' = "hello"
str = 'world' // Type '"world"' is not assignable to type '"hello"'
```
JS에선 변수를 let으로 선언하면 어떤 값이든 자유롭게 대입할 수 있었지만, TS에서는 타입과 일치하지 않는 값만 대입할 수 있다.  
어떻게 보면 자유도를 제한받는다라고 생각이 들지만, 그 대신 안정성을 챙길 수 있다.  
사실 위의 예처럼 let은 원시 자료형에 다한 리터럴 타입을 표기하는 경우는 거의 없다. 그 목적을 위해 const가 있는 것이다.
```ts
const str = 'hello'; //const str: "hello"
```
요렇게만 하면 TS가 알아서 'hello' 리터럴 타입으로 간단하게 표기를 할 수 있다.
그러나 자료형 타입은 let과 자주 사용 된다.

원시 자료형에 대한 리터럴 타입 외에서 객체를 표시하는 리터럴 타입도 있다.
 ```ts
const obj:{name:'sim'} = {name:'sim'}
const arr:[1,2,'three'] = [1,2, 'three']
const func:(amount:number, unit:string) => string 
      = (amount, unit) => amount + unit
```
obj, arr, func 위 세개의 변수의 타입은 순서대로 객체, 배열, 함수를 표시하는 리터럴 타입이다. 함수 리터럴 타입의 반환값 타입의 표기법은 콜론 대신에 =>를 사용한다. 함수 리터럴 타입에 매개변수와 반환값의 타입을 표기했으므로 실제 값에서는 타입 표기를 생략해도 된다.

2.2에서 잠깐 언급을 했지만 객체 리터럴 타입을 사용할 때 한 가지 주의해야할 점이 있다.
```ts
const obj = {name:'sim'} // const obj: {name: string;}
const arr = [1,2, 'three'] // const arr: (string | number)[]
// 다음에 배울 것이긴 한데 (string | number)[]는 문자열 혹은 숫자의 배열 이라는 뜻이다.
```
JS에서 const로 선언된 객체라도 수정이 가능하므로, TS는 그 가능성을 염두하고 타입을 넓게 추론한 것이다.  
만약 값이 수정되지 않는게 확실하다면 as const라는 접미사를 붙이면 된다.
```ts
const obj = {name:'sim'} as const // const obj: { readonly name: string;}
const arr = [1,2, 'three'] as const // const arr: readonly (string | number)[]
obj.name = 'kang' // Cannot assign to 'name' because it is a read-only property.
arr.push(4) // Property 'push' does not exist on type 'readonly [1, 2, "three"]'.
```
이렇게 타입이 고정되어서 추론되는걸 볼 수 있다. obj의 속성 앞과 arr배열에 readonly라는 수식어가 붙어있는데 해당 수식어가 붙으면 해당 값은 변경 할 수가 없다.

## 2.4 배열 말고 튜플도 있다.
먼저 배열의 타입을 간단히 표기하는 방법부터 알아보자.
```ts
const arr1:string[] = ['a','b','c']
const arr2:Array<number> = [1,2,3]
arr1.push(1) //Argument of type 'number' is not assignable to parameter of type 'string'.
```
배열은 끝이 없는 값들이 들어갈 수 있는데 그 것들을 하나하나 타이핑 해주는 것은 불가능하다.
그래서 위의 예시와 같이 타입[] 혹은 Array<타입>으로 타이핑을 한다. 여기서 타입은 요소의 타입을 의미하고, 다른 타입은 요소가 될 수 없다.
<>표기법은 제네릭이라고 부른다.(나중에 자세히 알아볼 예정)

TS는 배열을 추론할 때 요소들의 타입을 토대로 추론한다. 빈배열은 any[]로 추론되니 주의하자.
 ```ts
const arr3 = [1, 3, 5] //const arr3: number[]
const arr4 = [1,'3', 5] //const arr4: (string | number)[]
const arr5 = [] //const arr5: any[] -> 이 또한 변화를 줄 수 있으니 any로 추론되는것인가..?
```
arr3은 모두 숫자형 데이터이므로 number로 추론이 되고, arr4는 정수형과 문자형 자료형이 있어서 (string|number)[]로 추론한다.

그러나 이 추론에도 한계가 있다.
```ts
const array = [123, 4, 5]
array[3].toFixed() // array[3]이 undefined임에도 불구하고 에러가 없음
```
위 예시의 주석에 써있다싶이 array[3]이 undefined임에도 불구하고 에러가 발생하지 않는다. 원인은 array가 number[]로 추론되면서 array[3]도 숫자형으로 추론되기 때문.  
이럴때 **튜플**을 사용하면 해결 할 수 있다.  
각 요소 자리에 타입이 고정되어 있는 배열을 튜플이라고 부른다.
 ```ts
const tuple:[number, string, boolean] = [1, 'a', true]
tuple[0] = 3
tuple[1] = false // Type 'boolean' is not assignable to type 'string'.
tuple[3] = 'no'
/*
 * Type '"no"' is not assignable to type 'undefined'.
 * Tuple type '[number, string, boolean]' of length '3' has no element at index '3'.
 */
tuple.push('YES')
```
[]안에 정확한 타입을 하나씩 입력하면 되고, 표기하지 않은 자리는 undefined타입이 된다.  
tuple[0]은 숫자형만, [1]은 문자형만, [2]는 불린형만 가능하고, 그 뒤로는 undefined타입이 된다.

그런데 이상하게도 push, pop, unshift, shift 메서드를 통해 배열에 요소를 추가, 제거하는 것은 막지 않는다.
물론 해당 인덱스에 접근을 할 수 없으니 의미가 없는 짓이긴 하지만, 이것 조차 막으려면 readonly 수식어를 붙여줘야한다.
 ```ts
const tuple:readonly [number, string, boolean] = [1, 'a', true]
tuple.push(4) // Property 'push' does not exist on type 'readonly [number, string, boolean]'.

// 따라서 예시였던 아래의 코드를 아래와 같이 수정하면 된다.
const array:[number, number, number] = [123, 4, 5]
array[3].toFixed() //Object is possibly 'undefined'.
```
이렇듯 배열보다 정교한 타입검사를 원하면 튜플을 사용하면 된다.  

튜플은 각 요소 자리에 타입이 고정되어있는 것이지 길이는 가변적이다. 그럴때 타입을 어떻게 써줘야할까?
 ```ts
const strNumBools:[string, number, ...boolean[]] = ['hi', 1, true, true, true]
const strNumsBool:[string, ...number[], boolean] = ['hi', 1, 2, 3, 4, 5, false]
const strsNumBool:[...string[], number, boolean] = ['a', 'b', 'c', 100, true]
```
위와 같이 ...타입[]를 스프레드 문법을 사용하여 특정 타입이 연달아 나올수 있음을 알리면 된다.  
심지어 스프레드 문법을 사용을 해도 TS는 타입 추론이 가능하다.
```ts
const arr1 = ['hi', false]
const arr = [33, ...arr1] //const arr: (string | number | boolean)[]
```
구조분해 할당에서는 나머지 속성 문법(...rest)을 사용할 수있는데, 이 경우에도 TS가 추론을 한다.
```ts
const [a, ...rest1] = ['hi', 1, 23, 3434] 
//const a: string 
//const rest1: [number, number, number]
// 명시적 타이핑
const [b, ...rest2]:[string, ...number[]] = ['bye', 1, 2, 3]
// const b: string
// const rest2: number[]
```
또 하나 특별한 표기가 있다.
```ts
let tuple:[number, boolean?, string?] = [1, false, 'hi']
tuple = [3, true]
tuple = [5]
tuple = [7, 'no'] //Type 'string' is not assignable to type 'boolean | undefined'.
```
타입 뒤에 ?를 붙이는 것이다. 이는 옵셔널 수식어로, 해당 자리에 값이 있어도 그만, 없어도 그만이라는 뜻이다.  
따라서 ?이 없었다면 길이와 타입이 고정이 되어야하는데 틀린 타입을 넣지 않는 이상 에러가 발생하지 않는다.(undefiend 제외)

## 2.5 타입으로 쓸 수 있는 것을 구분하자
타입을 공부하다보면 값(value)과 타입이 헷갈릴 수 있다. 개인적으로 리터럴 타입 때문에 어떤 값을 타입으로 사용할수 있을지 없을지 헷갈리고, 타입을 값으로도 사용할 수 있는지도 헷갈린다.  
값은 일반적으로 JS에서 사용하는 값을 가리키고, 타입은 타입을 위한 구문에서 사용하는 타입을 말한다.
타입을 값으로는 사용할 수 없기에 타입으로 사용할 수 있는 값과 타입으로 사용할 수 없는 값만 구분하면 된다.

이전 절에서 봤듯이 대부분의 리터럴 값은 타입으로 사용할 수 있다. 반대로 변수의 이름은 타입으로 사용할 수 없다. 다만 Date, Math, Error, String, Object, Number, Boolean과 같은 내장객체는 타입으로 사용할 수 있다.
```ts
const date:Date = new Date()
const math:Math = Math;
const st:String= 'hi'
```
그러나 String, Object, Number, Boolean, Symbol을 타입으로 사용하는건 지양하는게 좋다.
```ts
function add(x: Number, y:Number){ return x+y } // Operator '+' cannot be applied to types 'Number' and 'Number'.
const str1:String = 'hi'
const str2:string = str1 // Type 'String' is not assignable to type 'string'. 'string' is a primitive, but 'String' is a wrapper object. Prefer using 'string' when possible.
const obj:Object = 'What?'
```
이렇게 예시에 나와있듯이 Number간에는 연산자를 사용할 수 없고, string에 String타입을 대입할 수 없다. 마지막으로 obj변수는 Object임에도 불구하고 문자열이 대입 가능하다.
따라서 string, number, boolean, object, symbol로 통일해서 쓰자.

만약 헷갈린다면 일단 타입으로 표기해보자.
```ts
function add(x:number,y:number){return x + y}
const add2:add = (x:number, y:number) => x + y 
// 'add' refers to a value, but is being used as a type here. Did you mean 'typeof add'?
// const add2:typeof add= (x:number, y:number) => x + y 로 수정
```
이렇게 TS가 친절히 에러메시지로 알려준다.  
위의 예시같은 경우는 add는 값이지만 타입으로 사용했다는 뜻이다. 그리고 typeof를 앞에 붙여서 타입으로 사용할 수 있다는 뜻이다.

그러나 함수의 호출을 타입으로 사용할 수 없다. 함수의 반환값을 타입으로 사용하고 싶을 때 아래와 같이 코드를 작성하면 에러가 발생한다.
```ts
function add(x:number, y:number){return x + y};
const result1: add(1, 2) = add(1, 2)
/*
 * 'add' refers to a value, but is being used as a type here. Did you mean 'typeof add'?
 * The left-hand side of an assignment expression must be a variable or a property access.(2364)
 */

const result2: typeof add(1, 2) = add(1,2)
/*
 * The left-hand side of an assignment expression must be a variable or a property access.(2364)
 */
```
에러메시지에서는 add는 타입이 아니라 값이고, = 연산자의 왼쪽은 병수이거나 속성 접근(property access)만 된다고 한다.  
즉 obj = 'abc' 혹은 obj.x = 'abc' 같은 것만 가능하다는 뜻이다. 지금 같은 상횡에선 = 연산자 왼쪽에 add(1,2)같은 함수 호출이 올 수 없다는 얘기다.  
(함수의 반환값을 타입으로 사용하는건 뒤에가서 배울 것이다.)

클래스는 조금 다르다. 클래스의 이름은 typeof 없이도 타입으로 사용할 수 있다.
```ts
class Person{
    name: string;
    constructor(name: string){
        this.name = name
    }
}

const person:Person = new Person('lucifer')
```

