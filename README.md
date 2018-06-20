# LCCL
view plain copy
public class wifi extends Activity  
{  
  private TextView mTextView01;  
  private CheckBox mCheckBox01;  
    
  /* 创建WiFiManager对象 */  
  private WifiManager mWiFiManager01;  
    
  /** Called when the activity is first created. */  
  @Override  
  public void onCreate(Bundle savedInstanceState)  
  {  
    super.onCreate(savedInstanceState);  
    setContentView(R.layout.main);  
      
    mTextView01 = (TextView) findViewById(R.id.myTextView1);  
    mCheckBox01 = (CheckBox) findViewById(R.id.myCheckBox1);  
      
    /* 以getSystemService取得WIFI_SERVICE */  
    mWiFiManager01 = (WifiManager)  
                     this.getSystemService(Context.WIFI_SERVICE);  
      
    /* 判断运行程序后的WiFi状态是否打开或打开中 */  
    if(mWiFiManager01.isWifiEnabled())  
    {  
      /* 判断WiFi状态是否“已打开” */  
      if(mWiFiManager01.getWifiState()==  
         WifiManager.WIFI_STATE_ENABLED)  
      {  
        /* 若WiFi已打开，将选取项打勾 */  
        mCheckBox01.setChecked(true);  
        /* 更改选取项文字为关闭WiFi*/  
        mCheckBox01.setText(R.string.str_uncheck);  
      }  
      else  
      {  
        /* 若WiFi未打开，将选取项勾选择消 */  
        mCheckBox01.setChecked(false);  
        /* 更改选取项文字为打开WiFi*/  
        mCheckBox01.setText(R.string.str_checked);  
      }  
    }  
    else  
    {  
      mCheckBox01.setChecked(false);  
      mCheckBox01.setText(R.string.str_checked);  
    }  
      
    /* 捕捉CheckBox的点击事件 */  
    mCheckBox01.setOnClickListener(  
    new CheckBox.OnClickListener()  
    {  
      @Override  
      public void onClick(View v)  
      {  
        // TODO Auto-generated method stub  
          
        /* 当选取项为取消选取状态 */  
        if(mCheckBox01.isChecked()==false)  
        {  
          /* 尝试关闭Wi-Fi服务 */  
          try  
          {  
            /* 判断WiFi状态是否为已打开 */  
            if(mWiFiManager01.isWifiEnabled() )  
            {  
              /* 关闭WiFi */  
              if(mWiFiManager01.setWifiEnabled(false))  
              {  
                mTextView01.setText(R.string.str_stop_wifi_done);  
              }  
              else  
              {  
                mTextView01.setText(R.string.str_stop_wifi_failed);  
              }  
            }  
            else  
            {  
              /* WiFi状态不为已打开状态时 */  
              switch(mWiFiManager01.getWifiState())  
              {  
                /* WiFi正在打开过程中，导致无法关闭... */  
                case WifiManager.WIFI_STATE_ENABLING:  
                  mTextView01.setText  
                  (  
                    getResources().getText  
                    (R.string.str_stop_wifi_failed)+":"+  
                    getResources().getText  
                    (R.string.str_wifi_enabling)  
                  );  
                  break;  
                /* WiFi正在关闭过程中，导致无法关闭... */  
                case WifiManager.WIFI_STATE_DISABLING:  
                  mTextView01.setText  
                  (  
                    getResources().getText  
                    (R.string.str_stop_wifi_failed)+":"+  
                    getResources().getText  
                    (R.string.str_wifi_disabling)  
                  );  
                  break;  
                /* WiFi已经关闭 */  
                case WifiManager.WIFI_STATE_DISABLED:  
                  mTextView01.setText  
                  (  
                    getResources().getText  
                    (R.string.str_stop_wifi_failed)+":"+  
                    getResources().getText  
                    (R.string.str_wifi_disabled)  
                  );  
                  break;  
                /* 无法取得或辨识WiFi状态 */  
                case WifiManager.WIFI_STATE_UNKNOWN:  
                default:  
                  mTextView01.setText  
                  (  
                    getResources().getText  
                    (R.string.str_stop_wifi_failed)+":"+  
                    getResources().getText  
                    (R.string.str_wifi_unknow)  
                  );  
                  break;  
              }  
              mCheckBox01.setText(R.string.str_checked);  
            }  
          }  
          catch (Exception e)  
          {  
            Log.i("HIPPO", e.toString());  
            e.printStackTrace();  
          }  
        }  
        else if(mCheckBox01.isChecked()==true)  
        {  
          /* 尝试打开Wi-Fi服务 */  
          try  
          {  
            /* 确认WiFi服务是关闭且不在打开作业中 */  
            if(!mWiFiManager01.isWifiEnabled() &&   
               mWiFiManager01.getWifiState()!=  
               WifiManager.WIFI_STATE_ENABLING )  
            {  
              if(mWiFiManager01.setWifiEnabled(true))  
              {  
                switch(mWiFiManager01.getWifiState())  
                {  
                  /* WiFi正在打开过程中，导致无法打开... */  
                  case WifiManager.WIFI_STATE_ENABLING:  
                    mTextView01.setText  
                    (  
                      getResources().getText  
                      (R.string.str_wifi_enabling)  
                    );  
                    break;  
                  /* WiFi已经为打开，无法再次打开... */  
                  case WifiManager.WIFI_STATE_ENABLED:  
                    mTextView01.setText  
                    (  
                      getResources().getText  
                      (R.string.str_start_wifi_done)  
                    );  
                    break;  
                  /* 其它未知的错误 */  
                  default:  
                    mTextView01.setText  
                    (  
                      getResources().getText  
                      (R.string.str_start_wifi_failed)+":"+  
                      getResources().getText  
                      (R.string.str_wifi_unknow)  
                    );  
                    break;  
                }  
              }  
              else  
              {  
                mTextView01.setText(R.string.str_start_wifi_failed);  
              }  
            }  
            else  
            {  
              switch(mWiFiManager01.getWifiState())  
              {  
                /* WiFi正在打开过程中，导致无法打开... */  
                case WifiManager.WIFI_STATE_ENABLING:  
                  mTextView01.setText  
                  (  
                    getResources().getText  
                    (R.string.str_start_wifi_failed)+":"+  
                    getResources().getText  
                    (R.string.str_wifi_enabling)  
                  );  
                  break;  
                /* WiFi正在关闭过程中，导致无法打开... */  
                case WifiManager.WIFI_STATE_DISABLING:  
                  mTextView01.setText  
                  (  
                    getResources().getText  
                    (R.string.str_start_wifi_failed)+":"+  
                    getResources().getText  
                    (R.string.str_wifi_disabling)  
                  );  
                  break;  
                /* WiFi已经关闭 */  
                case WifiManager.WIFI_STATE_DISABLED:  
                  mTextView01.setText  
                  (  
                    getResources().getText  
                    (R.string.str_start_wifi_failed)+":"+  
                    getResources().getText  
                    (R.string.str_wifi_disabled)  
                  );  
                  break;  
                /* 无法取得或识别WiFi状态 */  
                case WifiManager.WIFI_STATE_UNKNOWN:  
                default:  
                  mTextView01.setText  
                  (  
                    getResources().getText  
                    (R.string.str_start_wifi_failed)+":"+  
                    getResources().getText  
                    (R.string.str_wifi_unknow)  
                  );  
                  break;  
              }  
            }  
            mCheckBox01.setText(R.string.str_uncheck);  
          }  
          catch (Exception e)  
          {  
            Log.i("HIPPO", e.toString());  
            e.printStackTrace();  
          }  
        }  
      }  
    });  
  }  
    
  public void mMakeTextToast(String str, boolean isLong)  
  {  
    if(isLong==true)  
    {  
      Toast.makeText(wifi.this, str, Toast.LENGTH_LONG).show();  
    }  
    else  
    {  
      Toast.makeText(wifi.this, str, Toast.LENGTH_SHORT).show();  
    }  
  }  
    
  @Override  
  protected void onResume()  
  {  
    // TODO Auto-generated method stub  
      
    /* 在onResume重写事件为取得打开程序当下WiFi的状态 */  
    try  
    {  
      switch(mWiFiManager01.getWifiState())  
      {  
        /* WiFi已经在打开状态... */  
        case WifiManager.WIFI_STATE_ENABLED:  
          mTextView01.setText  
          (  
            getResources().getText(R.string.str_wifi_enabling)  
          );  
          break;  
        /* WiFi正在打开过程中状态... */  
        case WifiManager.WIFI_STATE_ENABLING:  
          mTextView01.setText  
          (  
            getResources().getText(R.string.str_wifi_enabling)  
          );  
          break;  
        /* WiFi正在关闭过程中... */  
        case WifiManager.WIFI_STATE_DISABLING:  
          mTextView01.setText  
          (  
            getResources().getText(R.string.str_wifi_disabling)  
          );  
          break;  
        /* WiFi已经关闭 */  
        case WifiManager.WIFI_STATE_DISABLED:  
          mTextView01.setText  
          (  
            getResources().getText(R.string.str_wifi_disabled)  
          );  
          break;  
        /* 无法取得或识别WiFi状态 */  
        case WifiManager.WIFI_STATE_UNKNOWN:  
        default:  
          mTextView01.setText  
          (  
            getResources().getText(R.string.str_wifi_unknow)  
          );  
          break;  
      }  
    }  
    catch(Exception e)  
    {  
      mTextView01.setText(e.toString());  
      e.getStackTrace();  
    }  
    super.onResume();  
  }  
    
  @Override  
  protected void onPause()  
  {  
    // TODO Auto-generated method stub  
    super.onPause();  
  }  
}  







view plain copy
<?xml version="1.0" encoding="utf-8"?>  
<manifest  
  xmlns:android="http://schemas.android.com/apk/res/android"  
  package="irdc.wifi"  
  android:versionCode="1"  
  android:versionName="1.0.0">  
  <application  
    android:icon="@drawable/icon"  
    android:label="@string/app_name">  
    <activity  
      android:name="irdc.wifi.wifi"  
      android:label="@string/app_name">  
      <intent-filter>  
        <action android:name="android.intent.action.MAIN" />  
        <category android:name="android.intent.category.LAUNCHER" />  
      </intent-filter>  
    </activity>  
  </application>  
  <!-- 声明WIFI以及网络等相关权限 -->  
  <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE"></uses-permission>  
  <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"></uses-permission>  
  <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses-permission>  
  <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"></uses-permission>  
  <uses-permission android:name="android.permission.INTERNET"></uses-permission>  
  <uses-permission android:name="android.permission.WAKE_LOCK"></uses-permission>  
</manifest> 






view plain copy
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout  
  xmlns:android="http://schemas.android.com/apk/res/android"  
  android:background="@drawable/white"  
  android:orientation="vertical"  
  android:layout_width="fill_parent"  
  android:layout_height="fill_parent"  
  >  
  <TextView  
    android:id="@+id/myTextView1"  
    android:layout_width="fill_parent"   
    android:layout_height="wrap_content"  
    android:textColor="@drawable/blue"  
    android:text="@string/hello"  
  />  
  <CheckBox  
    android:id="@+id/myCheckBox1"  
    android:layout_width="wrap_content"  
    android:layout_height="wrap_content"  
    android:text="@string/str_checked"  
    android:textColor="@drawable/blue"  
  />  
</LinearLayout> 
