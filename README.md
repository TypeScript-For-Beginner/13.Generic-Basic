# 013.Generic-Basic

# 김민영 13. 제네릭 기초

## 제네릭이란 무엇일까?

```TypeScript
function helloString(message: string): string {
	return message;
}
```
위 함수는 인자로 string 타입의 message를 받으며<br>
return 타입도 string이다.<br>

```TypeScript
function helloNumber(message: number): number {
	return message;
}
```
위 함수는 인자로 number 타입의 message를 받으며<br>
return 타입도 number이다.<br>

위 함수들은 각각 일정한 타입을 인자로 사용하고 return으로 받으며 반복되고 있다.<br>
그리고 더 많은 반복이 일어날 수 있고, 반복적인 함수가 생길 수 있다.<br>

그래서 우리는 모든 타입을 받을 수 있고 return할 수 있는 any 를 사용했다.<br>
하지만 any를 사용하게 되면 우리의 의도와는 다르게 다른 결과를 가져온다. <br>

```TypeScript
function hello(message: any): any {
	return message;
}
```

위 함수는 any를 사용한 범용적인 함수이다.<br>

```TypeScript
console.log(hello("hey"));
console.log(hello(30));
```
그렇다면 문자열, 숫자 모두 사용가능하다.<br>
하지만, length() 라는 문자열 메서드를 사용한다고 가정해보자.<br>
숫자형에서 length()는 사용할 수 없지만 any를 사용할 경우, <br>
컴파일 타임에는 에러가 발생하지 않지만 런타임에는 undefined가 나올 것이다.<br>

```TypeScript
console.log(hello("hey").length); // 3
console.log(hello(30).length); // undefined
```
<br><br>
![](https://velog.velcdn.com/images/minyoungdumb/post/19b79ad5-2891-465f-9268-cda2b24c047f/image.png)<br><br>

이런 문제를 해결하기 위해 인자로 들어가는 타입을 변수로 활용해서 return되는 타입에 연관을 시켜주면 어떨까? 이 아이디어에서 출발한 것이 generic이다.<br>

## 제네릭 기초

그렇다면 제네릭을 어떻게 사용하는지 한 번 알아보자.<br>

```TypeScript
function helloGeneric<T>
```
홀화살괄호를 사용하고 Type의 T를 사용해 아래와 같이 사용한다.<br>
```TypeScript
function helloGeneric<T>(message: T)
```
message라고 받은 인자를 T라고 지정한다.<br>
그렇게 되면 helloGeneric이라는 함수 내에서 T라고 하는 것을 기억하게 된다.<br>
그리고 나서 예를 들어,<br>
```
console.log(hello("hey"));
```
위 콘솔에 문자열을 넣게 되면, T가 문자열로 지정되게 된다.<br>
```TypeScript
function helloGeneric<T>(message: T): T {
	return message;
}
```
그리고 return값을 T로 지정하면 된다. 즉, T의 타입에 따라 함수의 타입이 결정된다.<br>
콘솔을 찍어보자.<br>

```
console.log(helloGeneric('hey'));
```
그러면 helloGeneric이라는 함수는 인자가 문자열로 왔기에 리터럴 타입을 보고 문자열 리터럴 타입을 가지는 함수가 된다.<br><br>
![](https://velog.velcdn.com/images/minyoungdumb/post/78c7bf1e-2318-411b-97d3-a3fa4e1f4757/image.png)<br><br>

```
console.log(helloGeneric('hey').length);
```
length는 문자열의 길이 즉, 숫자형 타입을 반환하기 떄문에 <br>
콘솔을 확인하면 Number를 반환하는 것을 알 수 있다.<br><br>

![](https://velog.velcdn.com/images/minyoungdumb/post/b0600a42-ee34-4919-8931-a1b291985f12/image.png)<br><br>
또한, 숫자를 넣으려고 시도하면, 자동완성기능에 숫자형 메소드를 천하고 length를 인식하지 못하는 것을 알 수 있다.<br>
```
console.log(helloGeneric(30).length);
```
![](https://velog.velcdn.com/images/minyoungdumb/post/9cac7206-cea8-4c52-b2f6-aeb0938cc373/image.png)<br><br>

이번엔 Boolean 타입을 대입해보자.<br>
```
console.log(helloGeneric(true));
```
![](https://velog.velcdn.com/images/minyoungdumb/post/01598b0f-a1ea-4806-a03c-0874ca263392/image.png)<br><br>
인자가 boolean으로 들어가고 그에 따라 T의 타입이 Boolean, Return값 또한 boolean이 된다. <br>

제네릭의 장점은 T를 변수처럼 사용해 타입을 지정해줄 수 있다.<br>
any는 모든 것을 반환하고 제네릭은 지정해서 사용하는 차이점을 가진다.<br>

any는 들어오는 input에 따라 달라지는 타이핑을 할 수 없지만 제네릭은 가능하다.<br>

함수를 만들어보자. 홀화살괄호 내에는 T뿐만아니라 2개, 여러 개를 포함시킬 수 있다.<br>
```TypeScript
function helloBasic<T, U, K>
```
T, U, K 는 함수 내에서 유효한 제네릭이다.<br>


작성하는 방법은 클래스, array, 함수 등 더 많아질 수 있으나 사용하는 방법은 명확하다. <br>
```TypeScript
function helloBasic<T>(message: T): T{
	return message;
}
```
위 함수에서 제네릭을 가져다써서 변수처럼 지정해 쓰는 방식과 그렇지 않은 방식이 있다. <br>
```TypeScript
helloBasic<string>("Hey");
```
![](https://velog.velcdn.com/images/minyoungdumb/post/a5fcd2e5-c2c7-43eb-b37e-d80f56e7b857/image.png)<br>

```TypeScript
helloBasic(36);
```
위와 같이 제네릭을 쓰지 않으면 <T>가 자동적으로 추론이 된다. 추론 규정에 따라서 T가 36이 된다.<br>
우리의 생각으로는 36이면 Number로 출력이 되어야 한다고 생각하겠지만 TS는 가장 좁은 범위의 타이핑을 추론하기 때문에<br>
36을 넣게 되면 T 자체는 36이 된다. 결과물인 return type도 36이 된다.<br>
  
그러니, 넣어서 사용하면 <T>뒤의 인자가 제한이 되고, 넣지 않고 사용하면 <T>가 추론된다고 이해하면 된다.<br>

그렇다면, 함수를 만들어보자.<br> 
홀화살괄호 내에는 T뿐만아니라 2개, 여러 개를 포함시킬 수 있다.<br>
```TypeScript
function helloGeneric<T, U, K>
```
`T, U, K` 는 함수 내에서 유효한 제네릭이다.<br>


작성하는 방법은 `클래스`, `array`, `함수` 등 더 많아질 수 있으나 사용하는 방법은 명확하다.<br>
```TypeScript
function helloGeneric<T>(message: T): T{
	return message;
}

helloGeneric<string>("Hey"); 
helloGeneric<string>(39); // Error
```
`string`으로 타입을 지정했으니, `39`라는 숫자를 넣으면 에러가 뜬다.<br>

제네릭을 가져다써서 `변수처럼 지정해 쓰는 방식`과 `그렇지 않은 방식`이 있다.<br>
	
```TypeScript
helloGeneric<string>("Hey");
```
(`string`으로 지정된다.)<br><br>
	
![](https://velog.velcdn.com/images/minyoungdumb/post/d17865d0-4cc8-4baa-8881-dd9ce144898f/image.png)<br><br>
	
```TypeScript
helloGeneric(36);
```
	
![](https://velog.velcdn.com/images/minyoungdumb/post/24c788a8-148e-43de-865c-7ba2a18a5bdb/image.png)<br><br>
(`36`으로 지정된다.)<br>
	
위와 같이 제네릭을 쓰지 않으면 `<T>`가 자동적으로 추론이 된다. 추론 규정에 따라서 `T`가 `36`이 된다.<br>

우리의 생각으로는 `36`이면 `Number`로 출력이 되어야 한다고 생각하겠지만 TypeScript는 가장 좁은 범위의 타이핑을 추론하기 때문에<br>
`36`을 넣게 되면 `T` 자체는 `36`이 된다. 결과물인 return type도 `36`이 된다.<br>
  
그러니, 넣어서 사용하면 `<T>`뒤의 인자가 제한이 되고, 넣지 않고 사용하면 `<T>`가 추론된다고 이해하면 된다.<br>
또한, 아래 그림과 같이 `T`뿐만 아니라 더 많은 인자를 넣어줄 수 있다.<br><br>
![](https://velog.velcdn.com/images/minyoungdumb/post/45e63f47-8c53-4e36-a7c6-7787f3b17204/image.png)<br><br>
현재, `U`를 리턴타입에 사용하지 않고 있기에 의미가 없긴 하지만 `T`와 조합해서 사용하면 의미가 있을 수 있다.<br>
위와 같이 두 가지 제네릭을 사용하는 경우엔<br><br>
![](https://velog.velcdn.com/images/minyoungdumb/post/33b93bf6-11af-4a02-9cb9-23e73ac0a2ab/image.png)<br><br>
인자를 `U`까지 채워라는 에러 메세지가 발생하는 것을 볼 수 있다.<br><br>
![](https://velog.velcdn.com/images/minyoungdumb/post/384c514e-4156-4be8-8cd0-ac981f289473/image.png)<br><br>
`string, number`로 `T, U` 를 채워 주었고, 인자로 `"Hey"`와 `12`를 넣어주니 에러가 사라졌다.<br><br>
![](https://velog.velcdn.com/images/minyoungdumb/post/3a9b6a10-c4ab-4146-8d09-83fb0cd13fa8/image.png)<br><br>
또한 변수를 지정하지 않은 방식 또한 `21, 31`을 넣어줌으로써 `T, U`를 `21, 31`로 만들어 주었다.<br>




