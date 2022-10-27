### 自动发弹幕

```js
const area = document.getElementsByClassName('ChatSend-txt')[0]
const btn = document.getElementsByClassName('ChatSend-button ')[0]
const danmu = '🌺꧁༺๑好🌺♬💖✨💖听๑༻꧂🌺♬🌺꧁༺๑好🌺♬💖✨💖听๑༻꧂🌺♬'
let interval
function start()
{
interval = setInterval(function(){
area.value = danmu
btn.click()
},1000)
}
function stop()
{
clearInterval(interval)
}
start()
```



### 视频倍速

```js
document.getElementsByTagName("video")[0].playbackRate = 10
```

如果嵌套在iframe中

```js
_iframe = document.getElementById('contentFrame').contentWindow;
_div=_iframe.document.getElementsByTagName("video")[0].playbackRate=10;
```

