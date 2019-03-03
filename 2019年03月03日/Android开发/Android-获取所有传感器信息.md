> 资料参考1：[android官方文档](https://developer.android.com/guide/topics/sensors/sensors_overview#java)
> 资料参考2：[挺不错的中文文档](https://www.kancloud.cn/kancloud/android-tutorial/87281)

1. 首先需要创建一个传感器的管理器
```
private SensorManager sensorManager;
sensorManager = (SensorManager) getSystemService(Context.SENSOR_SERVICE);
```

2. 获取手机支持的传感器列表，之后根据这个列表来创建传感器对象
```
private List<Sensor> sensors = new ArrayList<>();
sensors = sensorManager.getSensorList(Sensor.TYPE_ALL);
```

3. 遍历传感器列表，并且注册
```
for(Sensor sensor:sensors){
     sensorManager.registerListener(SensorListener, sensorManager.getDefaultSensor(sensor.getType()), SensorManager.SENSOR_DELAY_NORMAL);
}
```

4. 具体监听器代码
```
final SensorEventListener SensorListener = new SensorEventListener() {
        public void onSensorChanged(SensorEvent sensorEvent) {
            //当sensor的值发生变化的时候会触发这个回调方法
            editor.putString(sensorEvent.sensor.getName(),
                    String.valueOf(sensorEvent.values[0]) + "\r"
                    //+ "y=" + String.valueOf(sensorEvent.values[1]) + "\r"
                    //+ "z=" + String.valueOf(sensorEvent.values[2]) + "\r"
            );
            editor.apply();
        }
        public void onAccuracyChanged(Sensor sensor, int accuracy) {
            //当精度变化的时候来处理
        }
    };
```

这个时候就已经可以获取所有的传感器信息了。

5. 千万不要忘了最后注销！！
```
sensorManager.unregisterListener(SensorListener);
```
