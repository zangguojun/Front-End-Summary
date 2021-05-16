## `axios`

```js
axios.get(`/api1/search/users2?q=${keyWord}`).then(
    response => {
        PubSub.publish('res',{
            isLoading:false,
            users:response.data.items
        })
    },
    error => {
        PubSub.publish('res',{
            isLoading:false,
            errMsg:error.message
        })
    }
)
```

## `fetch`

#### 未优化

```js
fetch(`/api1/search/users2?q=${keyWord}`)
    .then(
    response => {
        console.log('连接服务器成功！');
        return response.json()
    },
    error => {
        console.log('连接服务器失败！',error);
        return new Promise()
    }
).then(
    response => {
        console.log('获取数据成功！');
        console.log(response);
    },
    error => {
        console.log('获取数据失败！',error);
    }
)
```

#### 优化①

```js
fetch(`/api1/search/users2?q=${keyWord}`)
    .then(
    response => {
        console.log('连接服务器成功！');
        return response.json()
    }
).then(
    response => {
        console.log('获取数据成功！');
        console.log(response);
    }
).catch(
    error => {
        console.log('失败！',error);
    }
)
```

#### 优化②

> 使用了`await`，需要在外层函数前加`async`

```js
try{
    const response = await fetch(`/api1/search/users2?q=${keyWord}`)
    const data = await response.json()
    PubSub.publish('res',{
        isLoading:false,
        users:data.items
    })
} catch (error) {
    PubSub.publish('res',{
        isLoading:false,
        errMsg:error.message
    })
}
```

