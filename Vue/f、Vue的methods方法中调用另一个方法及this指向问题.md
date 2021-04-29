# Vue的methods方法中调用另一个方法及this指向问题

## 1.问题概述

一、在vue方法中调用vue的另一个1.问题概述方法如何调用

```js
this.$options.methods.方法名称();
```

二、存在的问题

**被调用方法中的this指向的不再是vue实例，而是调用方法。**

## 2.解决方案

###### onSearch调用了getTableData
在调用时，只需要在onsearch方法中
将vue实例赋值给that
再将that作为参数传入getTabaleData中
getTableData将that作为vue实例使用即可。

```js
methods:{
	onSearch() {
	      console.log(this.searchSelectValue)
	      const that = this
	      this.$options.methods.getTableData(that)
	    },
	getTableData(that) {
	      that.loading = true
	      console.log(that.currentPage)
	      axios({
	        method: 'post',
	        url: 'api/room/search',
	        data: {
	          likeName: that.likeName,
	          searchSelectValue: that.searchSelectValue,
	          currentPage: that.currentPage,
	          pageSize: that.pageSize
	        }
	      }).then(res => {
	        that.tableData = res.data.records
	        that.total = res.data.total
	        that.pages = res.data.pages
	        that.loading = false
	      })
	    }
}
```

