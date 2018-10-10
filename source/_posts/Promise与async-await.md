---
title: Promise与async/await
toc: true
date: 2018-10-04 10:52:06
categories:
- Web
tags:
- JavaScript
- ES6
---

## Promise

`Promise`对象代表一个异步操作，有三个状态：

- `pending`进行中
- `fulfilled`已成功
- `rejected`已失败

<!-- more -->

`Promise`对象有两个特点：

- <u>对象的状态不受外界影响</u>。只有异步操作的结果可以决定当前是哪一种状态。
- <u>一旦状态改变，就不会再变</u>。只要状态发生改变（`pending`到`fulfilled`或`pending`到`rejected`），状态就凝固了，一直保持这个结果，这时就称为 `resolved`（已定型）。

`Promise`的缺点：

- 无法取消。一旦新建它就会立即执行，无法中途取消。
- 如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。

- 当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

例子：

```javascript
const getJSON = function(url) {
  // Promise 新建后就会立即执行。
  const promise = new Promise(function(resolve, reject){
    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        // resolve函数的参数除了正常的值以外，还可能是另一个 Promise 实例
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

  });

  return promise;
};

/*
 * then方法可以接受两个回调函数作为参数。 
 * 第一个回调函数在Promise对象的状态变为resolved时调用。
 * 第二个回调函数在Promise对象的状态变为rejected时调用。
 * then方法是定义在原型对象Promise.prototype上的。
 */
getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```

## async/await

async放置在函数前，确保这个函数返回一个promise（返回的不是promise也会被包装成promise）。

await只能在async函数中工作。

promise前面的await关键字能够使JavaScript等待，直到promise处理结束。

```js
async function showAvatar() {
    // read our JSON
    let response = await fetch('/article/promise-chaining/user.json')
    let user = await response.json()
    
    // read github user
    let githubResponse = await fetch(`https://api.github.com/users/${user.name}`)
    let githubUser = await githubResponse.json()
    
    // 展示头像
    let img = document.createElement('img')
    img.src = githubUser.avatar_url
    img.className = 'promise-avatar-example'
    documenmt.body.append(img)
    
    // 等待3s
    await new Promise((resolve, reject) => {
        setTimeout(resolve, 3000)
    })
    
    img.remove()
    
    return githubUser
}
showAvatar()
```

