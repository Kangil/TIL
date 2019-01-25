
# [PyYAML](https://pyyaml.org/)

## Install
```python
pip install pyyaml
#pip설치가 안되어있는 경우 python -m pip install pyyaml 로도 가능
```

# YAML 형식 읽기
```python
import yaml
document = """
  a: 1
  b:
    c: 3
    d: 4
"""
print(yaml.load(document))
```
```
# Result
{'a': 1, 'b': {'c': 3, 'd': 4}}
```

# YAML 형식 쓰기
```python
import yaml
some_dict = {'a': 1, 'b': {'c': 3, 'd': 4}}
print(yaml.dump(some_dict))
```
```
# Result
a: 1
b: {c: 3, d: 4}
```

- Block 형식으로 출력하고 싶은 경우
```python
import yaml
some_dict = {'a': 1, 'b': {'c': 3, 'd': 4}}
print(yaml.dump(yaml.load(document), default_flow_style=False))
```
```
# Result
a: 1
b:
  c: 3
  d: 4
```