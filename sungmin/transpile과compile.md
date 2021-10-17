# transpile vs compile

프론트엔드 개발을 하다보면 자주 babel이나 typescript를 자주 언급하게 되는데 이들을 설명할때 compile 혹은 transplie이라는 용어를 종종 사용 하게 되는데 
이 둘의 차이점을 정확하게 이해하고 올바른 용어를 사용할 수 있도록 되짚어 보도록 하겠습니다.

## compile
컴파일의 경우는, 한 언어로 작성된 소스 코드를 다른 언어로 변환하는 것을 의미 합니다.
```
Java -> bytecode
C -> assembly
```

## transpile
트랜스파일은 소스 코드를 비슷한 수준의 추상화를 가진 다른 언어로 변환하는 것을 말합니다.

```
es6 -> es5
c++ -> c
typescript -> javascript
```

### Q) 그럼 transpile 상황에 compile 한다는 것은 옳지 않은 것인가?
[Source-to-source compiler](https://en.wikipedia.org/wiki/Source-to-source_compiler)
transpiler는 source to source compiler 라고도 부르며, 컴파일러의 일종으로 트랜스파일러를 설명하고 있습니다. 따라서 compile은 transpile 보다 더 큰 범주에 속하게 되는 것입니다.

### Q) typescript는 javascript로 컴파일한다고 표현하는데 틀린말인가?
typescript는 javascript로 변환되는데, 컴파일링 된다고는 하지만 정확히 말하면 트랜스파일링 된다고 말할 수 있습니다. 하지만 위에서 보았듯이 컴파일한다는 말이 틀린말은 아닙니다.

## 결론
컴파일과 트랜스파일은 개념상 차이가 있지만 혼용해 사용하는 경우가 많은 것 같습니다. 자주 혼용하여 사용하는 만큼 조금 더 정확한 이해를 바탕으로 적절한 용어를 사용하면 좋을 것 같습니다.

