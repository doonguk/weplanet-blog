
> Filter 함수는 이러터블의 원소들에서 특정한 원소들만 걸러내고 싶을 때 사용한다.



### 1. 구현

```javascript
  const products = [
     {name : '안경', price : 1000},
     {name : '안경2', price : 2000},
     {name : '안경3', price : 3000},
     {name : '안경4', price : 4000},
     {name : '안경5', price : 5000},
   ]
  
  const filter = () => {
    let under3000 = []
    for(const a of products){
      if(a.price < 3000) under3000.push(a) 
    }
    return under3000
  }
  
  log(...filter) // {name : '안경', price : 1000} {name : '안경2', price : 2000 }
```

위 처럼 이터러블의 원소들에서 특정한 원소만 걸러내고 싶을 때 사용한다.

이를 좀 더 추상화 시켜보자.

```javascript
const filter = (f, iter) => {
  let res = []
  for(const a of iter){
    if(f(a)) res.push(a)
  }
  return res
}

log(...filter(p => p.price < 3000, products)) 
// {name : '안경', price : 1000} {name : '안경2', price : 2000 }
```

filter 함수의 파라미터로 어떻게 원소를 필터링 할지 체크하는 역할을 갖는 보조함수와 이터러블을 받는다.

특징 이라면 map, reduce 함수와 비슷하게 필터링하는 기능을 보조함수에 위임 했다는 것이다.

예제로는,

```javascript
log(...filter(a => a%2, function *(){
  yield 1
  yield 2
  yield 3
  yield 4
  yield 5
}())) // 1 3 5
```


> reduce 함수는 이러터블의 원소들을 다 더해서 하나의 값으로 만들고 싶을 때 사용한다.



### 1. 구현

```javascript
const nums = [1,2,3,4,5]

const reducer = () => {
  let total = 0;
  for(const a of nums){
    total = total + a
  }
  log(total)
}
```

위와 같은 기능을 하는 함수가 reduce 이다.

외부 인터페이스에서는 reduce 함수가 어떻게 사용될까?

```javascript
const add = (a,b) => a+b
log(reduce(add, 0, [1,2,3,4,5])) // 15
```

외부 인터페이스에서는 보조함수, 시작값, 이터러블(well-formed 이터레이터) 을 인자로 받는다. 이 reduce함수가 내부적으로 동작하는 방식은

```javascript
log(add(add(add(add(add(0,1),2),3),4),5))
```

0을 시작값으로 하여 재귀적으로 add 함수가 호출되어 값을 누적시키는 구조이다. 

이 구조를 구현 해보자.

```javascript
const reduce = (f, acc, iter) => {
  for(const a of iter){
    acc = f(acc, a)
  }
  return acc
}
```

코드를 보면 map, filter와 비슷하게 기존에 total + a 로 값을 직접 계산 해주던 부분( **어떻게 값을 축약할지 하는 부분을** )을 보조함수 에게 위임 하였다.

자바스크립트에서는 acc값이 없어도 reduce함수가 사용하도록 구현 되어있는데 원리는 이렇다.

```javascript
log(reduce(add, 1, [2,3,4,5]))
```

위와 같이 이터러블에서 첫번째 원소를 자동으로 acc로 설정하여 동작하는게 그 원리이다. 그럼 이를 위에서 작성한 코드에 구현해보자.

```javascript
const reduce = (f, acc, iter) => {
  if(!iter){ 
    iter = acc[Symbol.iterator]() // acc가 없다면 2번째 파라미터로 전달된 acc가 이터러블
    acc = iter.next().value
  }
  for(const a of iter){
    acc = f(acc, a)
  }
  return acc
}
```

초기값이 없다면 파라미터로 온 acc 값이 이터러블 인데, 이 이터러블로 이터레이터를 생성한뒤 next() 함수를 호출하여 초기값을 설정해 주었다. 

마지막으로 object 배열에서 가격에 해당하는 키의 value 값 을 모두 더하는 예제이다.

```javascript
  const products = [
     {name : '안경', price : 1000},
     {name : '안경2', price : 2000},
     {name : '안경3', price : 3000},
     {name : '안경4', price : 4000},
     {name : '안경5', price : 5000},
   ]

   const reduce = (f, acc, iter) => {
     if(!iter){
       iter = acc[Symbol.iterator]()
       acc = iter.next().value
     }
     for(const a of iter){
       acc = f(acc, a)
     }
     return acc
   }

   log(reduce(
       (total_price, p) => total_price + p.price,
       0,
       products
   ))
```