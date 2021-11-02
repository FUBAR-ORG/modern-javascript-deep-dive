# unknown vs any

## unknown

`unknown`은 타입스크립트 3.0 버전에서 추가된 타입이다.(현재 4.4.x)

`unknown`에는 `any`와 마찬가지로 모든 타입의 값의 할당이 허용된다. 그러나 `unknown` 타입의 값은 `any`와 `unknown` 타입의 변수에만 할당이 가능하다.

```ts
// #1. unknown 타입에는 모든 타입의 값 할당이 허용된다.

let foo: unknown;

foo = 1;
foo = 'It is a string value';
foo = {};
```

```ts
// #2. unknown 타입의 값은 any와 unknown 타입의 변수에만 할당이 가능하다.

declare const foo: unknown;

let baz: any = foo;
let bar: string = foo; // Error: 'unknown' 형식은 'string' 형식에 할당할 수 없습니다.
```

<br>`|` 연산자를 사용한 유니온 타입의 경우 하나라도 `unknown` 이면 모든 타입이 `unknown` 타입으로 평가된다. 예외적으로 하나 이상이 `any` 타입일 경우 `any`로 평가된다.

```ts
type UnionType1 = unknown | null; // unknown
type UnionType2 = unknown | undefined; // unknown
type UnionType3 = unknown | string; // unknown
type UnionType4 = unknown | number[]; // unknown

type UnionType5 = unknown | any; // any
```

`&` 연산자를 사용한 인터섹션 타입의 경우 어떤 타입과의 인터섹션을 하더라도 `unknown` 타입으로 평가되지 않는다.

```ts
type IntersectionType1 = unknown & null; // null
type IntersectionType2 = unknown & undefined; // undefined
type IntersectionType3 = unknown & string; // string
type IntersectionType4 = unknown & number[]; // number[]
type IntersectionType5 = unknown & any; // any
```

<br>

## unknown vs any

타입스크립트 컴파일러는 `any` 타입으로 지정된 값에 대한 모든 작업을 허용한다. 그러나 `unknown` 타입으로 지정된 값에 대해서는 제한이 있다.

```ts
let anyValue: any;

anyValue[0];
anyValue();
new anyValue();
anyValue.foo;
```

```ts
let unknownValue: unknown;

unknownValue(); // Error: 객체 '알수없는 형식'입니다. Object is of type 'unknown'
unknownValue[0]; // Error
new unknownValue(); // Error
unknownValue.foo; // Error
```

`unknown` 타입은 모든 타입의 값 할당이 가능하므로 함수에 입력받는 인수를 파악할 수 없거나 API에서 특정 값을 받아올 때 사용할 수 있다.

다음은 매개변수를 `unknown` 타입으로 지정하고 인수를 입력받아 타입 좁히기(type narrowing)를 통해 특정 동작을 하는 함수 예제이다.

```ts
function stringifyForLogging(value: unknown): string {
  if (typeof value === 'function') {
    // value가 Function 타입일 경우
    // 함수 name 프로퍼티에 액세스
    const functionName = value.name || '(anonymous)';
    return `[function ${functionName}]`;
  }

  if (value instanceof Date) {
    // `value`가 Date 타입일 경우
    // toISOString 메서드 호출
    return value.toISOString();
  }

  return String(value);
}
```

쉽게 말해, `any`는 타입 체킹을 하지 않겠다는 의미이며 `unknown`은 모든 타입의 상위 개념으로 생각할 수 있다.

`any` 타입의 경우 어떠한 경우에서도 컴파일 에러가 발생하지 않는다. **`unknown` 타입의 경우 타입을 좁혀서 사용해야 하기 때문에 `any`와 비교해 에러 발생을 줄일 수 있으므로 상대적으로 더 안전하다고 할 수 있다.**

<br>

## Conclusion

**`any` 타입과 `unknown` 타입 모두 특정 타입을 명시적으로 지정하는 것이 아니기 때문에 사용을 지양**해야한다. 두 타입을 사용하는 것은 타입스크립트 본질에 어긋난다.

그러나 **정말로 꼭 필요한 경우에는 `any` 보다는 `unknown` 타입을 사용하고 타입 좁히기를 하는 것이 더 안전**하다.

<br>

---

#### Refference

- [The unknown Type in TypeScript](https://mariusschulz.com/blog/the-unknown-type-in-typescript)
- [More on Functions](https://www.typescriptlang.org/docs/handbook/2/functions.html#unknown)
- ['unknown' vs. 'any'](https://stackoverflow.com/questions/51439843/unknown-vs-any)
