---
layout: post
title: "Python list"
description: "리스트에 대하여"
date: 2017-07-09
tags: [Python]
comments: true
share: true
---

python list 자료형에 대한 정리

# 리스트 자료형

## 1. 리스트 생성하기 
리스트 내부에는 어떠한 자료형도 포함시킬 수 있다.   
요소는 `,`로 구분하며 `[]`로 둘러싸여있다.

```python
empty = []	# 아무것도 포함하지 않는 빈 리스트
number = [1, 2, 3]	# 숫자를 요소로 갖는 리스트
string = ['a', 'b', 'c']	# 문자를 요소로 갖는 리스트
number_with_string = [1, 2, 'a', 'b']	# 숫자와 문자를 요소로 갖는 리스트
list_in_list = [[1, 2, 3], 'a', 'b']	# 리스트를 요소로 갖는 리스트
```

`list()` 함수로 빈 리스트를 할당할 수 있다.

```python
empty = list()
```

## 2. 데이터 타입을 리스트로 변환하기   
`list()` 함수를 사용하여 다른 데이터 타입을 리스트로 변환할 수 있다. 

#### string to list

```python
>>> list('anohk')
['a', 'n', 'o', 'h', 'k']
```

#### tuple to list

```python
>>> tuple = ('morning', 'noon', 'night')
>>> list(tuple)
['morning', 'noon', 'night']
```

#### using split()

```python
>>> coffee = 'cold brew black'
>>> coffee.split(' ')
['cold', 'brew', 'black']
```

## 3. [offset] 사용하기
### offset으로 값을 추출하기 
리스트는 offset으로 해당하는 값을 추출할 수 있다.

```python
>>> coffee = ['cold', 'brew', 'black']
>>> coffee[0]
'cold'
>>> coffee[1]
'brew'
>>> coffee[2]
'black'
```

offset을 음수로 적용하면 거꾸로 값을 추출한다. 

```python
>>> coffee[-1]
'black'
>>> coffee[-2]
'cold'
>>> coffee[-3]
'brew'
```


### offset으로 요소 변경하기 
offset으로 해당하는 값을 얻고, 이를 변경할 수 있다. 

```python 
>>> aroma = ['lemon', 'cherry', 'oak']
>>> aroma[0] = 'grape'
>>> aroma
['grape', 'cherry', 'oak']
```

### Slice
리스트를 슬라이스하여 서브시퀀스를 추출한다.

```python
>>> aroma = ['lemon', 'cherry', 'oak']
>>> aroma[1:] 
['cherry', 'oak']
>>> aroma[-2:]
>>> ['cherry', 'oak']
>>> armoa[::2]	# step
['lemon', 'oak']
```

## 4. 요소 추가하기 

### append()
`append()`를 사용하여 리스트의 마지막에 새로운 요소를 추가할 수 있다. 

```python
>>> aroma.append('leather')
>>> aroma
['lemon', 'cherry', 'oak', 'leather']
```

### insert()
`insert()`를 사용하면 원하는 위치에 요소를 추가할 수 있다. 

```python
>>> aroma = ['lemon', 'cherry', 'oak']
>>>aroma.insert(1, 'leather')
>>>aroma
['lemon', 'leather', 'cherry', 'oak']
```

### extend()
`extend()`를 사용하면 다른 리스트와 병합할 수 있다. 

```python
>>> aroma = ['lemon', 'cherry', 'oak']
>>> others = ['leather', 'grape']
>>> aroma.extend(others)
>>> aroma
['lemon', 'cherry', 'oak', 'leather', 'grape']
```

`+=` 를 사용하는 것도 가능하다. 

```python 
>>> aroma += others
>>> aroma
['lemon', 'cherry', 'oak', 'leather', 'grape']
```

## 5. 요소 삭제하기 

### offset 으로 삭제하기 
`del` 은 함수가 아니라 파이썬의 구문이다. `(할당)=`의 반대 개념이며, 객체로부터 이름을 분리하고 메모리를 비운다.

```python
>>> del aroma[0]
>>> aroma
['cherry', 'oak']
```

### 값으로 삭제하기
offset의 값을 모른다면, `remove()`를 사용하여 해당 요소를 삭제할 수 있다. 
 
```python
>>> aroma = ['lemon', 'cherry', 'oak']
>>> aroma.remove('cherry')
>>> aroma
['lemon', 'oak']
```

### offset으로 요소를 얻고 삭제하기 
`pop()`을 이용하면 해당 요소를 가져오고 해당 요소를 리스트에서 삭제한다. 

```python
>>> aroma = ['lemon', 'cherry', 'oak']
>>> aroma.pop()
'oak'
>>> aroma
['lemon', 'cherry']
```
`pop()` 인자에 아무것도 넘겨주지 않는다면 `-1`이 적용되어 마지막 요소가 추출되고, 삭제된다. 

인자에 offset 값을 넣어주면 해당 요소에 대해 동작한다. 

```python
>>> aroma = ['lemon', 'cherry', 'oak']
>>> aroma.pop(1)
'cherry'
>>> aroma
['lemon', 'oak']
```






