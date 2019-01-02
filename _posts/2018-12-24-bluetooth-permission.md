---
layout: post
title: Android 蓝牙状态监听及开启等示例
date: 2018-12-24 20:10:00 +0800
categories: Android
tag: [bluetooth]
---
* content
{:toc}


# Android 蓝牙相关

## 蓝牙状态检测
```bash
    private boolean checkBleOpen(Context context) {
        BluetoothAdapter mBluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
        if (mBluetoothAdapter == null) {
            // Device does not support Bluetooth
            LogUtil.e(Constants.TAG_V_520 + "device is not support Bluetooth.");
            return false;
        }
        if (!mBluetoothAdapter.isEnabled()) {
            return false;
        } else {
            return true;
        }
    }
```

## 开启蓝牙
```bash
    Intent enableBtIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);
    context.startActivity(enableBtIntent);

    或

    startActivityForResult(new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE), REQUEST_ENABLE_BT);

```
以上两个调用方式在华为荣耀X2上会显示`某个应用想要开启蓝牙 `，以下方式可以弹出正确的引导框，并且在`6.0`之前可用于默认开启蓝牙：
```bash
    BluetoothAdapter.getDefaultAdapter().enable();
```
当然，如果用户拒绝了，会返回`false`，可以继续调用`方式1`。

## 监听蓝牙状态
* onCreate中
  ```bash
    broadcastReceiver = new BluetoothBroadcastReceiver(this);
    registerReceiver(broadcastReceiver, getIntentFilter());
  ```
* onDestroy中
  ```bash
    unregisterReceiver(broadcastReceiver);
  ```
* 函数参考
  ```bash
    private IntentFilter getIntentFilter() {
        IntentFilter intentFilter = new IntentFilter();
        intentFilter.addAction(BluetoothAdapter.ACTION_STATE_CHANGED);
        return intentFilter;
    }
  ```

  ```bash
    public static class BluetoothBroadcastReceiver extends BroadcastReceiver {
        private IStartWifiDisplay iStartWifiDisplay;

        public BluetoothBroadcastReceiver(IStartWifiDisplay iStartWifiDisplay) {
            this.iStartWifiDisplay = iStartWifiDisplay;
        }

        @Override
        public void onReceive(Context context, Intent intent) {
            String action = intent.getAction();
            if (action == null) {
                LogUtil.e(Constants.TAG_V_520 + "receiver action is null.");
                return;
            }
            switch (action) {
                case BluetoothAdapter.ACTION_STATE_CHANGED:
                    int blueState = intent.getIntExtra(BluetoothAdapter.EXTRA_STATE, 0);
                    if (blueState == BluetoothAdapter.STATE_ON) {
                        //TODO
                    }
                    break;
                default:
                    break;
            }
        }
    }
    ```

## 参考来源
>[https://developer.android.com/guide/topics/connectivity/bluetooth](https://developer.android.com/guide/topics/connectivity/bluetooth)
