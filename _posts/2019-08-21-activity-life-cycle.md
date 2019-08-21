---
layout: post
title: "액티비티 생명주기"
categories: [Android]
tags: [Android]
---

# 액티비티 생명주기

![](https://developer.android.com/images/activity_lifecycle.png)

### 액티비티 시작

```
2019-08-21 23:44:37.479 6306-6306/... I/System.out: OJH A onCreate()
2019-08-21 23:44:37.484 6306-6306/... I/System.out: OJH A onStart()
2019-08-21 23:44:37.486 6306-6306/... I/System.out: OJH A onResume()
```

*시간을 보면 거의 동시에 들어온다는 것을 알 수 있다.*

### 액티비티 종료

```
2019-08-21 23:46:22.796 6306-6306/... I/System.out: OJH A onPause()

2019-08-21 23:46:23.452 6306-6306/... I/System.out: OJH A onStop()
2019-08-21 23:46:23.454 6306-6306/... I/System.out: OJH A onDestroy()
```

이유는 모르겠으나, *시간을 보면 onPause()와 onStop()사이의 시간차가 어느 정도 있는 것을 알 수 있다.*

# A액티비티와 B액티비티 사이의 생명주기

### A to B

A액티비티와 B액티비티 사이에 생명주기는 어떻게 될까? 다시말해, A 에서 B로 이동할 때 A의 생명주기와 B의 생명주기는 어떻게 나타날까?

```
2019-08-21 23:51:57.735 6306-6306/... I/System.out: OJH A onPause()

2019-08-21 23:51:57.806 6306-6306/... I/System.out: OJH B onCreate()
2019-08-21 23:51:57.814 6306-6306/... I/System.out: OJH B onStart()
2019-08-21 23:51:57.816 6306-6306/... I/System.out: OJH B onResume()

2019-08-21 23:51:58.496 6306-6306/... I/System.out: OJH A onStop()
```

A의 onPause()가 호출되고, B의 onCreate(), onStart(), onResume()이 빠르게 호출된 후 잠시 후 A의 onStop()이 호출되는 것을 확인할 수 있다.

### B to A(Back)

```
2019-08-22 00:05:11.038 6306-6306/... I/System.out: OJH B onPause()

2019-08-22 00:05:11.043 6306-6306/... I/System.out: OJH A onRestart()
2019-08-22 00:05:11.044 6306-6306/... I/System.out: OJH A onStart()
2019-08-22 00:05:11.044 6306-6306/... I/System.out: OJH A onResume()

2019-08-22 00:05:11.579 6306-6306/... I/System.out: OJH B onStop()
2019-08-22 00:05:11.579 6306-6306/... I/System.out: OJH B onDestroy()

```

최초, B의 onPause()가 호출된다. 

다음으로 빠르게 A의 onRestart(), onStart(), onResume()이 호출된다. 

약간의 시간차로 B의 onStop(), onDestroy()라 호출된다.

# AActivity에서 Dialog를 띄운다면?

Dialog는 Activity의 생명주기에 전혀 영향을 주지 않는다.

# AActivity에서 CActivity(Dialog) 사이의 생명주기

A에서 다이얼로그 테마로 설정한 C로 이동하면 A와 C의 생명주기는 각각 어떻게 될까? 

*C는 테마가 Dialog이기 때문에 C화면으로 이동해도, A의 화면이 반투명 처리된 배경에서 보이게 된다.*

```
2019-08-22 00:12:22.041 6306-6306/... I/System.out: OJH A onPause()

2019-08-22 00:12:22.069 6306-6306/... I/System.out: OJH C onCreate()
2019-08-22 00:12:22.076 6306-6306/... I/System.out: OJH C onStart()
2019-08-22 00:12:22.077 6306-6306/... I/System.out: OJH C onResume()
```

결과를 보자. A의 onPause()가 호출되고 onStop()이 호출되지 않는다. 곧이어, 빠르게 C의 onCreate() ~ onResume()이 호출된다. A의 onStop()이 호출되고 안되고는 A의 테마다 Dialog일때와 그렇지 않을 때 인것 같다.

```xml
    <activity
        android:name=".CActivity"
        android:screenOrientation="portrait"
        android:theme="@style/Theme.AppCompat.Dialog" />
```

그럼, Dialog 테마를 가진 모든 Activity를 호출하면 그런걸까? 그렇지 않다. 

```xml
    <activity
        android:name=".CActivity"
        android:screenOrientation="portrait"
        android:theme="@style/Theme.AppCompat.DialogWhenLarge" />
```

화면을 모두 가리는 Dialog 테마를 사용하면, A to B와 같이 동일한 콜백을 확인할 수 있다.

```
2019-08-21 23:51:57.735 6306-6306/... I/System.out: OJH A onPause()

2019-08-21 23:51:57.806 6306-6306/... I/System.out: OJH C onCreate()
2019-08-21 23:51:57.814 6306-6306/... I/System.out: OJH C onStart()
2019-08-21 23:51:57.816 6306-6306/... I/System.out: OJH C onResume()

2019-08-21 23:51:58.496 6306-6306/... I/System.out: OJH A onStop()
```

**A의 onStop()이 호출되거나 호출되지 않는것은 테마에 따라 다르다기보다, **

**A의 화면이 배경으로 보이느냐 보이지 않느냐의 차이다.**



### C to A(back)

```
2019-08-22 00:27:33.619 6845-6845/... I/System.out: OJH C onPause()

2019-08-22 00:27:33.636 6845-6845/... I/System.out: OJH A onResume()

2019-08-22 00:27:33.641 6845-6845/... I/System.out: OJH C onStop()
2019-08-22 00:27:33.643 6845-6845/... I/System.out: OJH C onDestroy()
```

A to B 에서 B to A 했을때랑 다르다. A의 onStop()이 불리지 않았으니 되돌아 올때 A의 onRestart(), onStart()가 호출되지  않았다. A의 onResume()만 호출됐다. 

액티비티 생명주기표에 *The activity is no longer visible.(액티비티가 더 이상 보이지 않습니다.)*가 증명된 셈이다.



# 결론

A to B에서 A가 onPause()까지만 호출된다면 A가 사용자에게 보이는 상태다. 보이는 상태라는 것은 UI를 신경써서 개발해야 한다는 말이 된다. 생명주기를 잘 알고 개발할 필요가 있다.