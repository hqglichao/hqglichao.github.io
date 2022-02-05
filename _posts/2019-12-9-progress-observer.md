---
layout: post
title: 下载UI和下载逻辑解耦--观察者方式
date: 2019-12-9 21:12:00 +0800
categories: 技术文章
tag: [观察者模式]
---

* content
{:toc}

# 下载UI和下载逻辑解耦
移动端编程中经常会用到ViewPager，容纳很多数据每个页面可能又是一个RecyclerView。如果RecyclerView的每个item想要有一个下载进度条，并且这个进度条要有记忆逻辑，就是item回收或者复用之后回来，状态能根据下载的进度实时更新显示，这个问题就有点烦了。  
这篇文章讲的解决方案是把每个item当成一个observer，然后自由的监听。  
ViewPager+RecyclerView的数据流组装形式比较常见。如下所示:  
<img src="https://picgo-1307686581.cos.ap-shanghai.myqcloud.com/github/hqglichao/gif/progress_observer.gif"/>  
上面动图中：  

1. 先是点击多个item下载，出现了进度条
2. 切到第一个tab，当前tab其实view被回收了  
3. 再切回来的时候，重新创建，进度条根据下载列表，重新观察获取数据。   

ViewPager的特点是内存里面最多只加载三个page，当前页面+左边页面+右边页面，其他页面没有在内存里面，滑动过程中就销毁了，销毁后重新建立的时候为了能重新获得当前page（RecyclerView）里面的某个item的下载进度，需要在这个item添加到视图上的时候，重新监听下载列表。  

### 数据结构定义  
```bash
    private final Map<String, IDownloadListener> downloadingMap = new HashMap<>();
    private final Map<String, List<IProgressObserver>> observerMap = new HashMap<>();

    public interface IDownloadListener {
        void onDownloadFinish(Info info, boolean isSuccess);
        void onProgressUpdate(Info info, int progress);
    }

    public interface IProgressObserver {
        // 返回类型可以自定义
        void onDownloadFinish(boolean isSuccess);
        void onProgressUpdate(int progress);
    }
``` 
主要有两个Map
1. downloadingMap `key`是item的id，可以是后台配的唯一的标示，`value`是下载的Listener，这个listener会常驻，直到下载完成移除
2. observerMap `key`是item的id，同上，`value`是这个id对应的监听者，因为有可能不同的page会有相同的id存在，所以这里一个下载任务要对应多个监听

### 启动下载
启动下载，每个item会对应一个listener，这个只有在下载好的时候才会移除，每个listener收到数据更新时，会去遍历现有的当前listener的`List<IProgressObserver> observerList`，如果不存在observer也不影响
```bash
    public void startDownload(String itemId, final String downloadUrl) {
        if (isListenerExits(itemId)) {
            //已经在下载列表，直接返回
            return;
        }
        DownloadProcessListener listener = new DownloadProcessListener();
        downloadingMap.put(itemid, listener);
        ThreadManager.excute(new Runnable() {
            @Override
            public void run() {
                RealDownloadManager.getInstance().download(downloadUrl, listener);
            }
        }, ThreadType.NETWORK, null, true);
    }
    public synchronized void notifyDownloadProgress(String id, int progress) {
        downloadingMap.remove(widgetId); //下载完成，移除相关listener
        List<IProgressObserver> observerList = observerMap.get(id);
        if (observerList == null) {
            observerMap.remove(id);
            return;
        }
        for (IProgressObserver observer : observerList) {
            observer.onProgressUpdate(id, progress);
        }
    }

    public synchronized void notifyDownloadFinish(final String itemId, boolean isSuccess) {
        List<IProgressObserver> observerList = observerMap.get(itemId);
        if (observerList == null) {
            observerMap.remove(itemId);
            return;
        }
        
        for (IProgressObserver observer : observerList) {
            observer.onDownloadFinish(itemId, isSuccess);
        }
        observerMap.remove(itemId);
    }

    private static class DownloadProcessListener implements AEMaterialDownloader.MaterialDownloadListener {
        @Override
        public void onDownloadFinish(Info info, boolean isSuccess) {
            notifyDownloadFinish(info.id, isSuccess);
        }

        @Override
        public void onProgressUpdate(Info info, int progress) {
            notifyDownloadProgress(info.id, progress);
        }
    }
```

### Item获取下载状态
下载状态通过`downloadingMap`里面的listener实时更新，并且会通过`notifyDownloadProgress`和`notifyDownloadFinish`回传给observer，RecyclerView的每个item可以继承IProgressObserver，
在Adapter的onViewAttachedToWindow中通过`addProgressObserver`添加订阅，注意在`onViewDetachedFromWindow`中通过removeProgressObserver移除。
此外页面销毁与`ViewPager`的`destroyItem`中也要注意移除对应的observer
```bash
    public synchronized void addProgressObserver(String id, IProgressObserver observer) {
        if (observer != null) {
            List<IProgressObserver> list = observerMap.get(id);
            if (list == null) {
                list = new ArrayList<IProgressObserver>();
            }
            list.add(observer);
            observerMap.put(id, list);
        }
    }

    public synchronized void removeProgressObserver(String id, IProgressObserver observer) {
        List<IProgressObserver> list = observerMap.get(id);
        if (list != null && list.size() > 1) {
            for (Iterator<IProgressObserver> iterator = list.iterator(); iterator.hasNext();) {
                IProgressObserver iProgressObserver = iterator.next();
                if (iProgressObserver == observer) {
                    iterator.remove();
                }
            }
        }
        if (list == null || list.size() < 1) {
            observerMap.remove(id);
        }
    }
```