# 올바른 `Ref` 사용 방법

일반적으로 `React`에서는 자식을 `re-render` 하려면 `props`를 전달한다.
그러나, 직접적으로 `React Element`, `DOM Node`를 수정하는 경우가 생길 수 있는데,
`Ref`는 `React Element`나 `DOM Node`에 접근하는 방법을 제공한다.

## `Ref`를 사용하는 타이밍

1. 포커스, 텍스트 선택 영역, 미디어의 재생 등을 관리
2. 애니메이션을 직접적으로 실행
3. 서드 파티 DOM 라이브러리(JQuery 등)를 `React`와 같이 사용

   선언적으로 해결될 수 있는 경우 `ref` 사용을 지양

> **비제어 컴포넌트를 제어할 때로 볼 수 있음**

---

## `Ref` 사용 방법

> 함수형 컴포넌트 기준으로 작성하였다.

> 상수 등 `render`와 무관한 값을 저장하는 방식은 논하지 않는다.

### `useRef` `Hook`을 사용

```tsx
const HookRef: FC = () => {
  const [focusState, setFocusState] = useState<boolean>(true);

  const focusRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    if (focusState) {
      focusRef.current?.focus();
    }
  }, [focusState]);

  useEffect(() => {
    const interval = setInterval(() => setFocusState((prev) => !prev), 1000);
    return () => clearInterval(interval);
  }, []);

  return (
    <div>
      <h2>used to Hook Ref</h2>
      {focusState && <input ref={focusRef} type="text" />}
    </div>
  );
};
```

### `DOM` 요소에 `ref`속성을 콜백함수로 사용

```tsx
const CallbackRef: FC = () => {
  const [focusState, setFocusState] = useState<boolean>(false);

  const onRefCallback: LegacyRef<HTMLInputElement> = (ref) => {
    ref?.focus();
  };

  useEffect(() => {
    const interval = setInterval(() => setFocusState((prev) => !prev), 1000);
    return () => clearInterval(interval);
  }, []);

  return (
    <div>
      <h2>used to Callback Ref</h2>
      {focusState && <input ref={onRefCallback} type="text" />}
    </div>
  );
};
```

`ref`가 설정되고 해제되는 상황을 세세하게 다룰 수 있는 방법이다.

이렇게 콜백함수로 사용할 경우 불필요한 `useEffect` 사용을 최적화 할 수 있어서 `render` / `setState` 시 포커스, 블러 및 **간단한 작업**을 이용할 때 효과적이다.

위 코드의 `onRefCallback`을 `useCallback`으로 감싼 후 `deps`에 빈 배열을 넣어주게 되면
콜백함수는 `mount / unmount` 시에만 실행된다.

콜백 함수는 **업데이트 과정** 중에 처음에는 `null`로, 그 다음에는 `DOM Node`로 총 두 번 호출된다.
이러한 현상은 매 렌더링마다 `ref` 콜백의 새 인스턴스가 생성되므로 발생한다.

> 위 방법들은 `ref`를 `HTML Element`에 **직접 접근**하여 사용하는 방법이다.

---

### `forwardRef`를 사용

```tsx
const ForwardRef = forwardRef<HTMLInputElement>((props, ref) => {
  return <input ref={ref} type="text" />;
});

interface PropsRefProps {
  propsRef: RefObject<HTMLInputElement>;
}

const PropsRef = ({ propsRef }: PropsRefProps) => {
  return <input ref={propsRef} type="text" />;
};

const ForwardAndProps: FC = () => {
  const forwardingRef = useRef<HTMLInputElement>(null);
  const propsRef = useRef<HTMLInputElement>(null);

  return (
    <div>
      <h3>Forward And Props</h3>
      <ForwardRef ref={forwardingRef} />
      <PropsRef propsRef={propsRef} />
    </div>
  );
};
```

`ref`라는 이름의 `props`는 `key`처럼 일반적인 방법으로 자식 컴포넌트로의 `props` 전달이 안 된다.  
`React`의 예약어라고 생각하면 이해하기 쉬울 듯 하다.
하지만 이를 위 코드처럼 `forwardRef`라는 `HOC`를 이용하면 넘길 수 있게 된다.

PropsRef 컴포넌트처럼 `props` 이름을 `ref`가 아닌 이름으로 넘겨도 사용할 수는 있는데, 이에 `React` 공식 문서에서는

> `forwardRef`는 일부 컴포넌트가 수신한 `ref`를 받아 조금 더 아래로 전달할 수 있는 `Opt-in` 기능입니다. 일반적으로 보기 드문 경우지만, 부모 컴포넌트에서 자식 컴포넌트의 `DOM Node`에 접근하는 것은 컴포넌트의 캡슐화를 파괴하기 때문에 권장되지 않습니다.

> 또한, 컴포넌트 라이브러리에서 `forwardRef`를 사용하기 시작할 때 이것을 변경사항으로 간주하고 라이브러리의 새로운 중요 버전을 릴리즈 해야 합니다.

때문에 부모 컴포넌트에서 자식 컴포넌트의 `props`로 `ref`를 사용해야하는 경우를 지양해야 하며,
하더라도 `React`에서 제공하는 공식적인 방법을 이용하는게 안전하다고 볼 수 있다.

또한 `ref`의 기능을 굳이 다른 이름으로 사용하는 것보다 가독성이 좋기 때문에 굳이 사용하자면 해당 방법을 권장한다.

### `forwardRef`와 `useImperativeHandle`을 사용

```tsx
interface RefHandler {
  focus: () => void;
}

const ImperativeHandleRef = forwardRef<RefHandler>((props, ref) => {
  const inputRef = useRef<HTMLInputElement>(null);

  useImperativeHandle(
    ref,
    () => ({
      focus: () => inputRef.current?.focus(),
    }),
    []
  );

  return <input ref={inputRef} type="text" {...props} />;
});

const Parent: FC = () => {
  const handleRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    handleRef.current?.focus();
  }, [handleRef]);

  return (
    <div>
      <h3>ImperativeHandleRef</h3>
      <ImperativeHandleRef ref={handleRef} />
    </div>
  );
};
```

> 대부분의 경우 `ref`를 사용한 명령형 코드는 피해야 합니다. 또한, `useImperativeHandle`는 `forwardRef`와 더불어 사용해야합니다.

`ref`를 사용할 때 부모 컴포넌트에 노출되는 인스턴스 값을 사용자화 하는 `Hook`이다.

이를 사용하면 자식 컴포넌트의 `DOM Node`에 직접적으로 접근을 하는 것이 아니라 사용자화된 Method에만 접근이 가능하므로 컴포넌트 간의 **독립성이 더욱 보장**될 수 있다.

---

## `DOM Node` `re-render` 성능을 최적화 시킬 수 있는 Tip.

`React`를 학습할 때, 보통의 문서에서 `input`, `textarea` 등의 `DOM Node`를 조작하는 방법으로

```tsx
const Input = () => {
  const [value, setValue] = useState<string>("");

  const onChange = (e: ChangeEvent<HTMLInputElement>) =>
    setValue(e.target.value);

  const onSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log("input: ", value);
  };

  return (
    <form onSubmit={onSubmit}>
      <input type="text" value={value} onChange={onChange} />
    </form>
  );
};
```

와 같이 `value`와 해당 `value`를 조작하는 `ChangeEventHandler`를 `props`로 넘겨서 사용하게 된다.

하지만 이렇게 사용할 경우, `onChange` 이벤트가 발생할 때마다 `re-render` 하게 되므로 불필요한 컴퓨팅 자원을 소모하고, 성능 저하를 유발하는 이슈로 판단될 수 있다.

만약 `onChange` 이벤트 발생 시 `input`의 내용 외의 별다른 변화가 없다면,

```tsx
const Input = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  const onSubmit = (e: FormEvent) => {
    e.preventDefault();
    console.log("input: ", inputRef.current?.value);
  };

  return (
    <form onSubmit={onSubmit}>
      <input ref={inputRef} type="text" />
    </form>
  );
};
```

위 코드와 같이 작성하여 불필요한 `re-render`를 방지할 수 있다.

---

## 주의사항

### 왜 `ref`는 `current`로 접근해야하는가?

> 컴포넌트가 마운트될 때 React는 current 프로퍼티에 DOM 엘리먼트를 대입하고, 컴포넌트의 마운트가 해제될 때 current 프로퍼티를 다시 null로 돌려놓습니다. ref를 수정하는 작업은 componentDidMount 또는 componentDidUpdate 생명주기 메서드가 호출되기 전에 이루어집니다.

`React`는 `Virtual DOM`을 사용하기 때문에 실제 `DOM`에 반영되는 시점부터 `ref`를 통한 `DOM Node`에 접근할 수 있게 된다. 여기서 `Virtual DOM`이 업데이트되는 경우 실제 `DOM`에도 변경이 반영되는 경우가 있기 때문에 `re-render` 발생 시 `React`는 `ref.current`에 우선 `null`로 변경한 후 다시 `DOM Node`로 변경하는 작업을 거친다.

이처럼 유동적이기에 `React`는 `current` property의 값을 계속해서 수정한다.

### 왜 `DOM API`를 쓰면 안 될까?

`React`에서 `document.querySelector('#h1')`, `document.getElementById('h1')`와 같은 `DOM API`를 사용하더라도 잘 동작한다.

하지만 이는 예측치 못한 상황의 로직의 큰 결함(`Life Cycle`의 흐름을 예측하지 못한 상황)이 발생할 수 있기에 지양하는 것이 좋다.


## 마무리

`ref`는 단순히 포커스와 같은 UX나 `render`와 무관한 값을 저장하는 요소로만 사용되지 않고
실제 성능을 개선하는데 충분한 역할을 할 수 있다는 점을 깊게 고려하며 개발을 하면 Good Application, Product를 개발하는데 좋은 요소로 작용될 수 있는 점을 인지하자.