---
layout: post
title: "Email电话号码加密"
date: 2021-12-21 23:18:40
---

生成新编码
```
<?php

function encodeEmail($email, $key=0) {
	$chars = str_split($email);
	$string = '';
	$key = $key ? $key : rand(10, 99);
	foreach ($chars as $value) {
		$string .= sprintf("%02s", dechex(ord($value)^$key));
	}
	return dechex($key).$string;
}

echo encodeEmail("sssssssssss");

?>
```
解析编码生成正常编码,js文件内容：
```
!function(){"use strict";function e(e){try{if("undefined"==typeof console)return;"error"in console?console.error(e):console.log(e)}catch(e){}}function t(e){return d.innerHTML='<a href="'+e.replace(/"/g,"&quot;")+'"></a>',d.childNodes[0].getAttribute("href")||""}function r(e,t){var r=e.substr(t,2);return parseInt(r,16)}function n(n,c){for(var o="",a=r(n,c),i=c+2;i<n.length;i+=2){var l=r(n,i)^a;o+=String.fromCharCode(l)}try{o=decodeURIComponent(escape(o))}catch(u){e(u)}return t(o)}function c(t){for(var r=t.querySelectorAll("a"),c=0;c<r.length;c++)try{var o=r[c],a=o.href.indexOf(l);a>-1&&(o.href="mailto:"+n(o.href,a+l.length))}catch(i){e(i)}}function o(t){for(var r=t.querySelectorAll(u),c=0;c<r.length;c++)try{var o=r[c],a=o.parentNode,i=o.getAttribute(f);if(i){var l=n(i,0),d=document.createTextNode(l);a.replaceChild(d,o)}}catch(h){e(h)}}function a(t){for(var r=t.querySelectorAll("template"),n=0;n<r.length;n++)try{i(r[n].content)}catch(c){e(c)}}function i(t){try{c(t),o(t),a(t)}catch(r){e(r)}}var l="/cdn-cgi/l/email-protection#",u=".__cf_email__",f="data-cfemail",d=document.createElement("div");i(document),function(){var e=document.currentScript||document.scripts[document.scripts.length-1];e.parentNode.removeChild(e)}()}();
!function(){"use strict";function z(z){try{if("undefined"==typeof console)return;"error"in console?console.error(z):console.log(z)}catch(z){}}function t(z){return d.innerHTML='<a href="'+z.replace(/"/g,"&quot;")+'"></a>',d.childNodes[0].getAttribute("href")||""}function r(z,t){var r=z.substr(t,2);return parseInt(r,16)}function n(n,c){for(var o="",a=r(n,c),i=c+2;i<n.length;i+=2){var l=r(n,i)^a;o+=String.fromCharCode(l)}try{o=decodeURIComponent(escape(o))}catch(u){z(u)}return t(o)}function c(t){for(var r=t.querySelectorAll("a"),c=0;c<r.length;c++)try{var o=r[c],a=o.href.indexOf(l);a>-1&&(o.href="tel:"+n(o.href,a+l.length))}catch(i){z(i)}}function o(t){for(var r=t.querySelectorAll(u),c=0;c<r.length;c++)try{var o=r[c],a=o.parentNode,i=o.getAttribute(f);if(i){var l=n(i,0),d=document.createTextNode(l);a.replaceChild(d,o)}}catch(h){z(h)}}function a(t){for(var r=t.querySelectorAll("template"),n=0;n<r.length;n++)try{i(r[n].content)}catch(c){z(c)}}function i(t){try{c(t),o(t),a(t)}catch(r){z(r)}}var l="/cdn-cgi/l/tel-protection#",u=".__cf_tel__",f="data-cftel",d=document.createElement("div");i(document),function(){var z=document.currentScript||document.scripts[document.scripts.length-1];z.parentNode.removeChild(z)}()}();

```

页面写法
```
<a href="/cdn-cgi/l/email-protection#eb988e999d82888eab9e988e333298c5888486"><span class="__cf_email__" data-cfemail="2a594f583335c43494f6a5f594f46535904494547">[email&#160;protected]</span></a>

<a href="/cdn-cgi/l/tel-protection#5e7566686f666e6e6f686e6663333c6a6d"><span class="__cf_tel__" data-cftel="577c6f61666333f67676661676f656364">[tel&#160;protected]</span></a>
```

原文：
https://cloud.tencent.com/developer/article/1737585
