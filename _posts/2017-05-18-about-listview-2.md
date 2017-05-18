---
layout: post
title: 解决listview层层嵌套的另外一种思路(细节说明)
---

上一篇提到用动态添加布局来解决listview的层层嵌套，由于这种做法我用到过很多次，所以做起来也很顺手，但是这一次的需求和上一次的稍微有点不同，导致最后快要提测的时候功能还没实现，当时就急了。。。。

先给大家看下上次实现的效果:

<img src="/resourse/about-listview(1).gif" alt="about-listview(1).gif" />


由于上一次的gif没有给大家展示横向的滑动，今天给补上。
但是现在我想问大家如果点击横向滑动的那几个小图片，会跳转到哪呢？
下面再给大家展示：

<img src="/resourse/about-listview(2).gif" alt="about-listview(2).gif" />

 大家可以看到不管点击哪个图片都是跳到订单详情界面，为什么呢？因为点击事件是对整个的linearlayout做的处理，如果想对单独的imageview进行点击事件呢？


可能小伙伴会直接说直接对imageview加上点击事件不就得了？确实是加上点击事件可以跳转，但是如果问题这么简单我也不会问大家了，我问大家一个问题，你如何知晓你是点击的哪个位置？大家看上面的示意图，第一行有6个图片，你随便点击一张图片，你可能知道点击的是哪一张，但是程序知道你点击的位置吗？如果你要把你点击的这张图片背后的信息传递到下个界面去，怎么办呢？其实，这就是困扰我的问题，当时没有第一时间想到解决办法，心里很慌，最后冷静下来才找到解决方案，下面大家再看效果：


 大家看的见点击不同的售后状态跳转到售后详情界面的展现也是不一样的，那就说明已经找到了点击的位置，同样也正确的把相关的信息传递过去了。
 那具体的是怎么实现的呢？其实就是在上一次的代码中加了一句代码：

```bash
for (int j = 0; j < productList.size(); j++) {  
                View viewTwo = mInflater.inflate(R.layout.add_order_view_image, null);  
                ImageView iv_goods = (ImageView) viewTwo.findViewById(R.id.iv_goods);  
                ImageView iv_gift = (ImageView) viewTwo.findViewById(R.id.iv_gift);  
                TextView tv_number = (TextView) viewTwo.findViewById(R.id.tv_number);  
                final TextView tv_status = (TextView) viewTwo.findViewById(R.id.tv_status);  
                TextView tv_descripe = (TextView) viewTwo.findViewById(R.id.tv_descripe);  
                if (productList.size() == 1) {  
                    tv_descripe.setVisibility(View.VISIBLE);  
                    tv_descripe.setText(productList.get(j).getProductName());  
                } else {  
                    tv_descripe.setVisibility(View.GONE);  
                }  
                ImageUtils.display(R.drawable.df2, productList.get(j).getProductImage(), iv_goods);  
                if ("0".equals(productList.get(j).getIsGift())) { // 不是赠品  
                    iv_gift.setVisibility(View.GONE);  
                } else {  
                    iv_gift.setVisibility(View.VISIBLE);  
                }  
                if (productList.get(j).getProductNumber().equals("1")) {  
                    tv_number.setVisibility(View.GONE);  
                } else {  
                    tv_number.setVisibility(View.VISIBLE);  
                    tv_number.setText(productList.get(j).getProductNumber());  
                }  
  
                if ("10".equals(productList.get(j).getOrderItemStatus())) {  
                    tv_status.setVisibility(View.VISIBLE);  
                    tv_status.setText("售后待处理");  
                    <span><span>tv_status.setTag(j)</span></span>  
                } else if ("11".equals(productList.get(j).getOrderItemStatus())) {  
                    tv_status.setVisibility(View.VISIBLE);  
                    tv_status.setText("待买家退货");  
                    <span><span>tv_status.setTag(j)</span></span>  
                } else if ("12".equals(productList.get(j).getOrderItemStatus())) {  
                    tv_status.setVisibility(View.VISIBLE);  
                    tv_status.setText("待卖家确认");  
                    <span style="font-size:18px;color:#FF0000;"><strong></strong></span><span><span>tv_status.setTag(j)</span></span>  
                } else if ("13".equals(productList.get(j).getOrderItemStatus())) {  
                    tv_status.setVisibility(View.VISIBLE);  
                    tv_status.setText("理赔成功");  
                    <span><span>tv_status.setTag(j)</span></span><span style="color:#FF0000;"><strong><span style="font-size:18px;"></span></strong></span>  
                } else if ("14".equals(productList.get(j).getOrderItemStatus())) {  
                    tv_status.setVisibility(View.VISIBLE);  
                    tv_status.setText("维权完成");  
                    <span><span>tv_status.setTag(j)</span></span>  
                } else if ("15".equals(productList.get(j).getOrderItemStatus())) {  
                    tv_status.setVisibility(View.VISIBLE);  
                    tv_status.setText("理赔中");  
                    <span><span>tv_status.setTag(j)</span></span>  
                } else {  
                    tv_status.setVisibility(View.GONE);  
                }  
                tv_status.setOnClickListener(new OnClickListener() {  
  
                    @Override  
                    public void onClick(View v) {  
                        switchStatusTwo(v);  
                        <span style="font-size:18px;"><strong><span style="color:#FF0000;"></span></strong></span>int nowPosition = (int) tv_status.getTag();  
                        Intent intent = new Intent(mContext, RefundContentActivity.class);  
                        intent.putExtra("orderNo", mListData.get(position).get(0).getOrderNo());// getProductList().get(position).getOrderNo()  
                        intent.putExtra("OrderItemId",  
                                mListData.get(position).get(0).getProductList().get(nowPosition).getOrderItemId());  
                        intent.putExtra("type", "001");// 商品type  
                        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);  
                        mContext.startActivity(intent);  
                    }  
                });  
                linear_group.addView(viewTwo);  
            }  
```

大家看见没就是这两句代码解决了我的困扰，动态的添加布局的时候肯定是需要遍历的，在遍历的时候为每一个控件设置一个标签`tv_status.setTag(j)`，那么实际上每一个图片对应的位置就是循环遍历的`int `值 j；`int nowPosition = (int) tv_status.getTag();`在用的时候拿出来就可以了；

好了，今天的博客就到这，算是对上一遍的细节补充，同时也记录一下成长的过程!