# ViewPager中RecyclerView的定位



ViewPager中每一个page是一个RecyclerView，或者外加一下其他的view比较常见，这里的例子是一个page嵌套一个RecyclerView的情况。

### 定位逻辑

定位包含两个部分，一个是tab的定位，其次才是RecyclerView的item定位。因为RecyclerView是一个page的内容，ViewPager在内存一般是左+右+当前三个page，所以RecyclerView的定位要在tab的定位之后（此时才有要定位的page）。

```java
switchToTab(i); //定位到tab逻辑简单，普通的TAB控件都有，不展开讲
//post就是为了RecyclerView的定位在tab定位之后，确保对应的page已经生成了
viewPager.post(new Runnable() {
    @Override
    public void run() {
        scrollToSpecItemWithClick(tabName, itemId);
    }
});


private void scrollToSpecItemWithClick(String tabName, String itemId) {
    TabData item = getCurrentTabMaterial(tabName);
    if (item != null) {
        List<RVItem> itemList = item.materialList;
        if (itemList == null) {
            return;
        }
        for (int i = 0; i < itemList.size(); i++) {
            RVItem rvItem = itemList.get(i);
         		//找到对应的item位置
            if (rvItem != null && itemId != null && itemId.equals(rvItem.id)) {
              	//PosPageView就是一个普通的包含RecyclerView的layout。
                PosPageView pageView = viewPagerAdapter.getCurrentPageView();
                if (pageView != null) {
                  	//调用PageView中的定位模拟点击方法
                    pageView.scrollToPosWithClick(i);
                }
                return;
            }
        }
    }
}
```

```java
//PageView中的定位代码
public void scrollToPosWithClick(final int pos) {
	if (mRecyclerView != null) {
        if (gridLayoutManager != null) {
            //mRecyclerView.scrollToPosition(pos);如果没有layoutManager，可以用这个代替定位
            gridLayoutManager.scrollToPositionWithOffset(pos, 0);
        }
    		//定位的item的点击要在滚动之后
        mRecyclerView.post(new Runnable() {
            @Override
            public void run() {
                if (gridLayoutManager != null) {
                    View view = gridLayoutManager.findViewByPosition(pos);
                    if (view != null) {
                        clearPositionFlag();
                        view.performClick();
                    }
                }
            }
        });
  }
}

```

这样就可以完成一个ViewPager中的RecyclerView一个item的定位+模拟点击了。

### 当前页面获取

PagerAdapter中储存了当前的展示的页面PosPageView。
可以在PagerAdapter.setPrimaryItem中储存，以备后用，但要小心泄漏。

