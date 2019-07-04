---
layout: post
title: "ExoPlayer(엑소플레이어) 사용법, 캐싱 하는법"
categories: [Android]
tags: [Android]
---

# What is ExoPlayer?

ExoPlayer는 Android 프레임워크의 일부가 아니며 Android SDK와 별도로 배포되는 공개 소스 프로젝트다. ExoPlayer의 표준 오디오 및 비디오 구성 요소는 Android 4.1에서 출시 된 Android의 MediaCodec API를 기반으로 한다.

ExoPlayer는 HTTP(DASH), SmoothStreaming 및 Common Encryption을 통한 동적 적응 스트리밍과 같은 기능을 지원한다.

# How to use ExoPlayer

```java
...
..
.
private SimpleExoPlayer player;
// 초기 설정
private void setup() {
	player = ExoPlayerFactory.newSimpleInstance(getContext());
  player.setRepeatMode(Player.REPEAT_MODE_ONE);
  player.addListener(eventListener);
  playerView.setPlayer(player);
}
// getMediaSource()를 통해 영상을 준비한다.
private void prepareVideo() {
  Glide.get(getContext()).clearMemory();
  player.prepare(getMediaSource());
}
// 영상을 재생한다.
private void playVideo() {
  player.setPlayWhenReady(true);
}
// 영상을 일시정지한다.
private void stopVideo() {
  player.setPlayWhenReady(false);
}
// ExoPlayer의 상태를 수신한다.
private Player.EventListener eventListener = new Player.EventListener() {
  @Override public void onPlayerStateChanged(boolean playWhenReady, int playbackState) {
    String stateString;
    switch (playbackState) {
      case Player.STATE_IDLE:
        loadingView.setVisibility(View.VISIBLE);
        stateString = "Player.STATE_IDLE      -";
        break;
      case Player.STATE_BUFFERING:
        loadingView.setVisibility(View.VISIBLE);
        stateString = "Player.STATE_BUFFERING -";
        break;
      case Player.STATE_READY:
        stateString = "Player.STATE_READY     -";
        thumbImageView.setVisibility(View.INVISIBLE);
        loadingView.setVisibility(View.INVISIBLE);
        break;
      case Player.STATE_ENDED:
        stateString = "Player.STATE_ENDED     -";
        break;
      default:
        thumbImageView.setVisibility(View.INVISIBLE);
        loadingView.setVisibility(View.VISIBLE);
        stateString = "UNKNOWN_STATE             -";
        break;
    }
    Timber.d("MainRecCityVideo changed state to "
             + stateString
             + " playWhenReady: "
             + playWhenReady);
  }
};
// ExoPlayer를 릴리즈한다.
private void destroyVideoPlayer() {
  playerView.setPlayer(null);
  if (player != null) {
    player.stop();
    player.release();
    player.removeListener(eventListener);
    player = null;
  }
}
// 영상 주소를 가지고 MediaSource를 생성한다.
private MediaSource getMediaSource() {
  VideoCacheManager vcm = App.getInstance().getVideoCacheManager();
  DataSource.Factory dataSourceFactory = new CacheDataSourceFactory(
    vcm.getSimpleCache(),
    vcm.getDefaultDataSourceFactory());
  ExtractorsFactory extractorsFactory = new DefaultExtractorsFactory();
  return new ExtractorMediaSource
    .Factory(dataSourceFactory)
    .setExtractorsFactory(extractorsFactory)
    .createMediaSource(Uri.parse(data.getMp4_url()));
}
...
..
.
```



# How to video caching or Pre-Caching

아래 클래스는 이번 여행관련 서비스 앱을 개발할 때 만든 클래스다. SimpleCache 객체는 싱글턴이어야 하므로 VideoCacheManager 객체를 싱글턴으로 유지하는게 좋다. 싱글턴으로 유지하기 위해 VoiceCacheManager를 커스텀 Applicatoin클래스에 생성하면 싱글턴으로 유지하기에 간편하다.

```java
public class VideoCacheManager {
  private final Context context;
  @Getter private DefaultDataSourceFactory defaultDataSourceFactory;
  @Getter private SimpleCache simpleCache;

  public VideoCacheManager(Context $context) {
    context = $context;
    simpleCache = new SimpleCache(getCacheDirFile(), new NoOpCacheEvictor());
    //simpleCache = new SimpleCache(getCacheDirFile(), evictor);
    String userAgent = Util.getUserAgent(context, context.getString(R.string.app_name));
    DefaultBandwidthMeter bandwidthMeter = new DefaultBandwidthMeter();
    defaultDataSourceFactory = new DefaultDataSourceFactory(
        context,
        bandwidthMeter,
        new DefaultHttpDataSourceFactory(userAgent, bandwidthMeter)
    );
  }

  private DataSource createDataSource() {
    return new CacheDataSource(
        simpleCache,
        defaultDataSourceFactory.createDataSource(),
        new FileDataSource(),
        new CacheDataSink(simpleCache, DEFAULT_MAX_CACHE_FILE_SIZE),
        CacheDataSource.FLAG_IGNORE_CACHE_ON_ERROR,
        new CacheDataSource.EventListener() {
          @Override public void onCachedBytesRead(long cacheSizeBytes, long cachedBytesRead) {
            Timber.d("onCachedBytesRead");
          }

          @Override public void onCacheIgnored(int reason) {
            Timber.d("onCacheIgnored");
          }
        }
    );
  }

  // 영상을 미리 캐시하고 싶을때 사용한다.
  public void cache(String url) {
    try {
      CacheUtil.cache(
          new DataSpec(Uri.parse(url)),
          simpleCache, createDataSource(),
          new CacheUtil.CachingCounters(),
          new AtomicBoolean());
    } catch (IOException e) {
      e.printStackTrace();
    } catch (InterruptedException e) {
      e.printStackTrace();
    }
  }

  private File getCacheDirFile() {
    return new File(context.getCacheDir(), "exo_cache");
  }
}
```

