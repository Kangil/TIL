# printf()
https://www.cs.colostate.edu/~cs160/.Summer16/resources/Java_printf_method_quick_reference.pdf
```java
System.out.printf("format-string"[, arg1, arg2, ...]);
```

## format-string
%[flags][width][.precision]conversion-character

## flags
```
- : 왼쪽정렬 (default 오른쪽정렬)
+ : 양수음수 기호 표시
  : 음수일때 기호 표시, 양수는 공백
0 : 제로패딩 (default 공백패딩)
, : 1000 이상 컴마표시

 ```

## conversion-character
- d
- f
- c
- s
- h
- n : 플랫폼에 맞춘 개행 \n보다 호환성 좋음

## examples
```java
double total = 12323.34324;
System.out.printf("Total: $%- ,20.2f!%n", total); 
```
```
// 결과
Total: $ 12,323.34          !
!
```