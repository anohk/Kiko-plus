---
layout: post
title: "Custom template filter"
description: "장고 템플릿 필터를 만들어보자"
date: 2017-07-08
tags: [Django]
comments: true
share: true
---


Django admin 페이지에서 데이터를 표시하는 방법을 커스텀해야할 일이 생겼다. 숫자 데이터의 가독성을 높이기 위해 천 단위로 `,` 를 적용하는 일이다. 

Django 문서를 뒤져보니 [humanize](https://docs.djangoproject.com/en/1.11/ref/contrib/humanize/)가 존재한다. 이 중 `intcomma` 필터를 적용하면 천 단위로 `,`를 표시할 수 있다. 

템플릿 파일에서 `intcomma`를 바로 적용하면 쉽게 끝날것이라 생각했지만 현실은 그렇지 않았다. admin이 GUI 구성을 쉽게 도와주는 (커스텀하지 않는다면...) 외부 패키지에 의존하고 있는 상황이라 커스텀 필터가 필요했다. 

## 1. Code layout

### 1-1. models.py, views.py 등의 파일과 같은 위계에 templatetags 디렉토리를 생성한다.  
> **templatetags** 디렉토리를 Python package로 취급할 수 있도록 **__init__.py** 파일을 포함해야한다. 

### 1-2. templatetags 패키지 내부에 커스텀 태그 혹은 필터 파일을 생성한다. 

```
product/
	__init__.py
	models.py
	views.py
	templatetags/
		__init__.py
		add.py
```

아래와 같이 `{{ "{% load " }}%}` 태그가 동작하기 위해서는 **templatetags** 패키지를 포함하고 있는 앱이 **INSTALLED_APPS** 에 설정되어 있어야 한다.  
**templatetags** 패키지에 여러개의 모듈을 생성하여 사용할 수 있으며, `{{ "{% load " }}%}` syntax에는 앱 이름이 아닌 모듈 이름을 사용한다. 

```python
{{ "{% load add " }}%}
```
### 1-3. 유효한 라이브러리로 만들기
작성할 모듈이 라이브러리로 동작하려면 아래와 같이 `register` 라는 모듈 레벨의 변수를 포함해야한다. 
아래와 같이 모듈 파일의 상단에 해당 변수를 포함시켜준다.

```python
from django import template

register = template.Library()
```
> **register** : 모든 태그와 필터가 등록되어있는 **template.Library** 의 인스턴스이다. 



## 2. 커스텀 필터 작성하기
모듈에 커스텀 필터를 정의한다.
실제 코드를 작성할 때에는 여러 조건들이 포함되었지만, 간단한 예제로 작성해보았다.

```python
def add_comma(value):
	return '{:,}'.format(value)
```
이렇게 정의된 모듈은 템플릿에서 다음과 같이 사용될  것이다.

```python
{{ "{% load " }}%}
...
...
{{ "{{ somevariable|add_comma " }}}}
```

> `{:,}` 은 천 단위로 구분점을 찍는 [Format String Syntax](https://docs.python.org/2/library/string.html#grammar-token-format_spec)이다.  
 
## 3. 커스텀 필터 등록하기
커스텀 필터를 정의했다면, Django의 템플릿 언어에서 사용할 수 있도록 Library 인스턴스에 등록을 해야한다. 

```python
register.filter('add_comma', add_comma)
```

**Library.filter()** 메서드는 두 개의 인자를 취한다. 

1. 필터의 이름 - a string
2. 컴파일 함수 - a Python function

**@register.filter** 데코레이터를 사용하는 방법도 있다.

```python
@register.filter(name='add_comma')
def add_comma(value):
	return '{:,}'.format(value)

# or

@register.filter
def add_comma(value):
	return '{:,}'.format(value)
```
두 번째 처럼 name 인자에 아무것도 넘기지 않으면 Django는 함수의 이름을 필터 이름으로 사용한다. 


> [Custom template tags and filters](https://docs.djangoproject.com/en/1.11/howto/custom-template-tags/) 문서 참조