# Ajax

```javascript
url = "http://google.com"

const ajax = new XMLHttpRequest();
ajax.open('GET', url, false);
ajax.send();

console.log(JSON.parse(ajax.response));
```
