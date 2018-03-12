##  微信小程序调试的问题


### 腾讯地图api调用

错误： `request`请求的`https://api.map.qq.com` 不在request请求api内。
解决： 
1. 勾选详情中的`不校验安全域名、web-view 域名、TLS 版本以及 HTTPS 证书`
2. 在微信小程序中添加服务器合法域名`https://api.map.qq.com`


### 巧用周期函数更新页面
|属性|类型|描述|
|-----------|-----|------------|
|onLoad	|Function	|生命周期函数--监听页面加载|
|onReady|Function	|生命周期函数--监听页面初次渲染完成|
|onShow	|Function	|生命周期函数--监听页面显示|
|onHide	|Function	|生命周期函数--监听页面隐藏|
|onUnload	|Function	|生命周期函数--监听页面卸载|


要会使用周期函数来进行页面的刷新，当前周期显示当前数据


### this.setData is not a function

- 原因：在回调函数中this的指向会改变为当前上下文
- 解决方案：在需要调用函数的上下文中定义this为指定变量，利用变量来调用函数

``` js
onReady: function(e){
    wx.getLocation({
      type: 'wgs84',
      success: function (res) {
        var latitude = res.latitude
        var longitude = res.longitude
        console.log(qqmapsdk);
        qqmapsdk.reverseGeocoder({
          location: {
            latitude: latitude,
            longitude: longitude
          },
          success: function (res) {
            console.log('----------');
            console.log(this);
            this.setData({
              city: res.result.address_component.city,
            });
          },
          fail: function (res) {
            console.log(res);
          },
          complete: function (res) {
            console.log(res);
          }
        })
      }
    });
  }
```

想要调用`onReady`中的`this.setData`方法，但是会报错在`success callback中没有这样的方法`，这是因为回调函数改变了this的指向。
解决方法是保存`onReady`中的`this`指向。

``` js
onReady: function(e){
    // 利用变量保存this
    const oR_this = this;
    wx.getLocation({
      type: 'wgs84',
      success: function (res) {
        var latitude = res.latitude
        var longitude = res.longitude
        console.log(qqmapsdk);
        qqmapsdk.reverseGeocoder({
          location: {
            latitude: latitude,
            longitude: longitude
          },
          success: function (res) {
            console.log('----------');
            console.log(this);
            // 改用保存的变量
            oR_this.setData({
              city: res.result.address_component.city,
            });
          },
          fail: function (res) {
            console.log(res);
          },
          complete: function (res) {
            console.log(res);
          }
        })
        
      }
    });
  }

```
