**comparable和comparator的区别和联系**


两者的方法都会返回一个int值，int的值有3中情况：

* 比较者大于被比较者（也就是compareTo方法里面的对象），那么返回正整数
* 比较者等于被比较者，那么返回0
* 比较者小于被比较者，那么返回负整数

**Comparable**

在java.lang包中

Comparable可以认为是一个内比较器，实现了Comparable接口的类有一个特点，就是这些类是可以和自己比较的

通过实体类实现了这一接口中的compareTo方法，来实现排序
```
import java.lang.Comparable;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.Date;
import java.util.List;
/**
 * 头条页面新闻的排序
 * 时间降序
 * 点击量降序
 * 标题升序
 * 该类实现了Comparable这一接口，并定义了比较规则，但只能实现这一种比较，如果要修改比较规则，还需要再次修改compareTo()方法
 * 可以通过Collections.sort(lists);来进行排序
 * @author ASUS
 *
 */
public class NewsItem implements Comparable{

	private Date pubDate;
	private int hits;
	private String title;
	
	public NewsItem()
	{
		
	}
	
	
	public NewsItem(Date pubDate, int hits, String title) {
		super();
		this.pubDate = pubDate;
		this.hits = hits;
		this.title = title;
	}


	public Date getPubDate() {
		return pubDate;
	}
	public void setPubDate(Date pubDate) {
		this.pubDate = pubDate;
	}
	public int getHits() {
		return hits;
	}
	public void setHits(int hits) {
		this.hits = hits;
	}
	public String getTitle() {
		return title;
	}
	public void setTitle(String title) {
		this.title = title;
	}
	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		
		int result=0;
		if(o instanceof NewsItem)
		{
			//首先按时间排序
			result=this.pubDate.compareTo(((NewsItem) o).pubDate);
			if(0!=result)
				return result;
			else
			{
				//然后按点击量排序
				result=this.hits-((NewsItem) o).hits;
				if(0!=result)
					return result;
				else
				{
					//按标题排序
					result=this.title.compareTo(((NewsItem) o).title);
					if(0!=result)
						return result;
					else
					{
						return 0;
						
					}
				}
			}
		}
		return 0;
	}
	
	public String toString()
	{
		StringBuilder str=new StringBuilder();
		str.append(title).append(" ").
		append(String.valueOf(pubDate)).append(" ").
		append(String.valueOf(hits));
		
		return str.toString();
	}
	public static void main(String[] args) {
		List<NewsItem> news=new ArrayList<>();
		news.add(new NewsItem(new Date(),1000,"武磊进球了"));
		news.add(new NewsItem(new Date(System.currentTimeMillis()-1000*60*60),2000,"火箭输给雷霆"));
		news.add(new NewsItem(new Date(System.currentTimeMillis()+1000*60*60),1000,"体育新闻"));
		System.out.println("----------------");
		System.out.println(news);
		
		System.out.println("-----------------");
		Collections.sort(news);
		
		System.out.println(news);
		
	}
}

```

**Comparator**

在java.util包中,方法是compare()

Comparator可以认为是是一个外比较器，个人认为有两种情况可以使用实现Comparator接口的方式：
1、一个对象不支持自己和自己比较（没有实现Comparable接口），但是又想对两个对象进行比较
2、一个对象实现了Comparable接口，但是开发者认为compareTo方法中的比较方式并不是自己想要的那种比较方式

实现了该接口的类可以认为是业务排序类，有以下优点：

* 解耦：与实体类分离，是专门的业务逻辑类
* 方便：可以应变多种不同的排序规则，只需要有不同排序规则的类重写方法即可

```
public class DateFirst implements Comparator<NewsItem>{

	@Override
	public int compare(NewsItem o1, NewsItem o2) {
		int result=0;		
		result=o1.getPubDate().compareTo(o2.getPubDate());		
		return result;
	}
	
	public static void main(String[] args) {
		List<NewsItem> news=new ArrayList<>();
		
		news.add(new NewsItem(new Date(),1000,"武磊进球了"));
		news.add(new NewsItem(new Date(System.currentTimeMillis()-1000*60*60),2000,"火箭输给雷霆"));
		news.add(new NewsItem(new Date(System.currentTimeMillis()+1000*60*60),1000,"体育新闻"));
		System.out.println("----------------");
		System.out.println(news);
		
		System.out.println("-----------------");
    //这里也可以采用其他的排序规则,而不需要修改实体类
    //Collections.sort(news,new HitsFirst());
		Collections.sort(news,new DateFirst());
		
		System.out.println(news);
	}
}
```
