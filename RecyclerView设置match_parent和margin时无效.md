将布局填充成View对象，一般有两种方法：View类的inflate静态方法和LayoutInflater类的inflate方法。以前我都是用的前者，因为它用起来简单一些，代码短一些，例如:

**采用View的inflate方法**  
View itemView = View.inflate(this, R.layout.item_view, null);  

**采用LayoutInflate的inflate方法**  
View itemView = LayoutInfalte.from(this).inflate(R.layout.item_view, parent, false);

我一直认为这两个方法实现是一样的，但这次在RecyclerView中的item中使用margin怎么设置都没有效果，然后我又试了下padding，发现是可以正常实现的，代码没有错，那么错的就是使用方法了，经过多方查证，发现使用LayoutInflate的inflate方法时margin是有效的，这才意识到原来这两个inflate方法是有区别的。

### View inflate方法和LayoutInflater inflate方法的区别 ###

LayoutInflater类的inflate方法适用于所有需要进行布局填充的场景，是Android中专门进行布局填充的方法，Android中其他需要使用布局填充的地方，都会调用本方法，而不是View类中的inflate方法。该方法不是静态方法，需要先创建LayoutInflater类的对象才能调用。

View类中的inflate方法内部包裹了LayoutInflater类的inflate方法，这个方法是一个静态方法，不需要创建View类的对象，直接使用View类名调用，相比上一种方法是一种简便方法。但很明显，这个方法不如上一个方法功能强大。

### 实际区别 ###
    public static View inflate(Context context, @LayoutRes int resource, ViewGroup root) {
        LayoutInflater factory = LayoutInflater.from(context);
        return factory.inflate(resource, root);
    }  
这是View类的inflate方法的源码，可以看出实际也是调用的LayoutInflate类的inflate方法，那么是什么原因导致这两个方法的区别的呢？让我们再深入源码看看。
 
    public View inflate(@LayoutRes int resource, @Nullable ViewGroup root) {
        return inflate(resource, root, root != null);
    }
这是LayoutInflate类的两个参数的inflate方法，从源码可以看出，实际也是调用的三个参数的inflate方法。所以，最终的问题归结于下面这个方法：

    /**
     * Inflate a new view hierarchy from the specified xml resource. Throws
     * {@link InflateException} if there is an error.
     * 
     * @param resource ID for an XML layout resource to load (e.g.,
     *        <code>R.layout.main_page</code>)
     * @param root Optional view to be the parent of the generated hierarchy (if
     *        <em>attachToRoot</em> is true), or else simply an object that
     *        provides a set of LayoutParams values for root of the returned
     *        hierarchy (if <em>attachToRoot</em> is false.)
     * @param attachToRoot Whether the inflated hierarchy should be attached to
     *        the root parameter? If false, root is only used to create the
     *        correct subclass of LayoutParams for the root view in the XML.
     * @return The root View of the inflated hierarchy. If root was supplied and
     *         attachToRoot is true, this is root; otherwise it is the root of
     *         the inflated XML file.
     */
    public View inflate(@LayoutRes int resource, @Nullable ViewGroup root, boolean attachToRoot)
看一下root参数的说明，提供一个可选的view作为布局层次的父布局（如果attachToRoot为true），或者仅仅作为一个提供LayoutParams参数的Object（如果attachToRoot为false）。关键就在于这个root参数，若root为null，则设置margin无效。