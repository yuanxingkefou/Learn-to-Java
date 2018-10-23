#HashMap

**HashMap的数据结构：**

![image]()

HashMap在JDK1.8中采用的是数组+链表+红黑树的结构。在链表过长的时候可以通过转换成红黑树提升访问性能

**HashMap的继承关系：**

```
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<k,V>,Clonable,Serializable
{
    //保证序列化时候版本的一致性
    private static final long serialVersionUID=362498820763181265L;
    
    //map的默认初始化容量 16
    static final int DEAFULT_INITIAL_CAPACITY=1<4;
    
    //map的最大容量 2^30
    static final int MAXIMUM_CAPACITY=1<<30;
    
    //默认的填充因子0.75；能较好的平衡时间和空间的消耗
    static final int DEAFULT_LOAD_FACTOR=0.75f;
    
    //亮标需要转化为红黑树时的临界值
    static final int TREEIFY_THRESHOLD=8;
    
    //将红黑树转化为链表的临界值
    static final int UNTREEIFY_THRESHOLD=6;
    
    //转变为树的table的最小容量，小于该值不会进行树化
    static final int MIN_TREEIFY_CAPACITY=64;
    
    /*-------Node结构-----------*/
    static class Node<K,V> implements Map.Entry<K,V>
    {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;
        
        Node(int hash ,K key, V value, Node<K,V> next)
        {
            this.hash=hash;
            this.key=key;
            this.value=value;
            this.next=next;
        }
        
        public final K getKey()   {   retrn key;  };
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }
        
        public final int hashCode()
        {
            return Objects.hashCode(key)^Objects.hashCode(value);
        }
        
        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }
        
        public final boolean equals(Object o)
        {
            if(o==this)
                return this;
                
            if(o instanceof Map.Entry)
            {
                Map.Entry<?,?> e=(Map.Entry<?,?>)o;
                if(objects.equals(key,e.getKey())&&
                   objects.equals(value,e.getValue())
                   return true;
            }
            return false;
        }
    }
    
    /* -------------Static utilities---------------- */
    static final int hash(Object key)
    {
        int h;
        return (key==null)?0:(h=key.hashCode())^(h>>>16);
    }
    
    /**
     * Returns x's Class if it is of the form "class C implements
     * Comparable<C>", else null.
     */
    static Class<?> comparableClassFor(Object x) {
        if (x instanceof Comparable) {
            Class<?> c; Type[] ts, as; Type t; ParameterizedType p;
            if ((c = x.getClass()) == String.class) // bypass checks
                return c;
            if ((ts = c.getGenericInterfaces()) != null) {
                for (int i = 0; i < ts.length; ++i) {
                    if (((t = ts[i]) instanceof ParameterizedType) &&
                        ((p = (ParameterizedType)t).getRawType() ==
                         Comparable.class) &&
                        (as = p.getActualTypeArguments()) != null &&
                        as.length == 1 && as[0] == c) // type arg is c
                        return c;
                }
            }
        }
        return null;
    }
    
    /**
     * Returns k.compareTo(x) if x matches kc (k's screened comparable
     * class), else 0.
     */
    @SuppressWarnings({"rawtypes","unchecked"}) // for cast to Comparable
    static int compareComparables(Class<?> kc, Object k, Object x) {
        return (x == null || x.getClass() != kc ? 0 :
                ((Comparable)k).compareTo(x));
    }
    
    //将初始化容量转化大于或等于最接近输入参数的2的整数幂的数：
    static final int tableSizeFor(int cap) {
        
        //如果cap恰好是2的整数次幂，那么返回的也是本身
        int n = cap - 1;
        
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
    
    /* ------------Fields-------------- */
    
    transient Node<K,V>[] table;
    
    //map中的键值对集合
    transient Set<Map.Entry<K,V>> entrySet;
    
    //map中键值对的数量
    transient int size;
    
    //用于统计map修改次数的计数器，用于fail-fast抛出ConcurrentModifaicationException
    transient int modCount;
    
    //大于该阀值，则重新进行扩容，threshold=capacity(table.length)* load factor
    int threshold;
    
    //填充因子
    final float loadFactor;
    
    
    /* ---------------Public operations------------------ */
    
    //各种构造函数
    
    //无参数的构造函数
    
    public HashMap()
    {
        this.loadFactor=DEFAULT_LOAD_FACTOR;
    }
    
    //传初始化容量（如果知道要使用的map容量，使用这种）
    public HashMap(int initialCapactity)
    {
        this(initialCapacity,DEFAULT_LOAD_FACTOR);
    }
    
    //传初始化容量以及填充因子
    public HashMap(int initialCapacity,float loadFactor)
    {
        if(initialCapacity<0)
            throw new IllegalArgumentException("Illegal initial capacity: "+initialCapacity);
        
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
            
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor=loadFactor;
        this.threshold=tableSizeFor(initialCapacity);
    }
    
    //传map转化为HashMap的构造函数
    
    public HashMap(Map<? extends K,? extends V> m)
    {
        this.loadFactor=DEFAULT_LOAD_FACTOR;
        putMapEntries(m,false);
    }
    
    //evict表示是不是初始化map,false表示是初始化map
    final void putMapEntries(Map<? extends K,? extends V> m,boolean evict)
    {
        int s = m.size();
        if (s > 0) {
            if (table == null) { // pre-size
                
                //计算map的容量，键值对的数量=容量*填充因子
                float ft = ((float)s / loadFactor) + 1.0F;
                int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                         (int)ft : MAXIMUM_CAPACITY);
                if (t > threshold)
                    threshold = tableSizeFor(t);
            }
            //如果table已经有，且键值对数量大于了阀值，进行扩容
            else if (s > threshold)
                resize();
            for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
                K key = e.getKey();
                V value = e.getValue();
                putVal(hash(key), key, value, false, evict);
            }
        }
    }
    
    public int size() {
        return size;
    }
    
    public boolean isEmpty() {
        return size == 0;
    }
    
    public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }
    
    final Node<K,V> getNode(int hash,Object key)
    {
        Node<K,V> tab;
        Node<K,V> first,e;
        int n;
        K k;
        
        //判断table是否为空，根据hash找到存放的table数组的下标，并赋值给临时变量
        if((tab=table)!=null&&(n=tab.length)>0&&
            (first=tab[(n-1)&hash])!=null)
        {
            //先检查数组下标第一个节点是否满足key,满足则返回
            if(first.hash==hash&&
                  ((k=first.key)==key||(key!=null&&key.equals(k))))
            return first;
            
            如果第一个与key不相等，则循环查看桶
            if((e=first.next)!=null)
            {
                if(first instanceof TreeNode)
                    return ((TreeNode<K,V>first).getTreeNode(hash,key);
                do
                {
                    if(e.hash==hash&&((k=e.key)==key||(key!=null&&key.equals(k))))
                    return e;
                }while((e=e.next)!=null);
            }
        }
        return null;
    }
    
    public boolean containsKey(Object key) {
        return getNode(hash(key), key) != null;
    }
    
    public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    
    /**
     * Implements Map.put and related methods
     *
     * @param hash hash for key
     * @param key the key
     * @param value the value to put
     * @param onlyIfAbsent if true, don't change existing value
     * @param evict if false, the table is in creation mode.
     * @return previous value, or null if none
     */
    final V putVal(int hash,K key,V value,boolean onlyIfAbsent,boolean evict)
    {
        Node<K,V>[] tab;
        Node<K,V> p;
        int n,i;
        
        //如果为空，则扩容
        if((tab=table)==null||(n=tab.length)==0)
            n=(tab=resize()).length;
        
        //如果tab对应的数组位置为空，则床架新的Node，并指向它
        if((p=tab[i=(n-1)&hash])==null)
            tab[i]=newNode(hash,key,value,null);
        else
        {
            Node<K,V> e;
            K k;
            
            //如果比较hash和key的值都相等，那么要put的键值对已经在里面，赋值给e
            if(p.hash==hash&&((k=p.key)==key||(key!=null&&key.equals(k))))
                e=p;
            
            //如果p为树节点，则执行插入树的操作
            else if(p instanceof TreeNode)
                e=((TreeNode<K,V>)p.putTreeVal(this,tab,hash,key,value);
            
            //在桶中查找
            else
            {
                for(int binCount=0;;++binCount)
                {
                    //找到了最后一个都不满足的话，则在最后插入节点
                    if((e=p.next)==null)
                    {
                        p.next=newNode(hash,key,value,null);
                        if(binCount>=TREEIFY_THRESHOLD-1)
                            treeifyBin(tab,hash);
                        break;
                    }
                    if(e.hash==hash&&((k=e.key)==key||(key!=null&&key.equals(k))))
                        break;
                    p=e;
                }
            
            //上面循环中找到了e，则根据onlyIfAbsent是否为true来决定是否替换旧值
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                    
                //钩子函数
                afterNodeAccess(e);
                return oldValue;
            }
        //修改计数器+1
        ++modCount;
        if(++size>threshold)
            resize();
        
        //钩子函数，用于给LinkedHashMap继承后使用，在HashMap里是空的
        afterNodeAccess(e);
        return null;
    }
    
    ```
    
    

    
    

    
    
    
    
    
    

    
