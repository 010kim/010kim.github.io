---
layout: single
title: "Pandas의 Categorical Type란"
excerpt: "Categorical Type 설명"
classes: wide
---

## Pandas의 Categorical Type란

Pandas에는 Categorical Type이 존재한다. Column Type의 흔한 예시로 int64와 String Object가 있는데, 'Categorical'이라고 불리는 Type은 다소 생소하다. Titanic 데이터를 예시로 사용해보자. 


```python
import pandas as pd

titanic = pd.read_csv('titanic.csv', header=0)
print(titanic.dtypes)
```

*PassengerId      int64*\
*Survived         int64*\
*Pclass           int64*\
*Name            object*\
*Sex             object*\
*Age            float64*\
*SibSp            int64*\
*Parch            int64*\
*Ticket          object*\
*Fare           float64*\
*Cabin           object*\
*Embarked        object*\
*dtype: object*

\
숫자의 Type은 int64, float64이며, 문자열의 Type은 object 인 것을 알 수 있다. (object는 Python String Object를 뜻함) 그렇다면 Categorical Type은 그럼 무엇일까? 'Sex' (성별) Column을 Categorical Type으로 변환시켜 보도록 하겠다.


```python
titanic_sex = titanic['Sex'].astype('category')
print(titanic_sex)
```

*0        male*\
*1      female*\
*2      female*\
*3      female*\
*4        male*\
        *...  \
886      male*\
*887    female*\
*888    female*\
*889      male*\
*890      male*\
*Name: Sex, Length: 891, dtype: category*\
*Categories (2, object): ['female', 'male']*\



데이터 타입이 'category'인 것을 확인할 수 있다. 일반적으로 DataFrame의 Column의 값은 Numpy Array로 저장되어 있다. 하지만  데이터 타입이 'category'인 Column은 Numpy Array로 저장되지 않고, Pandas Categorical라는 객체로 저장된다. 



```python
print(type(titanic_sex.values))
```

*<class 'pandas.core.arrays.categorical.Categorical'>*



Pandas Categorical 객체는 categories와 codes라는 attribute를 가지고 있다. categories에는 해당 객체가 가지고 있는 고유한 카테고리 값을 가지고 있으며, codes에는 각 고유한 카테고리에 매칭된 정수값을 가지고 있다. 



```python
print(titanic_sex.values.categories)
```

*Index(['female', 'male'], dtype='object')*

```python
print(titanic_sex.values.codes)
```

**[1 0 0 0 1 1 1 1 0 0 0 0 1 1 0 0 1 1 0 0 1 1 0 1 0 0 1 1 0 1 1 0 0 1 1 1 1*
 *1 0 0 0 0 1 0 0 1 1 0 1 0 1 1 0 0 1 1 0 1 0 1 1 0 1 1 1 1 0 1 0 1 1 0 1 1*
 *1 1 1 1 1 0 1 1 0 1 0 0 1 1 0 1 1 1 1 1 1 1 1 1 0 1 0 1 1 1 1 1 0 1 1 0 1*
 *0 1 0 0 1 1 1 1 0 1 1 1 0 1 1 1 1 0 1 1 1 0 0 1 1 0 1 1 1 0 0 0 1 1 1 1 0*
 *1 1 1 0 1 1 1 1 0 1 1 1 1 0 1 1 1 1 0 0 1 1 1 1 0 1 1 1 1 0 1 1 0 1 1 1 0*
 *1 0 1 1 1 0 1 0 1 0 0 1 1 0 0 1 1 1 1 1 0 1 1 0 1 1 0 1 1 1 0 0 1 0 1 1 1*
 *1 1 1 1 1 1 1 0 0 1 1 0 1 0 1 0 1 1 0 0 1 1 1 1 0 0 1 1 1 0 1 1 0 0 0 0 0*
 *0 1 1 1 1 0 1 1 1 0 0 1 1 0 1 0 0 0 1 1 0 1 1 1 1 1 1 1 1 1 0 0 0 1 0 1 1*
 *1 0 1 0 0 1 1 0 1 1 0 0 1 0 0 0 0 1 1 0 0 1 0 0 1 1 0 0 1 0 1 0 0 0 0 1 1*
 *1 0 1 1 0 1 1 1 0 1 1 1 0 0 0 1 1 1 1 1 1 1 1 0 0 0 0 1 1 0 1 1 1 0 0 0 0*
 *1 1 1 1 0 0 0 1 1 1 0 0 1 0 1 1 1 0 1 0 1 1 1 0 0 1 0 1 1 0 1 1 0 1 0 1 1*
 *1 1 0 1 1 0 1 1 0 0 0 1 0 1 1 1 0 1 1 0 0 1 1 1 0 0 1 1 0 0 0 1 1 0 1 1 0*
 *1 1 0 1 0 1 1 1 1 1 1 1 1 0 0 1 1 1 1 1 1 1 1 1 1 0 1 1 0 0 0 1 1 1 1 0 1*
 *1 1 0 1 0 0 1 1 1 1 1 1 1 1 1 0 1 0 1 1 0 0 0 0 1 0 1 1 1 1 1 1 0 1 1 0 1*
 *0 1 0 1 1 0 1 1 0 1 1 1 0 1 1 0 0 0 1 0 1 0 0 0 0 1 1 1 0 1 1 1 1 1 1 1 0*
 *1 0 1 0 0 1 1 1 1 0 1 1 0 1 1 1 0 1 0 1 1 0 0 0 1 0 0 1 1 1 0 1 1 1 1 1 0*
 *1 0 1 1 0 1 1 1 0 1 1 1 1 1 1 1 0 0 0 1 0 1 1 0 1 0 0 1 1 1 1 1 1 1 1 0 1*
 *1 1 1 1 1 0 0 1 1 0 1 1 0 0 1 0 1 1 1 1 0 1 0 1 0 0 1 1 0 1 1 1 1 1 1 1 1*
 *1 1 1 0 0 1 1 1 1 1 1 0 0 1 0 1 1 1 1 1 1 1 1 0 1 0 1 1 1 1 1 0 1 1 0 1 0*
 *1 1 1 0 1 0 1 0 1 1 1 1 1 0 0 1 1 0 1 1 1 1 1 0 0 1 0 0 1 1 1 1 1 0 1 1 1*
 *1 1 0 1 1 1 1 0 1 1 0 1 1 1 0 1 1 1 1 0 1 1 1 0 1 0 1 0 1 1 1 1 0 1 0 1 1*
 *0 1 0 0 0 1 1 1 1 0 1 1 1 1 1 0 1 1 1 0 0 1 0 1 0 1 1 1 1 1 0 1 0 1 1 1 0*
 *1 1 0 1 1 1 0 1 1 0 1 1 1 1 1 0 0 1 1 1 1 0 1 1 1 1 1 1 0 1 1 1 1 1 1 0 1*
 *1 0 0 0 0 0 1 0 1 1 1 0 0 1 0 0 1 1 1 1 0 1 1 0 0 1 1 1 0 0 1 0 1 1 0 1 0*
 0 1 1]*
