# String Interpolation
아래 3가지 방법이 가장 나은 방법...

## f-Strings
```python
str1 = "Hello"
str2 = "World"
print(f"{str1}, {str2}!")
```
- Python 3.6 이상부터 지원되며 속도도 가능빠름!

## #2
```python
print('%s, %s! test string' % ('Hello', 'World'))
```
## #3
```python
print("{}, {}!".format('Hello', 'World'))
```
- 3가지 방법 중 가장 느림