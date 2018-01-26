## Android开发之如何获取屏幕、状态栏、导航栏和虚拟功能键的高度 ##
先说说这些概念到底指的哪里吧。这里以我说的为准，至于对不对，管他呢，你知道是指的哪里就可以了。如下图，由于获取屏幕高度不会包含下面虚拟功能键的高度，所以当一个Android手机有虚拟键存在时，对屏幕高度的适配是一个非常烦的事，比如说，华为手机，华为手机，还有华为手机。</br>![](https://github.com/kangisme/img/raw/master/Screenshot_2018-01-06-15-31-56.png)
### 1.获取屏幕高度和宽度 ###
可以通过WindowManager获取，代码如下：

    Display display = getWindowManager().getDefaultDisplay();  
    Point size = new Point();  
    display.getSize(size);  
    int width = size.x;  
    int height = size.y;  
或者这样也可以获取

    int width = getResources().getDisplayMetrics().heightPixels;  
    int height = getResources().getDisplayMetrics().widthPixels;  
我更倾向于使用第二种方法，更简单更好用，不过两者都需要在Activity内或者拥有Context实例
### 2.获取状态栏的高度 ###
获取状态栏高度就比较复杂了，需要用到Java反射知识，具体百度，代码如下：

    public static int getStatusBarHeight(Context context){  
       Class<?> c = null;  
       Object obj = null;  
       Field field = null;  
       int x = 0, statusBarHeight = 0;  
       try {  
       c = Class.forName("com.android.internal.R$dimen");  
       obj = c.newInstance();  
       field = c.getField("status_bar_height");  
       x = Integer.parseInt(field.get(obj).toString());  
       statusBarHeight = context.getResources().getDimensionPixelSize(x);  
       } catch (Exception e1) {  
       e1.printStackTrace();  
       }  
       return statusBarHeight;  
    }  
### 3.获取导航栏的高度 ###
    public int getNavigationBarHeight(Activity activity) {  
       Resources resources = activity.getResources();  
       int resourceId = resources.getIdentifier("navigation_bar_height","dimen", "android");  
       //获取NavigationBar的高度  
       int height = resources.getDimensionPixelSize(resourceId);  
       return height;  
    }  
### 4.去除通知栏和导航栏 ###
除了选择特定的主题这个方法外，还可以在代码中设置来达到去除通知栏和导航栏的目的，代码如下：

    //去除导航栏
    requestWindowFeature(Window.FEATURE_NO_TITLE);
    //设置全屏，即去掉状态栏
    getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,WindowManager.LayoutParams.FLAG_FULLSCREEN);
需要强调的是，这两行代码必须在super.onCreate()后setContentView()之前调用，不然会报错！！！    
### 5.获取虚拟功能键的高度 ###
    public class NavigationBar {
    
	    /**
	     * 获取虚拟键的高度
	     * @param context
	     * @return
	     */
	    public static int getNavigationBarHeight(Context context) {
	    int result = 0;
	    if (hasNavBar(context)) {
	    Resources res = context.getResources();
	    int resourceId = res.getIdentifier("navigation_bar_height", "dimen", "android");
	    if (resourceId > 0) {
	    result = res.getDimensionPixelSize(resourceId);
	    }
	    }
	    return result;
	    }
	    
	    /**
	     * 判断是否有虚拟键
	     * @param context
	     * @return
	     */
	    public static boolean hasNavBar(Context context) {
	    boolean hasNavigationBar = false;
	    Resources rs = context.getResources();
	    int id = rs.getIdentifier("config_showNavigationBar", "bool", "android");
	    if (id > 0) {
	    hasNavigationBar = rs.getBoolean(id);
	    }
	    try {
	    Class systemPropertiesClass = Class.forName("android.os.SystemProperties");
	    Method m = systemPropertiesClass.getMethod("get", String.class);
	    String navBarOverride = (String) m.invoke(systemPropertiesClass, "qemu.hw.mainkeys");
	    if ("1".equals(navBarOverride)) {
	    hasNavigationBar = false;
	    } else if ("0".equals(navBarOverride)) {
	    hasNavigationBar = true;
	    }
	    } catch (Exception e) {
	    }
	    return hasNavigationBar;
	    }
    }