# DOM API

이 글은 공부할 때 사용했던 DOM API를 정리했습니다.



## DOM API란





## getElementById

HTML태그 중 id를 선택합니다.

```html
<html>
<body>
  <div id='root'></div>
</body>
</html>
```

```javascript
document.getElementById('root');
```

## createElement

HTML 태그를 생성합니다.

```
const ul = document.createElement('ul');
```

## 이벤트 등록

웹브라우저에게 이벤트 등록을 요청하는 함수입니다. 버튼 클릭 등이 이벤트에 해당합니다. <mark style="color:red;">이벤트가 발생하면 웹브라우저가 이벤트에 등록한 함수를 호출</mark>합니다.

```
addEventListener(<이벤트 이름>, <자바스크립트 함수>)
```



## location 객체



### hash

{% hint style="info" %}
참고자료: [https://developer.mozilla.org/ko/docs/Web/API/URL/hash](https://developer.mozilla.org/ko/docs/Web/API/URL/hash)
{% endhint %}

location.hash를 사용하면 현재 hash값을 가져옵니다. 만약, hash(#)만 있을 경우 빈값으로 인식합니다.

```
if(location.hash === '')
  ...
```
