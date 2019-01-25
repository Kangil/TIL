# String Interpolation
- https://realpython.com/python-f-strings/

## % formatting
```python
print('%s, %s! test string' % ('Hello', 'World'))
```
## str.format()
```python
print("{}, {}!".format('Hello', 'World'))
```
- 3가지 방법 중 가장 느림

## f-Strings
```python
str1 = "Hello"
str2 = "World"
print(f"{str1}, {str2}!")
```
- Python 3.6 이상부터 지원되며 속도도 세 가지중 가능 빠름
- Multiline 에서도 사용 가능