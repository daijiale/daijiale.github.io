title: 【Android-持续更新】Android开发常用知识点总结
date: 2015-7-10 15:13:09
tags:

 - Android

categories:

 - 原创博文
 - Android开发笔记 

---


# Android开发常用知识点总结：

<!--more-->

## Android异步加载：
  
  用异步的方式去加载数据（主要是图像，不解释），可以让用户在刷新数据时候不用经历太多卡顿，这也是Google官方要求我们去做的一种方式。
  
  - 为什么要使用异步加载：Android单线程模型以及耗时操作阻塞UI线程。
  - 异步加载最常用的两种方式：线程池以及AsyncTask。
  
###   AsyncTask流程：
  
####   字符串异步加载
  1.在`onCreate`方法中，`new NewsAsyncTask().execute(URL)`;传入所对应需要异步访问的URL。
  
  2.将URL传入下面`AsyncTask`中的`getJsonData()`。
  
  3.再通过`getJsonData()`中的Json解析操作，将网络Json数据存储到程序里封装的`NewsBean`数据项对象 (和 `Json Obj`格式一一对应的关系) 中。
  
  4.在`AsyncTask`的`onPostExecute`方法中，将`NewsBean`数据传递给`NewsAdapter`，来构造出`ListView`的数据源。
  
  ```
  class NewAsyncTask extend AsyncTask<String,Void,List<NewsBean>>{
  	protected List<NewsBean>doInBackground(...){
  		return getJsonData(params[0]);
  	}
  	
  	protected onPostExecute(List<NewsBean> newsBean){
  		super.onPostExecute(newsBean);
  		NewsAdapter adapter = new Newsadapter(MainActivity.this,newsBean);
  		mListView.setAdapter(adapter);  	
  	}
  	  	
  }
  
 ```
  
####  图像异步加载


##### 多线程方式
1.新建`ImageLoader`类，使用多线程去加载图片，引入`showImageByThread(ImageView imageView,String url)`方法，去通过`URL`获取图片数据，并转化成`Bitmap`数据，并在`Adapter`中调用该方法.

```

public class ImageLoad {
	public void showImageByThread(ImageView imageView,String url) {		new Thread(){
			public void run(){
			super.run();
			
		}		
	}.start();
}
	public Bitmap getBitmapFromURL(String urlString){
		Bitmap bitmap;
		InputStream is;
		try {
			URL url = new URL（urlString）；
			HttpURLConnection connection = (HttpURLConnection)url.openConnection();
			is = new BufferedInputStream(connection.getInputStream());
			bitmap = BitmapFactory.decodeStream(is);
			connection.disconnect();
			return bitmap;
		}catch(java.io.IOException e){
			e.printStackTrace();
		}finally{
			is.close();
		}
		return null;
	}
  
  }
  
```

2.在`NewsAdapter`类中设置`xml`文件对应的`ImageView`控件ID和`showImageByThread`绑定。

3.在子线程中去执行`bitmap`获取函数，并设置`handler`来绑定`ImageView`

 ```
public class ImageLoad {
       
       private ImageView mImageView;
       private Handler handler = new Handler(){
       		
       		public void handleMessage(Message msg){
       			super.handleMessage(msg);
       			
       			//将消息传递来的bitmap取出，设置给imageview
       			
       			mImageView.setImageBitmap((Bitmap)msg.obj);      			
       		}
       
       };
  

	public void showImageByThread(ImageView imageView,final String url) {
			mImageView = imageView;
			new Thread(){
				public void run(){
					super.run();
					Bitmap	bitmap = getBitmapFromURL(url);
			/* 虽然我们这里有了一个bitmap，但是由于Android的单线程模型（非主线程是不能直接去编辑UI的），我们无法直接把bitmap赋值给ImageView。所以这里我们需要启一个Handler去做消息的传递。*/
					Message message = Message.obtain();  //使用先用已经回收的message，提高Android系统效率
					message.obj = bitmap;
					handler.sendMessage(message);
				}		
			}.start();
}
	public Bitmap getBitmapFromURL(String urlString){...}
  
  }
  
```

4.最后注意`ListView的缓存机制`：`ListView`会重用`ConvertView`，每次下拉刷新后会从缓冲池中重用旧的图片。为了解决这个问题，我们需要在`Adapter`中增加`Tag`，在`handler`中来进行身份判定,确定`handle`接收到的`Tag`和传进的`URL`是相同的时候，才设置`Bitmap`，使得正确的`Item`显示正确的数据。（这是处理异步加载错乱常用的一种方式，另一种是：用成员变量，对数据进行缓冲来消除由于网络获取时间不确定导致时序顺序错误。）

NewsAdapter类中：

```
//在NewsAdapter中，设置图片UrlTag用作判定条件
String url = mList.getr(position).newsIconUrl;
viewHolder.ivIcon.setTag(url);

```

ImageLoad类中：

```

//在ImageLoad中，增加`handler`中`bitmap`绑定`ImageView`的判定条件

 private ImageView mImageView;
       private Handler handler = new Handler(){
       		
       		public void handleMessage(Message msg){
       			super.handleMessage(msg);
       			
       			//将消息传递来的bitmap取出，设置给imageview
       			if(mImageView.getTag().equals(url)){
       				mImageView.setImageBitmap((Bitmap)msg.obj);      			}
       		}
       
       };


```
  
#####   AsyncTask方式

1.在`Adapter`中新建`showImageByAsyncTask(ImageView imageView,String url)`方法

2.紧接着，在`Adapter`中创建`NewsAsyncTask extend AsyncTask`去加载`ImageView`并绑定

3.在`AsyncTask`的`onPostExecute`方法中，设置`Bitmap`绑定到`imageVIew`上，即：`mImageView.setImageBitmap(bitmap)`;

4.最后注意增加Tag来解决异步加载错乱的问题:

```
	if(mImageView.getTag().equals(mUrl)){
		mImageView.setImageBitmap(bitmap);
	}

```

#### 网络加载优化

##### 优化原因

图片资源过大，最好不要一滑动就重复加载。为了解决这个问题，我们增加缓存机制，提高用户体验。

##### Lru算法
- Lru: Least Recently Used 近期最少使用算法。
- Android提供了LruCache类来实现这个缓存算法。

##### 使用方法

```
private LruCache<String,Bitmap> mCaches;

public ImageLoader ( ) {
	int maxMemory = (int) Runtime.getRuntime().maxMemory();//获取最大可用内存
	int cacheSize = maxMemory / 4;
	mCaches = new LruCache<String , Bitmap>(cacheSize){
		protected int sizeof(String key,Bitmap value){
		//每次存入缓存的时候调用
			return value.getByteCount();
		}
	};
}

//增加到缓存
public void addBitmapToCache(String url,Bitmap bitmap){
	if(getBitmapFromCache(url) == null ){
		mCaches.put(url,bitmap);
	
	}
}

//从缓存中取出数据
public Bitmap getBitmapFromCache(String url){
	return mCaches.get(url);
}

//以AsyncTask为例，之后我们需要先判断一下缓存中是否已经有了一个URL对应的图片。

public void showImageByAsyncTask(ImageView imageView,String url){
		//从缓存中取出图片
	 Bitmap bitmap = getBitmapFromCache(url);
 		if(bitmap == null){
 		//如果缓存中没有，则必须去网络异步下载
 			new NewsAsyncTask(imageView,url).execute(url);
 		}else{
 			imageView.setImageBitmap(bitmap);
 	}

}


//修改NewsAsyncTask方法，将网络异步下载下的图片保存到LruCache中。

private class NewsAsyncTasks extends AsyncTask<String,Void,Bitmap>{
	
	private ImageView mImageView;
	private String mUrl;
	
	public NewsAsyncTask(ImageView imageView,String url){
		mImageView = imageView;
		mUrl = url;	
	}
	
	protected Bitmap doInBackground(String...params){
		String url = params[0];
		//从网络上获取图片
		Bitmap bitmap ＝ getBitmapFromURL（url）；
		
		if ( bitmap ! = null ) {
		//将不在缓存的图片加入缓存
		addBitmapToCache(url,bitmap);
		}
		return bitmap;
	}
	
	......
}

```


  
## Android字符流
 
  注意用InputStream去读取字节流时候需要用InputStreamReader转化成为UTF8字符流传入BufferedReader，再通过定义string content 去循环接收BufferedReader的字符流数据。
  
##Android.util.Log
  android.util.Log常用的方法有以下5个：Log.v() Log.d() Log.i() Log.w() 以及 Log.e() 。根据首字母对应VERBOSE，DEBUG,INFO, WARN，ERROR。

1、Log.v 的调试颜色为黑色的，任何消息都会输出，这里的v代表verbose啰嗦的意思，平时使用就是Log.v("","");

2、Log.d的输出颜色是蓝色的，仅输出debug调试的意思，但他会输出上层的信息，过滤起来可以通过DDMS的Logcat标签来选择.

3、Log.i的输出为绿色，一般提示性的消息information，它不会输出Log.v和Log.d的信息，但会显示i、w和e的信息

4、Log.w的意思为橙色，可以看作为warning警告，一般需要我们注意优化Android代码，同时选择它后还会输出Log.e的信息。

5、Log.e为红色，可以想到error错误，这里仅显示红色的错误信息，这些错误就需要我们认真的分析，查看栈的信息了。

## Android Handler



