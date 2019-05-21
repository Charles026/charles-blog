
---

title: 小程序初识--仿Vue.js中文论坛

date: 2017-08-15 17:06:02 +0800

tags: vue,wechat,前端

categories: 前端

---


小程序的秉承着 “体验极简化”、“用完即走” 的理念应运而生。许久之前，我就对小程序的个人版的推出满怀期待，想做点小玩意儿体验一下小程序，而后又忙于工作，没有付诸行动，如今在找工作期间得闲，花了4天，基于[Vue.js中文论坛]()提供的开发接口，开发了论坛的小程序版。<br /><!-- more -->
<a name="e245b6b1"></a>
### 1. 主要功能：

- 论坛帖子查看、收藏
- 登陆发布新帖子
- 评论和消息管理
<a name="3fefb115"></a>
### 2. 目录结构
```
|——README.md          <= 项目介绍
|——app.js             <= 主要逻辑文件
|——app.json           <= 全局配置文件
|——app.wxss           <= 公共样式文件 
|——component          <= 组件库
|  |——timeTeanslate   <= 时间格式转换组件
|  |——wxParse         <= 富文本解析组件
|  |——weui            <= weui样式库
|——pages              <= 页面文件
|  |——detail
|  |——index           
|  |——user
|  |——api.js          <= 接口文件
|  |——const.js        <= 常量定义文件
|——static             <= 静态资源
|  |——image
```
<a name="d610751b"></a>
### 3. 效果演示
3.1 主页<br />![vue(2).png](https://cdn.nlark.com/yuque/0/2019/png/155457/1547265459399-7c9daac9-5265-4898-ba0e-960ac50f202f.png#align=left&display=inline&height=1327&name=vue%282%29.png&originHeight=1334&originWidth=750&size=244969&width=746)<br />3.2 文章详情<br />![vue(4).png](https://cdn.nlark.com/yuque/0/2019/png/155457/1547265471980-b3850dc9-d6b9-4ef2-bc67-602b8bf1b63a.png#align=left&display=inline&height=1327&name=vue%284%29.png&originHeight=1334&originWidth=750&size=154779&width=746)<br />3.3 登陆<br />![vue(1).png](https://cdn.nlark.com/yuque/0/2019/png/155457/1547265483018-9947b743-9f3e-4c05-9ea9-81e1a1b7d27e.png#align=left&display=inline&height=545&name=vue%281%29.png&originHeight=545&originWidth=320&size=7088&width=320)<br />3.4 个人中心<br />![vue(6).png](https://cdn.nlark.com/yuque/0/2019/png/155457/1547265510110-ca9be02e-1954-45f4-b49b-bcf024540bf2.png#align=left&display=inline&height=548&name=vue%286%29.png&originHeight=548&originWidth=319&size=12327&width=319)<br />3.5 发布文章<br />![vue(3).png](https://cdn.nlark.com/yuque/0/2019/png/155457/1547265526119-491b42c1-10c6-4fd4-8279-a8992f5c5c52.png#align=left&display=inline&height=1327&name=vue%283%29.png&originHeight=1334&originWidth=750&size=47954&width=746)<br />3.6 我的收藏<br />![vue(5).png](https://cdn.nlark.com/yuque/0/2019/png/155457/1547265549463-6798d862-a71c-495d-b5dd-649343ecc426.png#align=left&display=inline&height=1327&name=vue%285%29.png&originHeight=1334&originWidth=750&size=63813&width=746)<br />3.7 我的消息<br />![vue(7).png](https://cdn.nlark.com/yuque/0/2019/png/155457/1547265560772-4d23749b-b636-4caa-a39d-c6e0afc1937d.png#align=left&display=inline&height=547&name=vue%287%29.png&originHeight=547&originWidth=322&size=16121&width=322)
<a name="76304aaf"></a>
### 4.总结
<a name="0bd99f9a"></a>
#### 4.1 小程序的基本功能

- 小程序的文件类型：
```
js ---------- 主要逻辑
json -------- 项目配置文件，负责窗口颜色等等
wxml ------- 类似HTML文件
wxss ------- 类似CSS文件
```

- 创建一个页面：

在page文件夹下创建一个页面的文件夹，里面新建4个必要的基本文件，然后再app.json中配置路由：
```
{
  "pages":[
    "pages/index/index",
    "pages/detail/detail",
    "pages/user/user/user",
    "pages/user/login/login",
    "pages/user/message/message",
    "pages/user/collect/collect",
    "pages/user/newtopic/newtopic"
  ]
}
```

- 发起一个API请求：
```
wx.request({
      url: API.API.Post_mark_all,    // 请求url
      method: 'POST',                // 请求方式
      data: {
        'accesstoken': token         // 参数
      },
      success: function (res) {
        if (res.data.success) {
          wx.showToast({
            title: '标记成功',
            icon: 'success',
            duration: 1000
          })
          that.getMessage()
        }else {
          wx.showToast({
            title: res.data.error_msg,
            icon: 'success',
            duration: 1000
          })
```

- 本地缓存<br />
由于没有类似Vuex的状态管理工具，一些公用的数据保存在本地缓存中，例如登陆的状态、accesstoken、用户的信息等。<br />
操作本地缓存的方法有：
```
// 设置
wx.setStorage()   //异步
wx.setStorageSync() //同步
// 获取
wx.getStorage()   //异步
wx.getStorageSync() //同步
//清除
wx.clearStorage()   //异步
wx.clearStorageSync() //同步
```

- 页面的生命周期的方法：
```
onLoad   //页面加载
onReady  //页面渲染完成
onShow   //页面显示
onHide   //页面隐藏
onUnload  //页面卸载
```
<a name="0e0387f2"></a>
#### 4.2 未实现的功能
由于时间原因,以下2个功能尚未完成:

- 针对于某个特定用户的回复功能<br />
- 点赞功能<br />
<a name="ec036747"></a>
#### 4.3 有待优化的地方

- 文章的富文本解析<br />
- 小程序的状态管理<br />
<a name="7ad31752"></a>
#### 4.4 个值得推荐的组件和库

- wxParse 富文本解析组件
- weui   微信官方UI库
<a name="9415a826"></a>
#### 最后
项目的github地址：[https://github.com/Ghostdar/wechat-weapp-vueForum]()<br />欢迎 `star!`

