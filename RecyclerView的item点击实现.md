RecyclerView的item点击事件实现起来比较复杂，记录一下。

### 方法一 : RecyclerView的addOnItemTouchListener()###

	public abstract class OnRecyclerItemClickListener implements RecyclerView.OnItemTouchListener {
		private GestureDetectorCompat mGestureDetector;
    	private RecyclerView recyclerView;

	    public OnRecyclerItemClickListener(RecyclerView recyclerView) {
	        this.recyclerView = recyclerView;
	        mGestureDetector = new GestureDetectorCompat(recyclerView.getContext(), new ItemTouchHelperGestureListener());
	    }

	    @Override
	    public boolean onInterceptTouchEvent(RecyclerView rv, MotionEvent e) {
	        mGestureDetector.onTouchEvent(e);
	        return false;
	    }
	
	    @Override
	    public void onTouchEvent(RecyclerView rv, MotionEvent e) {
	        mGestureDetector.onTouchEvent(e);
	    }
	
	    @Override
	    public void onRequestDisallowInterceptTouchEvent(boolean disallowIntercept) {
	    }
	
	    private class ItemTouchHelperGestureListener extends GestureDetector.SimpleOnGestureListener {
	        @Override
	        public boolean onSingleTapUp(MotionEvent e) {
	            View child = recyclerView.findChildViewUnder(e.getX(), e.getY());
	            if (child != null) {
	                RecyclerView.ViewHolder vh = recyclerView.getChildViewHolder(child);
	                onItemClick(vh);
	            }
	            return true;
	        }
	
	        @Override
	        public void onLongPress(MotionEvent e) {
	            View child = recyclerView.findChildViewUnder(e.getX(), e.getY());
	            if (child != null) {
	                RecyclerView.ViewHolder vh = recyclerView.getChildViewHolder(child);
	                onItemLongClick(vh);
	            }
	        }
	    }
	
	    public abstract void onItemClick(RecyclerView.ViewHolder vh);
	
	    public abstract void onItemLongClick(RecyclerView.ViewHolder vh);
	}
然后设置RecyclerView

    mRecyclerView.addOnItemTouchListener(new OnRecyclerItemClickListener(mRecyclerView) {
        @Override
        public void onItemClick(RecyclerView.ViewHolder vh) {
            //这里实现单击响应
        }

        @Override
        public void onItemLongClick(RecyclerView.ViewHolder vh) {
			//这里实现长按响应
        }
    });

### 方法二 : 接口抛出 ###
这种方法和ListView一样,在Adapter里面创建View时添加点击事件

	@Override
	public void bindCustomViewHolder(SingleHolder holder, final int position) {
	    Person person = getItem(position);
	    holder.nameView.setText(person.getName());
	    holder.ageView.setText(String.valueOf(person.getAge()));
	 
	    if (clickListener != null) {
	        holder.nameView.setOnClickListener(new View.OnClickListener() {
	            @Override
	            public void onClick(View v) {
	                clickListener.onNameClick(position);
	            }
	        });
	        holder.ageView.setOnClickListener(new View.OnClickListener() {
	            @Override
	            public void onClick(View v) {
	                clickListener.onAgeClick(position);
	            }
	        });
	    }
	}
然后在Adapter对象上添加监听回调:

	singleAdapter.setClickListener(new SingleAdapter.OnSingleItemClickListener() {
	    @Override
	    public void onNameClick(int position) {
	        DevLog.i("adapter click name:" + position);
	        Toast.makeText(SingleActivity.this, "adapter click name:" + position, Toast.LENGTH_SHORT).show();
	    }
	 
	    @Override
	    public void onAgeClick(int position) {
	        DevLog.i("adapter click age:" + position);
	        Toast.makeText(SingleActivity.this, "adapter click name:" + position, Toast.LENGTH_SHORT).show();
	    }
	});