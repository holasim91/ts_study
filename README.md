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
