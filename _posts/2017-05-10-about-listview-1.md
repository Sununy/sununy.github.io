---
layout: post
title:  解决listview层层嵌套的另外一种思路
---

最近项目涉及到订单的优化，比较忙没能及时更新博客，今天就吧自己在自己项目中遇到的listview嵌套的相关解决办法展现给大家。废话不多说，先给大家看下产品给的设计ui图：

<img src = "/resourse/我的订单UI图.jpeg" alt="我的订单UI图.jpeg"/>

这是商品订单列表的一个效果图，下面我们来分析一下这个效果图，就会发现里面涉及到了层层的嵌套，来看下面一张图：

<img src = "/resourse/我的订单分析图.png" alt="我的订单分析图.png"/>

上面的标志我想大家都能看懂，其实里里外外涉及到三层的嵌套。那我们来分析一下，应该如何去实现呢？


 第一层：我想这个是毋庸置疑的要用listview或者RecycleView或者Gridview吧，这三个你可以选一个，由于我们的工程还是eclipse在开发，我就用的项目中自带XlistView，   不过不管用什么都可以的。


第二层：仔细一看这个第二层还是要用listview的啊，是不是瞬间就有一种懵逼的感觉啊。


第三层：fuck，咋一看又要用个gridview嵌套在里面呢？


这样一分析，是不是瞬间就不想写代码了呢 ？


山穷水复疑无路，柳暗花明又一村。当进入死胡同的时候，何妨不转换下思路呢？我当时想的就是不用层层嵌套，我动态的添加布局应该也能实现，敢想敢做，我一颗也没耽误就去码代码了。

第一层的item的布局先写出来：

``` bash
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical" >  
  
    <View  
        android:layout_width="match_parent"  
        android:layout_height="8dp"  
        android:background="#F7F7F7" />  
  
    <LinearLayout  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:orientation="horizontal"  
        android:paddingBottom="8dp"  
        android:paddingTop="8dp" >  
  
        <View  
            android:layout_width="4dp"  
            android:layout_height="16dp"  
            android:background="#FF4B0F" />  
  
        <TextView  
            android:id="@+id/tv_order_time"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_marginLeft="12dp"  
            android:text="2017-4-7"  
            android:textSize="13sp"  
            android:textStyle="bold" />  
  
        <TextView  
            android:layout_width="0dp"  
            android:layout_height="wrap_content"  
            android:layout_weight="1" />  
  
        <TextView  
            android:id="@+id/tv_order_paynum"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_marginRight="8dp"  
            android:text="支付订单号：SS22344dada4445"  
            android:textStyle="bold"  
            android:textSize="13sp" />  
    </LinearLayout>  
  
<span style="color:#FF0000;"><strong> <span style="font-size:18px;">   <LinearLayout  
        android:id="@+id/linear_container"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:orientation="vertical" >  
    </LinearLayout></span></strong></span>  
  
    <TextView  
        android:id="@+id/tv_line"  
        android:layout_width="match_parent"  
        android:layout_height="0.5dp"  
        android:layout_marginLeft="8dp"  
        android:layout_marginRight="8dp"  
        android:background="#E7E0E3" />  
  
    <LinearLayout  
        android:id="@+id/linear_money"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:layout_marginTop="8dp"  
        android:gravity="end"  
        android:orientation="horizontal" >  
  
        <TextView  
            android:id="@+id/tv_goods_num"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_centerInParent="true"  
            android:layout_marginLeft="12dp"  
            android:text="共7件商品"  
            android:textColor="#767480" />  
  
        <TextView  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_centerInParent="true"  
            android:layout_marginLeft="12dp"  
            android:text="合计："  
            android:textColor="#767480" />  
  
        <TextView  
            android:id="@+id/tv_goods_total_money"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_centerInParent="true"  
            android:layout_marginLeft="4dp"  
            android:paddingRight="8dp"  
            android:text="￥ 3850.00"  
            android:textStyle="bold" />  
  
        <TextView  
            android:id="@+id/tv_goods_transfee"  
            android:visibility="gone"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_centerInParent="true"  
            android:layout_marginRight="8dp"  
            android:text="（含运费 ￥ 5）"  
            android:textColor="#767480" />  
    </LinearLayout>  
  
    <LinearLayout  
        android:id="@+id/linear_order_button"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:layout_marginTop="8dp"  
        android:gravity="end"  
        android:orientation="horizontal"  
        android:paddingBottom="12dp"  
        android:paddingLeft="4dp"  
        android:paddingRight="8dp"  
        android:paddingTop="4dp" >  
  
        <TextView  
            android:id="@+id/tv_order_cancle"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_marginRight="@dimen/size_10dp"  
            android:background="@drawable/shape_light_bg"  
            android:paddingBottom="6dp"  
            android:paddingLeft="8dp"  
            android:paddingRight="6dp"  
            android:paddingTop="6dp"  
            android:text="取消订单"  
            android:textColor="#9291A4"  
            android:textSize="@dimen/text_size14" />  
  
        <TextView  
            android:id="@+id/tv_order_check"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:background="@drawable/shape_light_bg"  
            android:paddingBottom="6dp"  
            android:paddingLeft="12dp"  
            android:paddingRight="10dp"  
            android:paddingTop="6dp"  
            android:text="查看订单"  
            android:textColor="#9291A4"  
            android:textSize="@dimen/text_size14" />  
  
        <TextView  
            android:id="@+id/tv_order_pay"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_marginLeft="10dp"  
            android:background="@drawable/shape_eacrad_pay"  
            android:paddingBottom="6dp"  
            android:paddingLeft="8dp"  
            android:paddingRight="6dp"  
            android:paddingTop="6dp"  
            android:text="立即付款"  
            android:textColor="#FF6893"  
            android:textSize="@dimen/text_size14" />  
    </LinearLayout>  
    <ImageView  
        android:id="@+id/order_end_iv"  
        android:layout_width="match_parent"  
        android:layout_height="30dp"  
        android:layout_marginBottom="8dp"  
        android:contentDescription="@null"  
        android:src="@drawable/ic_nomore" />  
</LinearLayout>

```

上面的额布局代码中有一段我标红的不知道大家看见没？这个只是一个布局的容器，现在里面什么都没有，只是待会再代码中动态添加子布局的父容器而已.

大家在看第二层嵌套的布局：`add_order_view：`

``` bash 
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical" >  
  
    <TextView  
        android:id="@+id/tv_order_ordernum"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:background="#F7F7F7"  
        android:paddingTop="8dp"  
        android:paddingRight="8dp"  
        android:paddingLeft="16dp"  
        android:paddingBottom="8dp"  
        android:text="订单编号：SS22344dada4445"  
        android:textColor="#767480"  
        android:textSize="14sp" />  
  
    <LinearLayout  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:gravity="center_vertical"  
        android:orientation="horizontal"  
        android:paddingTop="8dp"  
        android:paddingRight="8dp"  
        android:paddingLeft="16dp"  
        android:paddingBottom="8dp" >  
  
        <ImageView  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:src="@drawable/bq" />  
  
        <TextView  
            android:id="@+id/tv_order_merchant_name"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_marginLeft="8dp"  
            android:text="美罗城购物中心"  
            android:textColor="#767480"  
            android:textSize="14sp" />  
  
        <TextView  
            android:layout_width="0dp"  
            android:layout_height="wrap_content"  
            android:layout_weight="1" />  
  
        <TextView  
            android:id="@+id/tv_order_status"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_marginLeft="16dp"  
            android:text="待支付"  
            android:textColor="#FF6893"  
            android:textSize="14sp"  
            android:textStyle="bold" />  
    </LinearLayout>  
  
<span style="font-size:18px;color:#FF0000;"><strong>    <HorizontalScrollView  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:scrollbars="@null"  
        android:padding="8dp" >  
  
        <LinearLayout  
            android:id="@+id/linear_group"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:orientation="horizontal" >  
  
        </LinearLayout>  
    </HorizontalScrollView></strong></span>  
  
</LinearLayout> 
```

啥都没有是吧？同时也要看我布局标红放大的部分，HorizontalScrollView能保证图片的水平滚动，里面的额linearlayout也是动态添加布局的容器。


接下来就是最里面一层，其实很简单的布局就是一张图片，我也贴出来给大家看看：

`add_order_view_image：`

``` bash 
<?xml version="1.0" encoding="utf-8"?>  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:padding="8dp" >  
  
    <RelativeLayout  
        android:id="@+id/rela_1"  
        android:layout_width="80dp"  
        android:layout_height="80dp" >  
  
        <ImageView  
            android:id="@+id/iv_goods"  
            android:layout_width="80dp"  
            android:layout_height="80dp"  
            android:layout_marginTop="6dp"  
            android:src="@drawable/df2" />  
  
        <ImageView  
            android:id="@+id/iv_gift"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_marginTop="6dp"  
            android:src="@drawable/goods_tag" />  
  
  
        <TextView  
            android:id="@+id/tv_status"  
            android:layout_width="80dp"  
            android:layout_height="wrap_content"  
            android:layout_alignBottom="@+id/iv_goods"  
            android:layout_centerHorizontal="true"  
            android:layout_centerVertical="true"  
            android:text="已售罄"  
            android:background="#A6000000"  
            android:gravity="center"  
            android:textColor="@color/White"  
            android:textSize="12sp" />  
    </RelativeLayout>  
  
        <TextView  
            android:id="@+id/tv_number"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:background="@drawable/shape_order_list_circle"  
            android:layout_marginLeft="-12dp"  
            android:layout_toRightOf="@+id/rela_1"  
            android:padding="4dp"  
            android:text="17"  
            android:textColor="@color/White"  
            android:textSize="12sp" />  
       <TextView  
            android:id="@+id/tv_descripe"  
            android:layout_width="200dp"  
            android:layout_height="wrap_content"  
            android:layout_toRightOf="@+id/rela_1"  
            android:layout_centerInParent="true"  
            android:padding="4dp"  
            android:layout_marginRight="30dp"  
            android:visibility="gone"  
            android:text="我是商品"  
            android:textSize="12sp" />  
</RelativeLayout>
```



