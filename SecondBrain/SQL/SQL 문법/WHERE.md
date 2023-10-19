WHERE 절은 레코드를 필터링하는 데 사용된다.
이는 특정 조건을 충족하는 레코드만 추출하는 데 사용된다.

## 사용 방법
```
SELECT _column1_, _column2, ..._  
FROM _table_name_  
WHERE _condition_;
```

### 조건 예제
```
SELECT * FROM Customers  
WHERE CustomerID = 1;
```

```
SELECT * FROM Customers  
WHERE Country = 'Mexico';
```

#### 사용할 수 있는 연산자
| ------- | ------------------------------------------------------------------------------- |
| =       | Equal                                                                           |
| >       | Greater than                                                                    |
| <       | Less than                                                                       |
| >=      | Greater than or equal                                                           |
| <=      | Less than or equal                                                              |
| <>      | Not equal. **Note:** In some versions of SQL this operator may be written as != |
| BETWEEN | Between a certain range                                                         |
| LIKE    | Search for a pattern                                                            |
| IN      | To specify multiple possible values for a column                                |
