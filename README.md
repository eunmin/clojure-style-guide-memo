# 클로저 스타일 가이드 잘 안지켜지는 부분 모음

* <a name="vertically-align-fn-args"></a>
  함수의 인수는 세로로 줄 맞춤 한다.
<sup>[[link](#vertically-align-fn-args)]</sup>

    ```Clojure
    ;; 좋은 예
    (filter even?
            (range 1 10))

    ;; 나쁜 예
    (filter even?
      (range 1 10))
    ```

* <a name="empty-lines-between-top-level-forms"></a>
  최상위의 구문들 사이에는 빈 줄을 넣어라.
<sup>[[link](#empty-lines-between-top-level-forms)]</sup>

    ```Clojure
    ;; 좋은 예
    (def x ...)

    (defn foo ...)

    ;; 나쁜 예
    (def x ...)
    (defn foo ...)
    ```

    관련있는 `def`들을 붙여쓰는 것은 예외다.

    ```Clojure
    ;; 좋은 예
    (def min-rows 10)
    (def max-rows 20)
    (def min-cols 15)
    (def max-cols 30)
    ```

* <a name="pre-post-conditions"></a>
  함수 내부에 확인할 조건이 있을 때 pre와 post 함수를 사용하자.
<sup>[[link](#pre-post-conditions)]</sup>

    ```Clojure
    ;; 좋은 예
    (defn foo [x]
      {:pre [(pos? x)]}
      (bar x))

    ;; 나쁜 예
    (defn foo [x]
      (if (pos? x)
        (bar x)
        (throw (IllegalArgumentException "x must be a positive number!")))
    ```

* <a name="dont-shadow-clojure-core"></a>
  내부 바인딩으로 `clojure.core`의 이름을 사용하지 말자.
<sup>[[link](#dont-shadow-clojure-core)]</sup>

    ```Clojure
    ;; 나쁜 예 - 내부에 clojure.core/map 사용을 강제한다.
    (defn foo [map]
      ...)
    ```

* <a name="no-multiple-forms-fn-literals"></a>
  함수 바디가 여러개의 구문으로 구성되는 경우 함수 리터럴을 사용하지 말자.
<sup>[[link](#no-multiple-forms-fn-literals)]</sup>

    ```Clojure
    ;; 좋은 예
    (fn [x]
      (println x)
      (* x 2))

    ;; 나쁜 예 ( 명시적으로 do 구문이 필요하다.)
    #(do (println %)
         (* % 2))
    ```

* <a name="complement"></a>
  익명함수의 사용보다 `complement` 함수를 사용하자.
<sup>[[link](#complement)]</sup>

    ```Clojure
    ;; 좋은 예
    (filter (complement some-pred?) coll)

    ;; 나쁜 예
    (filter #(not (some-pred? %)) coll)
    ```

    분리함수의 폼 안에 여집합 속성이 있는 경우 이 규칙은 무시하자 (e.g. `even?` and `odd?`).

* <a name="pos-and-neg"></a>
  `(> x 0)`, `(< x 0)` & `(= x 0)` 대신 `(pos? x)`, `(neg? x)` & `(zero? x)`를 사용한다.
<sup>[[link](#pos-and-neg)]</sup>



* <a name="idiomatic-names"></a>
  `pred`와 `coll`과 같은 `clojure.core`에서 사용된 관용어를 사용한다.
<sup>[[link](#idiomatic-names)]</sup>

    * 함수 안에서:
        * `f`, `g`, `h` - 함수 값
        * `n` - 크기 같은 숫자 값
        * `index` - 숫자 인덱스
        * `x`, `y` - 숫자
        * `xs` - 시퀀스
        * `m` - 맵
        * `s` - 문자열 입력
        * `re` - 정규식 표현
        * `coll` - 컬렉션
        * `pred` - 참이나 거짓을 리턴하는 함수
        * `& more` - 가변 인자
        * `xf` - 트랜스듀서에 xform
    * in macros:
        * `expr` - 표현식
        * `body` - 바디
        * `binding` - 매크로 바인딩 벡터


* <a name="atoms-prefer-swap-over-reset"></a>
  가능하면 `reset!`보다 `swap!`을 사용한다.
<sup>[[link](#atoms-prefer-swap-over-reset)]</sup>

    ```Clojure
    (def a (atom 0))

    ;; 좋은 예
    (swap! a + 5)

    ;; 그리 좋지 않음
    (reset! a 5)
    ```

* <a name="reuse-existing-exception-types"></a>
  가능하면 자바에 있는 예외 타입을 사용하라. 기본 타입(예. `java.lang.IllegalArgumentException`,
  `java.lang.UnsupportedOperationException`,
  `java.lang.IllegalStateException`, `java.io.IOException`)의 예외를 던지는 것이
  일반적인 클로저 코드이다.
<sup>[[link](#reuse-existing-exception-types)]</sup>



* <a name="fixme"></a>
  `FIXME`는 코드가 잘못되어서 고쳐야할 때 사용한다.
<sup>[[link](#fixme)]</sup>

* <a name="optimize"></a>
  `OPTIMIZE`는 코드가 느리거나 비효율적이라서 성능상 문제가 생기는 경우에 사용한다.
<sup>[[link](#optimize)]</sup>

* <a name="hack"></a>
  `HACK`은 코드에 냄새가 나는 것 같아 리팩토링이 필요한 경우 사용한다.
<sup>[[link](#hack)]</sup>

* <a name="review"></a>
  `REVIEW`는 의도한대로 동작하는지 확인해야하는 경우에 사용한다.
  예:
  ```
  `REVIEW: Are we sure this is how the client does X currently?``
  ```
<sup>[[link](#review)]</sup>


* <a name="test-ns-naming"></a>
  테스트 네임스페이스는 `yourproject.something-test`과 같이 이름 붙이고 파일은 보통
  `test/yourproject/something_test.clj`(또는 `.cljc`, `.cljs`)로 만든다.
  <sup>[[link](#test-ns-naming)]</sup>

* <a name="test-naming"></a>`clojure.test`를 사용할 때 `deftest`를 사용해 정의하는 테스트는
  `something-test`로 이름을 붙인다.
  ```clojure
  ;; 좋은 예
  (deftest something-test ...)

  ;; 나쁜 예
  (deftest something-tests ...)
  (deftest test-something ...)
  (deftest something ...)
  ```
  <sup>[[link](#test-naming)]</sup>            
