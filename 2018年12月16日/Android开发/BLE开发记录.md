近期写了一个关于android ble（低功耗蓝牙）的APP，这里写篇文章记录下

#参考资料
1. [google官方的demo](https://github.com/googlesamples/android-BluetoothLeGatt)
2. [Android BLE蓝牙通信库](https://github.com/dingjikerbo/BluetoothKit)
3. [青草_离离的博客](https://blog.csdn.net/qq_34947883/article/details/78815237)


#基础知识
* BLE是Bluetooth low energy的意思，属于蓝牙低功耗协议，Android4.3以上及苹果手机等现在都支持蓝牙BLE，而且不得不承认苹果手机支持得更好。
* 在我们的这个APP中，我们的手机作为中心设备，而手环等蓝牙设备作为周边设备。
* BLE技术是基于GATT进行通信的，GATT是一种属性传输协议，简单的讲可以认为是一种属性传输的应用层协议。所以理解它**非常重要**，GATT下面有很多服务(service)，每个服务都有自己的UUID；同时每个服务都有一个或者多个特征(characteristic)，同样每个特征都具有对应的UUID。每个特征都有对应的value(一般写APP要获取的就是特征下面的value值)和几个descriptor(descriptor是用来对这个value进行描述)
* 我以我的小米手环2为例，详细说明下这段。可以这么理解，小米手环2本身是一个GATT(注意，**仅仅是为了便于表达**，这么说其实是错的)，我们假设它包含三个service，刚好对应它的三个功能：分别是提供设备信息的service、提供步数的service、检测心率的service。设备信息的service中，会包含一个或者多个characteristic，这些characteristic中的value值就相对应于手环的厂商信息、硬件信息等；心率Service则包括心率characteristic等，而心率characteristic中的value就是真正的心率的数据，而descriptor则是对该value的描述说明，比如value的单位啊，描述啊，权限啊等。
* 了解了上面的两段问题就不大了。

#大致步骤
申请权限：手机需要版本高于4.3且支持蓝牙，并且开启蓝牙。
搜索蓝牙：搜索蓝牙，在回调方法中查看相关设备信息，并在一定时间停止扫描。
连接蓝牙：首先获取到ble设备的mac地址，然后调用connect()方法进行连接。
获取特征：蓝牙连接成功后，需要获取蓝牙的服务(service)和特征(characteristic)等，然后开启接收设置。
发送消息：调用writeCharacteristic()方法，发送数据给ble设备。
接收消息：通过蓝牙的回调接口中onCharacteristicRead()方法，接收蓝牙收的消息。
释放资源：断开连接，关闭资源。

#APP要求
我做的这个APP是一开始扫描周边的蓝牙设备，然后将这些设备汇总成一个列表，当点击的时候就进入第二个页面，该页面可以有四个功能：
1. 连接该设备 
2. 发数据给设备
3. 获取通知(notification)
4. 断开设备连接

#详细步骤
###1.声明权限
在manifest文件中声明如下的权限
```
<uses-permission android:name="android.permission.BLUETOOTH" />
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
``` 
前两个是蓝牙相关的，后面两个需要**动态申请** 

###2.动态申请权限
我这里用了[第三方库](https://github.com/dfqin/PermissionGrantor)，直接在`build.gradle`中加一句`implementation 'com.github.dfqin:grantor:2.1.0'`即可。然后在MainActivity中一开始动态申请了权限。具体代码见下：
```
    /**
     * 申请ACCESS_COARSE_LOCATION权限，如果用户拒绝就直接关闭程序
     * 第三方库 compile 'com.github.dfqin:grantor:2.1.0'
     */
    private void askForLocationPermission() {
        if (PermissionsUtil.hasPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION)) {
            //用户已经授予权限，什么都不用做
        } else {
            //向用户申请权限
            PermissionsUtil.requestPermission(this, new PermissionListener() {
                @Override
                public void permissionGranted(@NonNull String[] permissions) {
                    //用户授予了权限，什么都不用做
                }

                @Override
                public void permissionDenied(@NonNull String[] permissions) {
                    //用户拒绝了权限申请，关闭
                    finish();
                }
            }, new String[]{Manifest.permission.ACCESS_COARSE_LOCATION});
        }
    }
```
###3.初始化控件
```
        bluetoothManager = (BluetoothManager)getSystemService(Context.BLUETOOTH_SERVICE);
        mBluetoothAdapter = bluetoothManager.getAdapter();
        mClient = new BluetoothClient(this);
        listView = findViewById(R.id.list_view);
        adapter = new ArrayAdapter<>(MainActivity.this, android.R.layout.simple_list_item_1);
        deviceMacAddressList = new ArrayList<>();
```
就是简单获取控件，我这里用`deviceMacAddressList`这个数据结构来存储所有的设备的MAC地址。
###4.扫描设备
这里我还是用了[第三方库](https://github.com/dingjikerbo/BluetoothKit)来直接扫描，直接在`build.gradle`中加一句`implementation 'com.inuker.bluetooth:library:1.4.0'`即可。详细代码见下：
```
    /**
     * 开始扫描设备
     */
    private void searchDevice() {

        SearchRequest request = new SearchRequest.Builder()
                .searchBluetoothLeDevice(1000 * SEARCH_TIME, 1)   
                //.searchBluetoothClassicDevice(5000) // 再扫经典蓝牙5s
                //.searchBluetoothLeDevice(2000)      // 再扫BLE设备2s
                .build();

        mClient.search(request, new SearchResponse() {
            @Override
            public void onSearchStarted() {
                Log.d(TAG, "SearchStart");
            }

            @Override
            public void onDeviceFounded(SearchResult device) {
                if (!deviceMacAddressList.contains(device.getAddress())) {
                    String device_name = device.getName();
                    if ("NULL" == device_name) {
                        device_name = "未知设备";
                    }
                    adapter.add(device_name + "\n" + device.getAddress());
                    adapter.notifyDataSetChanged();
                    deviceMacAddressList.add(device.getAddress());
                }
            }

            @Override
            public void onSearchStopped() {
                Log.d(TAG, "SearchStop");
            }

            @Override
            public void onSearchCanceled() {
                Log.d(TAG, "SearchCancel");
            }
        });
    }
```
直接用第三方库，很方便，就是把扫描到的设备的MAC地址放入`deviceMacAddressList`中，然后更新ListView的Adapter即可。还有对应的listView的对应的点击事件我这里就不贴代码了，逻辑就是点击对应的iterm，会跳转到下个活动，且只传递一个mac地址即可。

###5. 连接目标Mac地址的设备
```
//与指定的设备进行连接
    public boolean connect() {
        if (mBluetoothAdapter == null || mBluetoothDeviceAddress == null) {
            Log.d(TAG, "mBluetoothAdapter为空或者MAC地址为空");
            return false;
        }
        device = mBluetoothAdapter.getRemoteDevice(mBluetoothDeviceAddress);
        if (device == null) {
            Log.w(TAG, "设备没有找到，无法连接");
            return false;
        }

        myTextViewAppend("正在连接......");

        mBluetoothGatt = device.connectGatt(this, false, mGattCallback);
        if (mBluetoothGatt == null) {
            myTextViewAppend("连接发生错误");
            return false;
        }

        return true;
    }
```
这里因为我之前说了新开了一个活动，所以需要重新绑定控件和进行一些初始化操作，这里先略去了。首先进来判断`BluetoothAdapter`和Mac地址是否为空，如果不是则对device利用Mac地址进行绑定。之后则是最重要的一步，`mBluetoothGatt = device.connectGatt(this, false, mGattCallback);`，接下来的几乎所有操作都要依赖这个`mBluetoothGatt`，是通过设备的`connectGatt`获取到的。，第二个参数似乎是自动重连，我这边没有这个需求，第三个参数是回调函数，之后的所有操作都会用到它。
###6. 设备的读写和获取通知
在之前的第五步中我们已经成功连接了设备，那么我们现在可以马上进行读写吗？还不行，目前我们首先还需要进行回调函数的重写。
```
private BluetoothGattCallback mGattCallback = new BluetoothGattCallback() {

        @Override
        public void onConnectionStateChange(BluetoothGatt gatt, int status, int newState) {
            super.onConnectionStateChange(gatt, status, newState);
            if (newState == BluetoothProfile.STATE_CONNECTED) {
                //连接成功
                Log.d(TAG, "连接成功");
                myTextViewAppend("连接成功");
                hasConnected = true;
                myTextViewAppend("开始发现服务......");
                mBluetoothGatt.discoverServices();
                //开线程来确保如果 RECONNECT_TIME秒后没有发现服务，则继续执行
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        try {
                            Thread.sleep(1000 * RECONNECT_TIME);
                            if (!hasServiceFound && hasConnected) {
                                reConnect();
                            }
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                }).start();
            } else if (newState == BluetoothProfile.STATE_DISCONNECTED) {
                //连接断开
                myTextViewAppend("连接断开，status=" + status);
                Log.d(TAG, "连接断开 status=" + status);
                closeAll();
            }
        }

        @Override
        public void onServicesDiscovered(BluetoothGatt gatt, int status) {
            super.onServicesDiscovered(gatt, status);
            if (status == BluetoothGatt.GATT_SUCCESS) {
                //发现设备，遍历服务，初始化特征
                myTextViewAppend("发现服务！开始初始化特征！");
                hasServiceFound = true;
                initBLE(gatt);
            } else {
                myTextViewAppend("无法发现服务！status=" + status);
                hasServiceFound = false;
                Log.d(TAG, "onServicesDiscovered fail-->" + status);
            }
        }

        @Override
        public void onCharacteristicRead(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic, int status) {
            super.onCharacteristicRead(gatt, characteristic, status);
            if (status == BluetoothGatt.GATT_SUCCESS) {
                // 收到的数据
                Log.d(TAG, "onCharacteristicRead！");
                myGetValueFromCharacteristic(characteristic);
                new Thread(new Runnable() {
                    @Override
                    public void run() {
                        try {
                            Thread.sleep(100 * RECONNECT_TIME);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    }
                }).start();
            } else {
                myTextViewAppend("数据读取失败,status=" + status);
                Log.d(TAG, "onCharacteristicRead fail-->" + status);
            }
        }

        @Override
        public void onCharacteristicChanged(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic) {
            super.onCharacteristicChanged(gatt, characteristic);
            //当特征中value值发生改变
            gatt.readCharacteristic(characteristic);
            Log.d(TAG, "onCharacteristicChanged被执行");
            myGetValueFromCharacteristic(characteristic);
        }


        @Override
        public void onCharacteristicWrite(BluetoothGatt gatt, BluetoothGattCharacteristic characteristic, int status) {
            super.onCharacteristicWrite(gatt, characteristic, status);
            Log.d(TAG, "onCharacteristicWrite被调用");
            if (status == BluetoothGatt.GATT_SUCCESS) {
                // 发送成功
                Log.d(TAG, "发送成功");
                myTextViewAppend("发送成功");
            } else {
                // 发送失败
                Log.d(TAG, "发送失败");
                myTextViewAppend("发送失败");
            }
        }

        @Override
        public void onMtuChanged(BluetoothGatt gatt, int mtu, int status) {
            super.onMtuChanged(gatt, mtu, status);
            if (status == BluetoothGatt.GATT_SUCCESS) {
                //成功协商mtu
                myTextViewAppend("协商后的MTU=" + mtu);
            } else {
                myTextViewAppend("MTU=" + mtu);
            }

        }

        @Override
        public void onDescriptorWrite(BluetoothGatt gatt,
                                      BluetoothGattDescriptor descriptor, int status) {
            super.onDescriptorWrite(gatt, descriptor, status);
            if (status == BluetoothGatt.GATT_SUCCESS) {

            }
        }

        @Override
        public void onReadRemoteRssi(BluetoothGatt gatt, int rssi, int status) {
            super.onReadRemoteRssi(gatt, rssi, status);
            if (status == BluetoothGatt.GATT_SUCCESS) {

            }
        }

        @Override
        public void onDescriptorRead(BluetoothGatt gatt, BluetoothGattDescriptor descriptor, int status) {
            super.onDescriptorRead(gatt, descriptor, status);
            Log.d(TAG, "onDescriptorRead被调用");
            if (status == BluetoothGatt.GATT_SUCCESS) {
                Log.d(TAG, "onDescriptorRead 成功读取");
                BluetoothGattDescriptor clientConfig = mBluetoothGattDescriptor;
                clientConfig.setValue(BluetoothGattDescriptor.ENABLE_INDICATION_VALUE);
                //clientConfig.setValue(BluetoothGattDescriptor.ENABLE_NOTIFICATION_VALUE);
                mBluetoothGatt.writeDescriptor(clientConfig);
            }
        }
    };
```
下面开始一点一点解析这个回调函数。
第一个是`onConnectionStateChange`，这个会在设备连接和设备断开连接的时候被调用，如果连接成功，那么显而易见我们应该去找到所有的服务(service)，所以写了`mBluetoothGatt.discoverServices();`，如果连接被断开，记得在其中断开连接，释放资源。
第二个是`onServicesDiscovered`，这个是因为你前面在连接成功的时候去发现服务，如果成功找到服务那么这个函数就会被回调。在里面加入你初始化characteristic的逻辑。
第三个是`onCharacteristicRead`，这个函数会在你调用`mBluetoothGatt.readcharacteristic(你要读取的对应的characteristic)`被调用。
第四个是`onCharacteristicChanged`，每当一个characteristic的value发生变化的时候，都会调用这个函数，所以如果你发送了一些指令给BLE设备，如果BLE设备进行了响应，那么对应的逻辑应该在这里处理。
第五个是`onCharacteristicWrite`，这个是你成功向BLE设备发送信息的时候会被调用。

OK有了上面的了解之后，还是继续从我们连接设备开始之后。
#####Step 1 连接成功
如果连接成功，那么`onConnectionStateChange`显然会被调用，你可以在其中用`mBluetoothGatt.discoverServices()`去发现服务。当然如果连接失败，千万不要忘记释放资源。

#####Step 2 发现服务
既然已经发现服务了，那么`onServicesDiscovered`会被回调，在里面可以初始化你的各种characteristic了。我的代码见下：
```
public void initBLE(BluetoothGatt gatt) {
        if (gatt == null) {
            return;
        }
        //遍历所有服务
        for (BluetoothGattService BluetoothGattService : gatt.getServices()) {
            for (BluetoothGattCharacteristic bluetoothGattCharacteristic : BluetoothGattService.getCharacteristics()) {
                String str = bluetoothGattCharacteristic.getUuid().toString();
                if (str.equals(writeUUID)) {
                    //根据写UUID找到写特征
                    writeCharacteristic = bluetoothGattCharacteristic;
                } else if (str.equals(notifyUUID)) {
                    //根据通知UUID找到通知特征
                    notifyCharacteristic = bluetoothGattCharacteristic;
                    setCharacteristicNotification(notifyCharacteristic, true);
                    myTextViewAppend("找到通知特征");
                } else if (str.equals(readUUID)) {
                    readCharacteristic = bluetoothGattCharacteristic;
                } else if (str.equals(writeNoResponseUUID)) {
                    writeNoResponseCharacteristic = bluetoothGattCharacteristic;
                    myTextViewAppend("清空按钮已启用");
                }
            }
        }
    }
```
代码思路很简单，就是根据判断uuid是否相等(注意，开发所需的UUID都应该会由设备的开发者提供，所以你应该和他们沟通，即“我应该读取UUID为多少的characteristic”)来给对应的Characteristic赋值。这里注意一点，当时我做的时候也有疑惑，既然我已经知道了我所要读/写/获取通知的Characteristic的UUID的值，那我可不可以不进行这一步？可以。但是推荐这么做，因为做这一步的目的是为了确保你的Characteristic确实是存在的，而不是空，否则你如果去直接读写，万一Characteristic不存在，那程序就直接闪退了，很不好。而且有些Characteristic可能既可以读又可以写还可以获取通知，通过遍历你也可以获取所有的Characteristic的详情。(我的代码中只是简单遍历，并没有获取所有Characteristic的属性)
#####Step 3 开启通知(可选，如果你只是写可以不开，但是还是强烈建议开)
```
public void setCharacteristicNotification(BluetoothGattCharacteristic characteristic,
                                              boolean enabled) {
        if (mBluetoothAdapter == null || mBluetoothGatt == null) {
            Log.w(TAG, "BluetoothAdapter not initialized");
            return;
        }
        mBluetoothGatt.setCharacteristicNotification(characteristic, enabled);
        if (UUID_HEART_RATE_MEASUREMENT.equals(characteristic.getUuid())) {
            BluetoothGattDescriptor descriptor = characteristic.getDescriptor(
                    UUID.fromString("00002902-0000-1000-8000-00805f9b34fb"));
            //descriptor.setValue(BluetoothGattDescriptor.ENABLE_INDICATION_VALUE);
            descriptor.setValue(BluetoothGattDescriptor.ENABLE_NOTIFICATION_VALUE);
            mBluetoothGatt.writeDescriptor(descriptor);
        }
    }
```
上面的`UUID_HEART_RATE_MEASUREMENT`等于这东西：`public final static UUID UUID_HEART_RATE_MEASUREMENT =
            UUID.fromString("00002a37-0000-1000-8000-00805f9b34fb");`
这段代码是对你传入的Characteristic进行开启通知的操作，只有进行了这一步，你之后才可以获取到通知，尤其是第二个if非常重要，它是设置descriptor，相当于把value设置成了可以发送通知。

#####Step 4 写操作
```
byte[] data = {0x02};
writeNoResponseCharacteristic.setValue(data);
mBluetoothGatt.writeCharacteristic(writeNoResponseCharacteristic);
```
很简单，就是首先提供数据，然后调用你的写Characteristic的`setValue`方法，然后再用`BluetoothGatt`的`writeCharacteristic`方法，其中你的Characteristic作为参数传入即可。之后`onCharacteristicWrite`这个函数会被回调，根据状态可以判断是否发送成功，并执行后续的操作。**这里注意，一次最多只能发送20个字节，如果多了需要多次发送。**

#####Step 5 读操作
```
mBluetoothGatt.readCharacteristic(readCharacteristic);
```
如果在`readCharacteristic`中有数据，这时候`onCharacteristicRead`就会被回调，可以在里面执行对应的逻辑。

#####Step 6 获取通知操作
这其实是我目前还没有解决的问题，因为设备写了很长一串的notification，我只能接受20字节的。
获取通知对于APP来说是被动的，也就是不需要任何操作，只要BLE设备发送了通知给手机，`onCharacteristicChanged`就会被回调，在里面进行操作即可。
之后的解决方法是通过在`onConnectionStateChange`中加入协商MTU的部分，记得协商MTU是需要时间的，所以需要开启线程来sleep一段时间，我的设备是1秒钟就可以。

### 7 断开连接
```
    //关闭所有资源
    private void closeAll() {
        if (mBluetoothGatt != null) {
            mBluetoothGatt.disconnect();
            mBluetoothGatt.close();
            mBluetoothGatt = null;
        }
        hasConnected = false;
        hasServiceFound = false;
        writeCharacteristic = null;
        readCharacteristic = null;
        notifyCharacteristic = null;
        writeNoResponseCharacteristic = null;
        device = null;
        mBluetoothManager = null;
        mBluetoothAdapter = null;


    }
```

### 8.踩过的坑
1. 没有动态权限申请导致无法扫描到设备
2. 没有异步操作textView导致程序闪退
3. 没有协商MTU导致数据超过20字节之后就收不到




