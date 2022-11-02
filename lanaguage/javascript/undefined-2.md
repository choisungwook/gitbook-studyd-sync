# 클린코드

{% hint style="danger" %}
자바스크립트를 이렇게 작성하면 좋을 것 같다라는 개인적 생각이므로 여러분 의견과 틀릴 수있습니다.&#x20;
{% endhint %}

## HTML 태그를 생성하는 DOM API사용 자제

자바스크립트로 HTML 태그를 생성하는 로직이 많아진다면, 시간이 지나 내가 보거나 다른 사람이 봤을 때 전체적인 태그 구조를 이해하기 힘듭니다.&#x20;

그러므로 DOM API를 사용하여 HTML태그를 만드는 것보다 HTML 태그를 문자열로 감싸는 것이 좋다고 생각합니다.



### 시간이 지나면 이해하기 힘든 코드

```javascript
div = document.createElement('div')

root = document.getElementById('root');
root.innertHTML(div)
```



### 읽기 좋은 코드

```javascript
root = document.getElementById('root');
root.innertHTML('<div></div>')
```
