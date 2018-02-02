一切View的事件可分解为一个或多个MotionEvent，所谓View事件的分发，其实就是对MotionEvent事件的分发过程，即当一个MotionEvent产生了之后，系统需要把这个事件传递给一个具体的View，这个传递的过程就是分发过程。这个过程由三个方法来共同完成：
### 1.public boolean dispatchTouchEvent(MotionEvent ev) ###
用来进行事件的分发。如果事件能够传递给当前View，那么此方法一定会被调用，返回结果受当前View的onTouchEvent和下级View的dispatchTouchEvent方法的影响，表示是否消耗当前事件。

### 2.public boolean onInterceptTouchEvent(MotionEvent ev) ###
在上面方法内部调用，用来判断是否郎酒某个事件，如果当前View拦截了某个人事件，那么在同一个事件序列当中，此方法不会被再次调用，返回结果表示是否拦截当前事件。

### 3.public boolean onTouchEvent(MotionEvent ev) ###
在dispatchTouchEvent方法中调用，用来处理点击事件，返回结果表示是否消耗当前事件，如果不消耗，则在同一个事件序列中，当前View无法再次接收到事件。

以上三个方法的关系：

    public boolean dispatchTouchEvent(MotionEvent ev){
		boolean consume = false;
		if (onInterceptTouchEvent(ev)) {
			consume = onTouchEvent(ev);
		} else {
			consume = child.dispatchTouchEvent(ev);
		}
		
		return consume;
	}