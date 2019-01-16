  最近一个项目需要获取android设备电池信息，稍微记录下。
  所用的方法是使用广播被动接受的。详细代码见下：
1. 首先声明四个变量
```
private SharedPreferences preferences;
private SharedPreferences.Editor editor;
private IntentFilter intentFilter;
private BatteryChangeReceiver batteryChangeReceiver;
```
2. 初始化它们，并且进行注册
```
preferences = getSharedPreferences("data", MODE_PRIVATE);
editor = getSharedPreferences("data", MODE_PRIVATE).edit();
intentFilter = new IntentFilter();
intentFilter.addAction(Intent.ACTION_BATTERY_CHANGED);
batteryChangeReceiver = new BatteryChangeReceiver(editor);
registerReceiver(batteryChangeReceiver, intentFilter);
```
3. BatteryChangeReceiver我单独注册了一个类用来进行操作
```
public class BatteryChangeReceiver extends BroadcastReceiver {

    SharedPreferences.Editor editor;

    public BatteryChangeReceiver(SharedPreferences.Editor editor) {
        this.editor = editor;
    }

    @Override
    public void onReceive(Context context, Intent intent) {
        //获取电池状态
        int batteryStatus = intent.getIntExtra(BatteryManager.EXTRA_STATUS, -1);
        switch (batteryStatus) {
            case BatteryManager.BATTERY_STATUS_UNKNOWN:
                editor.putString("batteryStatus", "BATTERY_STATUS_UNKNOWN");
                break;
            case BatteryManager.BATTERY_STATUS_CHARGING:
                editor.putString("batteryStatus", "BATTERY_STATUS_CHARGING");
                break;
            case BatteryManager.BATTERY_STATUS_DISCHARGING:
                editor.putString("batteryStatus", "BATTERY_STATUS_DISCHARGING");
                break;
            case BatteryManager.BATTERY_STATUS_NOT_CHARGING:
                editor.putString("batteryStatus", "BATTERY_STATUS_NOT_CHARGING");
                break;
            case BatteryManager.BATTERY_STATUS_FULL:
                editor.putString("batteryStatus", "BATTERY_STATUS_FULL");
                break;

            default:
                break;
        }

        //获取电池的健康程度
        int batteryHealth = intent.getIntExtra(BatteryManager.EXTRA_HEALTH, -1);
        switch (batteryHealth) {
            case BatteryManager.BATTERY_HEALTH_UNKNOWN:
                editor.putString("batteryHealth", "BATTERY_HEALTH_UNKNOWN");
                break;
            case BatteryManager.BATTERY_HEALTH_GOOD:
                editor.putString("batteryHealth", "BatteryManager.BATTERY_HEALTH_GOOD");
                break;
            case BatteryManager.BATTERY_HEALTH_OVERHEAT:
                editor.putString("batteryHealth", "BatteryManager.BATTERY_HEALTH_OVERHEAT");
                break;
            case BatteryManager.BATTERY_HEALTH_DEAD:
                editor.putString("batteryHealth", "BatteryManager.BATTERY_HEALTH_DEAD");
                break;
            case BatteryManager.BATTERY_HEALTH_OVER_VOLTAGE:
                editor.putString("batteryHealth", "BatteryManager.BATTERY_HEALTH_OVER_VOLTAGE");
                break;
            case BatteryManager.BATTERY_HEALTH_UNSPECIFIED_FAILURE:
                editor.putString("batteryHealth", "BatteryManager.BATTERY_HEALTH_UNSPECIFIED_FAILURE");
                break;
            case BatteryManager.BATTERY_HEALTH_COLD:
                editor.putString("batteryHealth", "BatteryManager.BATTERY_HEALTH_COLD");
                break;
            default:
                break;
        }

        //获取电池是否存在
        boolean batteryPresent = intent.getBooleanExtra(BatteryManager.EXTRA_PRESENT, false);
        editor.putString("batteryPresent", String.valueOf(batteryPresent));

        //获取电池的当前电量和总电量
        int batteryCurrent = intent.getExtras().getInt("level");// 获得当前电量
        int batteryTotal = intent.getExtras().getInt("scale");// 获得总电量
        editor.putString("batteryCurrent", String.valueOf(batteryCurrent));
        editor.putString("batteryTotal", String.valueOf(batteryTotal));

        //获取电池充电方式
        int batteryPlugged = intent.getIntExtra(BatteryManager.EXTRA_PLUGGED, -1);
        switch (batteryPlugged) {
            case BatteryManager.BATTERY_PLUGGED_AC:
                editor.putString("batteryPlugged", "BATTERY_PLUGGED_AC");//交流电
                break;
            case BatteryManager.BATTERY_PLUGGED_USB:
                editor.putString("batteryPlugged", "BatteryManager.BATTERY_PLUGGED_USB");//USB充电
                break;
            case BatteryManager.BATTERY_PLUGGED_WIRELESS:
                editor.putString("batteryPlugged", "BatteryManager.BATTERY_PLUGGED_WIRELESS");//无线充电
                break;
            default:
                break;
        }

        //获取电池的电压
        int batteryVoltage = intent.getIntExtra(BatteryManager.EXTRA_VOLTAGE, -1);
        editor.putString("batteryVoltage", String.valueOf(batteryVoltage));

        //获取电池的温度
        int batteryTemperature = intent.getIntExtra(BatteryManager.EXTRA_TEMPERATURE, -1);
        editor.putString("batteryTemperature", String.valueOf(batteryTemperature));

        //获取电池的技术
        String batteryTechnology = intent.getStringExtra(BatteryManager.EXTRA_TECHNOLOGY);
        editor.putString("batteryTechnology", batteryTechnology);

        //提交
        editor.apply();
    }
}
```
