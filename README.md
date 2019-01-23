# xcx_component
小程序组件实例运用


组件popup.wxml
```html
<!--component/popup.wxml-->
<view class="wx-popup" hidden="{{flag}}">
  <view class='popup-container'>
    <view class="wx-popup-title">{{title}}</view>
    <view class="wx-popup-con">{{content}}</view>
    <view class="wx-popup-btn">
      <text class="btn-no" bindtap='_error'>{{btn_no}}</text>
      <text class="btn-ok" bindtap='_success'>{{btn_ok}}</text>
    </view>
  </view>
</view>
```

组件popup.js
```js
// component/popup.js
Component({
  options: {
    multipleSlots: true // 在组件定义时的选项中启用多slot支持
  },
  /**
   * 组件的属性列表
   */
  properties: {
    title: {            // 属性名
      type: String,     // 类型（必填），目前接受的类型包括：String, Number, Boolean, Object, Array, null（表示任意类型）
      value: '标题'     // 属性初始值（可选），如果未指定则会根据类型选择一个
    },
    // 弹窗内容
    content: {
      type: String,
      value: '内容'
    },
    // 弹窗取消按钮文字
    btn_no: {
      type: String,
      value: '取消'
    },
    // 弹窗确认按钮文字
    btn_ok: {
      type: String,
      value: '确定'
    } 
  },

  /**
   * 组件的初始数据
   */
  data: {
    flag: true,
  },

  /**
   * 组件的方法列表
   */
  methods: {
    //隐藏弹框
    hidePopup: function () {
      this.setData({
        flag: !this.data.flag
      })
    },
    //展示弹框
    showPopup () {
      this.setData({
        flag: !this.data.flag
      })
    },
    /*
    * 内部私有方法建议以下划线开头
    * triggerEvent 用于触发事件
    */
    _error () {
      console.log("sss")
      //触发取消回调
      this.triggerEvent("erroraaa",{'aa':11,'bb':22})
    },
    _success () {
      //触发成功回调
      this.triggerEvent("success");
    }
  }
})
```

组件popup.json
```json
{
  "component": true,
  "usingComponents": {}
}
```

使用界面 index.html
```html
<!--index.wxml-->
<view class="container">
  <view class="userinfo">
    <button bindtap="showPopup"> 点我 </button>
    <button bindtap="pushData" data-id='232' data-name='fff'> 点我111 </button>
  </view>
  <popup id='popup' 
      title='小组件1' 
      content='学会了吗' 
      btn_no='没有'  
      btn_ok='学会了'
      bind:erroraaa="_error1"  
      bind:success="_success">
  </popup> 
</view>
```

使用界面 index.js

```js
//index.js
//获取应用实例
const app = getApp()

Page({
  onReady: function () {
    //获得popup组件
    this.popup = this.selectComponent("#popup");
  },

  showPopup() {
    this.popup.showPopup();
  },

  //取消事件
  _error1(res) {
    console.log('你点击了取消',res);
    this.popup.hidePopup();
  },
  //确认事件
  _success() {
    console.log('你点击了确定');
    this.popup.hidePopup();
  },

  pushData(e){
    console.log("传值：",e)
  }
})
```

使用界面 index.json

```json
{
  "usingComponents": {
    "popup": "/component/popup/popup"
  }
}
```