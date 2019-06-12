
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190604-lightgray.svg?style=flat-square)

# \[MongoDB] find 수행 시 filter 와 projection 적용하기

> sql 에서는 where 절 이하로 적용하는 여러 가지 조건들을 mongodb 에서는 filter, projection 등의 인자로 대신한다. 일단 예제를 보자

```sql
// in mongodb
db.countries.find({Name: {$regex: 'republic', $options: 'i'}}, {_id: false, Name: true, Code:true});

// in pymongo
db[countries].find(filter={'Name': {'$regex': 'republic', '$options': 'i'}}, projection={'Name': 'true', 'Code': 'true'})
```

- ```SELECT Name, Code From countries WHERE 'Name' LIKE '%republic%'``` 쿼리문과 같은 결과를 반환한다.
- 추가적으로 ```'$options': 'i'``` 조건은 대소문자를 구분하지 않고 조회하도록 하는 필터이다.
- NoSql 이므로, 결과값은 JSON-tuple 형식으로 반환된다.
- 추가적인 옵션으로 **skip / limit / sort** 등을 세팅할 수 있다.
- 각각 **조회할 시작 도큐먼트 / 한 번에 조회되는 개수 / 정렬방식** 을 의미한다.





