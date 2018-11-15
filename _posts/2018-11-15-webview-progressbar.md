---
layout: post
title: "WebView ProgressBar 제어"
description: 
categories: [Android]
tags: [Android]
---

# ProgressBar 제어

```java
  private class MyWebViewClient extends WebViewClient {

    @Override public void onPageStarted(WebView view, String url, Bitmap favicon) {
      progressBar.show();
      super.onPageStarted(view, url, favicon);
    }

    @Override public void onPageFinished(WebView view, String url) {
      progressBar.hide();
      super.onPageFinished(view, url);
    }
  }

```

```java
  private class MyWebChromeClient extends WebChromeClient {
    @Override public void onProgressChanged(WebView view, int newProgress) {
      progressBar.setProgress(newProgress);
      super.onProgressChanged(view, newProgress);
    }
  }
```

## 로딩중 화면 종료할 때 ProgressBar 처리

[progressBar](https://developer.android.com/reference/android/support/v4/widget/ContentLoadingProgressBar){:target="_blank"}를 구현한 상태에서, 웹페이지를 불러오는 중에 화면을 종료하면 NPE가 발생한다. *progressBar.set(newProgress)* 부분이다. 화면 종료 후 View가 제거된 상태에서 View의 메서드를 참조하려했기 때문이다. View는 제거됐지만, [WebViewClient](https://developer.android.com/reference/android/webkit/WebViewClient){:target="_blank"}, [WebChromeClient](https://developer.android.com/reference/android/webkit/WebChromeClient){:target="_blank"}는 제거되지 않았다는 말이 된다.

NEP를 막는 방법은 두 가지다. progressBar를 null체크하여 사용하거나 [WebView의 destory](https://developer.android.com/reference/android/webkit/WebView#destroy()){:trget="_blank"}를 호출하는 방법이다.

null체크하는 방법은 지저분해 보인다. 화면이 종료된 후에도 WebViewClient, WebchromeClient가 남아 있다는 말이지 찝찝하기까지 하다. 때문에,

**destory()를 호출하는 방법을 권장한다.** onDestory를 호출하면 더 이상 WebViewClient나 WebChromeClient를 더이상 호출되지 않는다. 웹뷰 내부 상태들을 파괴하기 때문이다.

```java
@Override protected void onDestroy() {
	webView.destroy();
	super.onDestroy();
}
```

[StackOverFlow를 참고하자](https://stackoverflow.com/questions/17418503/destroy-webview-in-android){:target="_blank"}