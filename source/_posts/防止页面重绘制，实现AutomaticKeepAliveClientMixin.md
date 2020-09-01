---
layout: post
title: 防止页面重绘制，实现AutomaticKeepAliveClientMixin
date: 2019-07-10 15:27:16
toc: true
reward: true
tags: [Flutter,Flutter问题]
categories: [Flutter]
---
[原文](https://blog.csdn.net/weixin_34320159/article/details/87566116 )

### 参考 
https://www.colabug.com/3162835.html
https://stackoverflow.com/questions/53011686/flutter-automatickeepaliveclientmixin-is-not-working-with-bottomnavigationbar62835.html

*Flutter切换tab后保留tab状态 概述 Flutter中为了节约内存不会保存widget的状态,widget都是临时变量。当我们使用TabBar,TabBarView是我们就会发现,切换tab，initState又会被调用一次。
怎么为了让tab一直保存在内存中，不被销毁？
添加AutomaticKeepAliveClientMixin，并设置为true,这样就能一直保持当前不被initState了。*
 <!--more-->
```
class TicketListViewState extends State<TicketListView>
    with AutomaticKeepAliveClientMixin {
  @override
  void initState() {
    super.initState();
  }
 
  @override
  Widget build(BuildContext context) {
    super.build(context);
    return new SmartRefresher(
        enablePullDown: true,
        enablePullUp: true,
        onRefresh: _onRefresh,
        controller: refreshController,
        child: ListView.builder(
          itemCount: _result.length,
          itemBuilder: (context, index) {
            return getItem(_result[index]);
          },
        ));
  }
  //不会被销毁,占内存中
  @override
  bool get wantKeepAlive => true;
}
```
**如果不起作用:**
```
  @override
  Widget build(BuildContext context) {
    super.build(context);//必须添加
   .....
        ));
```
官方解释
```
/// A mixin with convenience methods for clients of [AutomaticKeepAlive]. Used
/// with [State] subclasses.
///
/// Subclasses must implement [wantKeepAlive], and their [build] methods must
/// call `super.build` (the return value will always return null, and should be
/// ignored).
```