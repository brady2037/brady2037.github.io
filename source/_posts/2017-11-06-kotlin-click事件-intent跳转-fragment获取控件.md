---
title: kotlin click事件 intent跳转 fragment获取控件
date: 2017-11-06 15:12:04
categories:
- 技术
- android
- kotlin
tags:
- kotlin
- android
- intent
- fragment
---
> 搬运自CSDN:[ kotlin click事件 intent跳转 fragment获取控件](http://blog.csdn.net/qqduxingzhe/article/details/76474813)

#### click事件 intent跳转 传递参数
``` kotlin
mFloatBtn.onClick {
  val intent = Intent(this@MainContentActivity,MainActivity::class.java)
  startActivity(intent)
}
```

#### 只跳转，无参数传递
``` kotlin
mFloatBtn.onClick { startActivity<MainActivity>() }
```
<!-- more -->
#### 跳转，传参
``` kotlin
mFloatBtn.onClick {
  startActivity<MainActivity>(
          "name" to "MainContent" // key to value
  )
}
```

#### fragment获取控件
kotlin 在activity中，支持无需findviewbyid，控件直接可使用 
但 fragment中，不支持，只好 find控件

``` kotlin
private var mTxt: TextView? = null

override fun onCreateView(inflater: LayoutInflater?, container: ViewGroup?,
            savedInstanceState: Bundle?): View? {

   val view = inflater!!.inflate(R.layout.frag_textview_content, null)
   initView(view)
   setUpViews()
   return view
}

private fun initView(view: View) {
   mTxt = view.find(R.id.mTxt)
}
```

