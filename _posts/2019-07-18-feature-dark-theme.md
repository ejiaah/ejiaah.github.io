---
title: "Dark theme"
excerpt: "Android Q Dark Theme 미리보기"
categories:
  - Programming
  - Feature
tags:
  - Android Q
  - Dark theme
  - 다크모드
toc: true
toc_sticky: true
---

대부분의 개발자들이 에디터나 툴에서 선호하는 다크모드가 드디어 Android에서도 Q부터 공식적으로 지원하게 되었습니다! 

Android Q에서는 Bubble, Dark theme, Foldable 등 새로운 기능을  소개하고 있는데 가장 흥미있는 다크모드에 대해서 먼저 포스팅하려 합니다.

(이 포스트는 Android Q Beta5 기준으로 작성 및 테스트 되었습니다.)

## Dark theme

Android Q부터 시스템UI 및 실행중인 앱에서 Dark theme를 지원합니다.

**Drak theme의 이점**은 아래와 같습니다.

* 전력 사용량을 줄임
* 눈이 나쁘거나 빛에 민감한 사람들의 가시성 향상
* 어두운 환경에서 디바이스 사용이 쉬워짐



## Supporting Dark theme in your app
### Themes and styles

Dark theme를 지원하기 위해서는 `res/values/styles.xml`에 DayNight를 상속받은 theme를 적용하면 됩니다.

```java
<style name="AppTheme" parent="Theme.AppCompat.DayNight">
```

DayNight theme를 적용하면 시스템에서 다크모드 설정 시 하드코딩 된 색을 제외하고 자동으로 어두운 색으로 전환됩니다.

![alt]({{ site.url }}/assets/images/post/190718-feature-dark-theme/img-1.png){: width="40%"}
![alt]({{ site.url }}/assets/images/post/190718-feature-dark-theme/img-2.png){: width="40%"}

### Changing themes in-app

앱이 실행되는동안 다크모드 설정을 변경할 수 있습니다.

Android 9 이하의 기기에서 권장하는 테마는 아래와 같습니다.
* Light
* Dark
* Set by Battery Saver (the recommended default option)

Android Q 에서 권장하는 옵션은 다음과 같습니다.
* Light
* Dark
* System default (the recommended default option)

아래 코드를 통해 다크모드를 설정할 수 있습니다.
```java
AppCompatDelegate.setDefaultNightMode(mode); 
```

DayNight Style theme가 적용되어 있는 상태에서는 위의 코드 적용 시 사용자가 시스템에서 설정한 모드와 상관없이 아래와 같이 동작합니다.
* AppCompatDelegate.MODE_NIGHT_YES : 어두운 테마

* AppCompatDelegate.MODE_NIGHT_NO : 밝은 테마

* AppCompatDelegate.MODE_NIGHT_AUTO_BATTERY : 배터리 절전 상태에 따라 밝은/어두운 테마

* AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM : 유저가 설정한 시스템 모드에 따라 밝은/어두운 테마

  


## Force Dark
Android Q에서 제공하는 Force Dark 기능을 이용하면 빠르게 다크모드를 지원할 수 있습니다.

Force Dark는 Light theme가 적용된 뷰를 분석하여 자동으로 어두운 테마로 적용합니다. 

**Theme.Material 혹은 Theme.DayNight 를 상속받은 경우 Force Dark 기능이 동작하지 않습니다.**

Force Dark 기능은 아래 코드를 삽입하면 되며, 뷰마다 설정이 가능합니다.
```java
android:forceDarkAllowed="true"
```
![alt]({{ site.url }}/assets/images/post/190718-feature-dark-theme/img-3.png){: width="40%"}
![alt]({{ site.url }}/assets/images/post/190718-feature-dark-theme/img-4.png){: width="40%"}

위 이미지는 Light Theme로 만들어져 있으며 배경과 글자색을 하드코딩 지정하였습니다.
사용자가 시스템 다크모드를 설정하여도 색이 변하지 않으나 Force Dark 기능을 적용하여 자동으로 다크모드로 색이 변환된 것을 확인할 수 있습니다.

## Best Practices

### Notifications and Widgets

기기에 표시되지만 직접 제어되지 않는 UI 의 경우 사용하는 모든 뷰의 host app theme가 반영되었는지 확인하는 것이 중요합니다. (ex. Notifications , launcher widgets)

### Luanch screens

앱에 custom launch screen 이 있는 경우 선택한 theme가 반영되도록 수정해야 할 수 있습니다.

하드 코딩된 색상을 제거해야 합니다. `?android:attr/colorBackground` 테마 속성을 사용합니다.

dark-tehme로 설정된 `android:windowBackground` drawables은 Android Q에서만 동작합니다.

### Configuration changes

앱의 테마가 변경되면 `uiMode` configuration이 trigger 됩니다. 

시스템 다크모드 설정 변경 시 Activity는 자동으로 recreate 됩니다.

이를 막기위해서는 AndroidManifest.xml에 `android:configChanges="uiMode"` 추가하면 Activity가 recreate 되는 것을 막고 `onConfigurationChanged()` 통해 변경 된 시스템 다크모드를 알 수 있습니다.

```java
val currentNightMode = configuration.uiMode and Configuration.UI_MODE_NIGHT_MASK
    when (currentNightMode) {
        Configuration.UI_MODE_NIGHT_NO -> {} // Night mode is not active, we're using the light theme
        Configuration.UI_MODE_NIGHT_YES -> {} // Night mode is active, we're using dark theme
    }
```

---

참조: https://developer.android.com/preview/features/darktheme

