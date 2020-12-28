Android开发 使用实现 MediaPlayer 音频播放
---------------------------

# 简介:
> 本篇文章是介绍 MediaPlayer 播放音频的简单运用(不包含视频).

# 目录:
[1.什么是 MediaPlayer](#1)
[2.MediaPlayer 的常用方法](#2)
[3.MediaPlayer 的简单运用](#3)

# <span id = "1">**1.什么是 MediaPlayer**</span>

### 介绍:
`MediaPlayer` 是处于Android多媒体包下 `"android.media.MediaPlayer"` 的 Android 自带的多媒体库, 一般我们用于实现**音频**和**视频**(视频需要和其他控件一起使用比如: SurfaceView 和 VideoView 等)的操作.

# <span id = "2">**2.MediaPlayer 的常用方法**</span>

### 实例化:
| 方法名 | 说明 |
| :-: | :-: |
| create(Context context) | 创建一个 MediaPlayer 实例 |
| create(Context context,int resid) | 通过给定的 id 来创建一个 MediaPlayer 实例 |
| create(Context context,Uri uri) | 通过给定的 Uri 来创建一个 MediaPlayer 实例 |

### 常用方法:
| 方法名 | 说明 |
| :-: | :-: |
| void setDataSource(String path) | 通过一个具体的路径来设置MediaPlayer的数据源，path可以是本地的一个路径，也可以是一个网络路径 |
| void setDataSource(Context context, Uri uri) | 通过给定的Uri来设置MediaPlayer的数据源，这里的Uri可以是网络路径或是一个ContentProvider的Uri。 |
| void setDataSource(MediaDataSource dataSource) | 通过提供的MediaDataSource来设置数据源 |
| void setDataSource(FileDescriptor fd)  | 通过文件描述符FileDescriptor来设置数据源 |
| int getCurrentPosition() | 获取当前播放的位置 |
| int getAudioSessionId() | 返回音频的 session ID |
| int getDuration() | 得到文件的时长 |
| TrackInfo[] getTrackInfo()  | 返回一个 track 信息的数组 |
| boolean isLooping () | 是否循环播放 |
| boolean isPlaying()  | 是否正在播放 |
| void pause ()  | 暂停 |
| void start ()  | 开始 |
| void stop ()  | 停止 |
| void prepare()  | 同步的方式装载流媒体文件。 |
| void prepareAsync() | 异步的方式装载流媒体文件。 |
| void reset() | 重置 MediaPlayer 至未初始化状态。 |
| void release () | 回收流媒体资源。 |
| void seekTo(int msec)  | 指定播放的位置（以毫秒为单位的时间） |
| void setAudioStreamType(int streamtype)  | 指定流媒体类型 |
| void setLooping(boolean looping)  | 设置是否单曲循环 |
| void setNextMediaPlayer(MediaPlayer next)  | 当前这个 MediaPlayer 播放完毕后， MediaPlayer next 开始播放 |
| void setWakeMode(Context context, int mode) | 设置 CPU唤醒的状态。 |

### 监听:
| 方法名 | 说明 |
| :-: | :-: |
| setOnBufferingUpdateListener(MediaPlayer.OnBufferingUpdateListener listener) | 网络流媒体的缓冲变化时回调 |
| setOnCompletionListener(MediaPlayer.OnCompletionListener listener)  | 网络流媒体播放结束时回调 |
| setOnErrorListener(MediaPlayer.OnErrorListener listener)  | 发生错误时回调 |
| setOnPreparedListener(MediaPlayer.OnPreparedListener listener) | 当装载流媒体完毕的时候回调。 |

# <span id = "3">**3.MediaPlayer 的简单运用**</span>

> 这个是我以前交的课堂作业很简单, 把 MediaPlayer 放到 Service 去执行.


### `activity_main.xml`
```

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center_horizontal"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.orange_music.MainActivity" >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/MusicStatus"
        android:text="   "/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/MusicTime"
        android:text="    "/>

    <SeekBar
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/MusicSeekBar"/>

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/BtnPlayorPause"
            android:text="@string/btnPlayorPause"
            android:onClick="onClick"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/BtnStop"
            android:text="@string/btnStop"
            android:onClick="onClick"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/BtnQuit"
            android:text="@string/btnQuit"
            android:onClick="onClick"/>

    </LinearLayout>

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btnPre"
            android:text="@string/btnPre"
            android:onClick="onClick" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/btnNext"
            android:text="@string/btnNext"
            android:onClick="onClick"/>

    </LinearLayout>

</LinearLayout>

```
> 布局很简单就是几个 TextView ,几个 Button 和一个 SeekBar.




### `MainActivity.java`
```

public class MainActivity extends Activity implements OnClickListener{
	
	private MusicService musicService;
    private SeekBar seekBar;
    private TextView musicStatus, musicTime;
    private Button btnPlayOrPause, btnStop, btnQuit;
    private SimpleDateFormat time = new SimpleDateFormat("m:ss");
	
	private ServiceConnection sc = new ServiceConnection(){


		@Override
		public void onServiceConnected(ComponentName componentName, IBinder iBinder) {
			// TODO Auto-generated method stub
			musicService = ((MusicService.MyBinder)iBinder).getService();
		}

		@Override
		public void onServiceDisconnected(ComponentName arg0) {
			// TODO Auto-generated method stub
			musicService = null;
		}
		
	};

	
	
	private void bindServiceConnection(){
		Intent intent = new Intent(MainActivity.this,MusicService.class);
		startService(intent);
		bindService(intent, sc, this.BIND_AUTO_CREATE);
	}

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		Log.d("hint", "ready to new MuiscService");
		musicService = new MusicService();
		Log.d("hint", "finish to new MusicService");
		bindServiceConnection();
		
		seekBar = (SeekBar)this.findViewById(R.id.MusicSeekBar);
        seekBar.setProgress(musicService.mp.getCurrentPosition());
        seekBar.setMax(musicService.mp.getDuration());

        musicStatus = (TextView)this.findViewById(R.id.MusicStatus);
        musicTime = (TextView)this.findViewById(R.id.MusicTime);

        btnPlayOrPause = (Button)this.findViewById(R.id.BtnPlayorPause);

        Log.d("hint", Environment.getExternalStorageDirectory().getAbsolutePath()+"/You.mp3");
		
	}
	
	
	public android.os.Handler handler = new android.os.Handler();
	public Runnable runnable = new Runnable() {
	    @Override
	    public void run() {
	        if(musicService.mp.isPlaying()) {
	            musicStatus.setText(getResources().getString(R.string.playing));
	            btnPlayOrPause.setText(getResources().getString(R.string.pause).toUpperCase());
	        } else {
	            musicStatus.setText(getResources().getString(R.string.pause));
	            btnPlayOrPause.setText(getResources().getString(R.string.play).toUpperCase());
	        }
	        musicTime.setText(time.format(musicService.mp.getCurrentPosition()) + "/"
	                + time.format(musicService.mp.getDuration()));
	        seekBar.setProgress(musicService.mp.getCurrentPosition());
	        seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
	            @Override
	            public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
	                if (fromUser) {
	                    musicService.mp.seekTo(seekBar.getProgress());
	                }
	            }

	            @Override
	            public void onStartTrackingTouch(SeekBar seekBar) {

	            }

	            @Override
	            public void onStopTrackingTouch(SeekBar seekBar) {

	            }
	        });
	        handler.postDelayed(runnable, 100);
	    }
	};

	@Override
	protected void onResume() {
	    if(musicService.mp.isPlaying()) {
	        musicStatus.setText(getResources().getString(R.string.playing));
	    } else {
	        musicStatus.setText(getResources().getString(R.string.pause));
	    }

	    seekBar.setProgress(musicService.mp.getCurrentPosition());
	    seekBar.setMax(musicService.mp.getDuration());
	    handler.post(runnable);
	    super.onResume();
	    Log.d("hint", "handler post runnable");
	}

	public void onClick(View view) {
		// TODO Auto-generated method stub
		 switch (view.getId()) {
	        case R.id.BtnPlayorPause:
	            musicService.playOrPause();
	            break;
	        case R.id.BtnStop:
	            musicService.stop();
	            seekBar.setProgress(0);
	            break;
	        case R.id.BtnQuit:
	            handler.removeCallbacks(runnable);
	            unbindService(sc);
	            try {
	                System.exit(0);
	            } catch (Exception e) {
	                e.printStackTrace();
	            }
	            break;
	        case R.id.btnPre:
	            musicService.preMusic();
	            break;
	        case R.id.btnNext:
	            musicService.nextMusic();
	            break;
	        default:
	            break;
	    }
	}

	@Override
	public void onDestroy() {
	    unbindService(sc);
	    super.onDestroy();
	}

	@Override
	public void onClick(DialogInterface dialog, int which) {
		// TODO Auto-generated method stub
		
	}

	
}


```
> 没有把 MediaPlayer 放到 MainActity, 只做一些控件初始化, 显示进度条和控制 Service.



### `MusicService.java`
```

public class MusicService extends Service{
	
	public final IBinder binder = new MyBinder();
	public class MyBinder extends Binder{
		MusicService getService(){
			return MusicService.this;
		}
	
	}
	//歌曲路径
	private String []musicDir = new String[]{
			Environment.getExternalStorageDirectory().getAbsolutePath() + "/Music/Jameston Thieves Krumm - Unusual Suspects.mp3",
			Environment.getExternalStorageDirectory().getAbsolutePath() + "/Music/Max Vangeli Flatdisk - Blow This Club (Extended Mix).mp3",
			Environment.getExternalStorageDirectory().getAbsolutePath() + "/Music/Kingsfoil - Grapevine Valentine.mp3",
			Environment.getExternalStorageDirectory().getAbsolutePath() + "/Music/VANTAGE    - ∞【 Rediscover 】∞.mp3"
	}; 
	//哪一首歌曲
	private int musicIndex = 1;
	//MediaPlayer-----Idle 状态(闲置状态)
	public static MediaPlayer mp = new MediaPlayer();
	
	public MusicService(){
		
		try {
			musicIndex = 1;
			//MediaPlayer-----Initialized 状态(初始化状态)
			mp.setDataSource(musicDir[musicIndex]);
			//MediaPlayer-----Prepared 状态(准备状态)
			mp.prepare();
		} catch (Exception e) {
			// TODO: handle exception
			Log.d("hint", "can't get to the song");
			e.printStackTrace();
		}
	}
	
	public void playOrPause() {
		// TODO Auto-generated method stub
		if (mp.isPlaying()) {
			mp.pause();
		}else {
			mp.start();
		}
	}
	
	public void stop(){
		if (mp != null) {
			mp.stop();
			try {
				mp.prepare();
				mp.seekTo(0);
			} catch (Exception e) {
				// TODO: handle exception
				e.printStackTrace();
			}
		}
	}
	
	public void nextMusic(){
		if (mp != null && musicIndex < 3) {
			mp.stop();
			try {
				mp.reset();
				mp.setDataSource(musicDir[musicIndex+1]);
				musicIndex++;
				mp.prepare();
				//MediaPlayer-----Started状态
				mp.seekTo(0);
				mp.start();
			} catch (Exception e) {
				// TODO: handle exception
				Log.d("hint", "Can't jump next music");
				e.printStackTrace();
			}
		}
	}
	
	public void preMusic(){
		if (mp != null && musicIndex > 0) {
			mp.stop();
			try {
				mp.reset();
				mp.setDataSource(musicDir[musicIndex-1]);
				musicIndex--;
				mp.prepare();
				mp.seekTo(0);
				mp.start();
			} catch (Exception e) {
				// TODO: handle exception
				Log.d("hint", "Can't jump next music");
				e.printStackTrace();
			}
		}
	}
		
	@Override
	public IBinder onBind(Intent intent) {
		// TODO Auto-generated method stub
		return null;
	}

}

```
> 服务是主要逻辑, 用来控制 MediaPlayer.