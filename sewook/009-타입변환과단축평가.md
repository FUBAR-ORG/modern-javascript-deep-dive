# 9장 타입 변환과 단축 평가

## 9.3 명시적 타입 변환

```javascript
// to number
+'0' + // 0
  true; // 1
'99.99' * 1; // 99.99

// to boolean
!!undefined; // false
!!'false'; // true
!!0; // false
```

## 9.4 단축 평가

```javascript
// 논리연산자
'Cat' || false; // 'Cat'
false && 'Cat'; // false
'Cat' && false; // false
```

```javascript
// null 병합 연산자: ??
// 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환
// 그렇지 않으면 좌항의 피연산자 반환

const foo = null ?? 'default';
const bar = '' ?? 'default';
foo; // 'default'
bar; // ''
```
