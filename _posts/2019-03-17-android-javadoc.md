---
title: "Javadoc 사용하기"
excerpt: "Javadoc이란 무엇이며 어떻게 사용할까?"
categories:
  - Programming
  - Android
tags:
  - javadoc
  - oracle
  - annotation
toc: true
toc_sticky: true
---

## JavaDoc이란 무엇인가?

JavaDoc은 **Java 코드에서 API 문서를 HTML 형식으로 생성해주는 도구**입니다.
HTML 형식이어서 관련 다른 API를 하이퍼 링크를 통해 접근이 가능하다는 장점이 있습니다.

Android 개발자라면 Android 개발자 사이트에서 자주 보았을 형식이며, 소스코드에 약간의 annotation만 추가하면 멋진 문서형식으로 정리되기 때문에 사용하는 습관을 들이길 권장합니다!

(Android는 당연하고 Kakao SDK([Link](https://developers.kakao.com/docs/android-reference/overview-summary.html))에서도 .css파일만 변경하여 JavaDoc으로 가이드 문서를 제공하고 있는 듯 합니다.)


## JavaDoc Tags

JavaDoc은 여러 Tag를 작성하여 문서를 완성합니다.

Tag는 **Java 소스코드에 annotation(주석)으로 추가**하며, Android Studio에서 '/**' 까지 입력 후 Enter를 치면 자동으로 기본 형식이 추가됩니다. 

Javadoc Tags 종류는 다음과 같습니다.

- @author
- @deprecated
- @exception
- @param
- @return
- @see
- @serial
- @serialData
- @serialField
- @since
- @throws
- @version



아래 내용은 위 태그를 직접 만든 Gallery Project([Link](https://github.com/ejiaah/android-gallery))에 적용한 예제입니다.

### Class 또는 Interface 

```java
/**
 * A class is the Manager that gets the album and photo list of the device.
 * For example:
 * <pre>
 *     GalleryManager manager = new GalleryManager(context, listener);
 *     galleryManager.getImageAlbums();
 *     galleryManager.getImagePhotos(album);
 * </pre>
 *
 * @author ejiaah
 * @version 1.0.0 19/03/13
 * @see com.ejiaah.gallery.Photo
 * @see com.ejiaah.gallery.Album
 */
class GalleryManager {
   ...
}
```

```java
/**
 * A Listener receives Album/Photo list asynchronously.
 */
public interface GalleryListener {
    ...
}
```

![alt]({{ site.url }}/assets/images/post/190317-android-javadoc/img-4.png)




### Field
```java
/**
 * Name of Album
 *
 * @see #getName()
 * @see #setName(String)
 */
private String name = "";

/**
 * Number of photos in Album
 *
 * @see #getCount()
 * @see #setCount(int)
 */
private int count = 0;
```
![alt]({{ site.url }}/assets/images/post/190317-android-javadoc/img-5.png)
![alt]({{ site.url }}/assets/images/post/190317-android-javadoc/img-6.png)




### Constructor and Method Tags

```java
/**
 * Get a list of photos from the Album.
 *
 * @param album
 * @since 1.0.0
 */
public void getImagePhotos(final Album album) {
    ...
}

/**
 * Test method. 테스트 함수
 *
 * @param stringNumber
 * @return isNumberForamt
 * @throws NullPointerException parameter is null or empty
 *
 * @since 1.0.0
 * @deprecated 1.0.1
 */
public boolean isNumberFormat(String stringNumber) {
    if(TextUtils.isEmpty(stringNumber)) {
        throw new NullPointerException();
    }

    try {
        int number = Integer.parseInt(stringNumber);
    } catch (NumberFormatException e) {
        e.printStackTrace();
        return false;
    }
    return true;
}
```
![alt]({{ site.url }}/assets/images/post/190317-android-javadoc/img-7.png)
![alt]({{ site.url }}/assets/images/post/190317-android-javadoc/img-8.png)



## JavaDoc 사용하기

### JavaDoc 생성하기

Android Studio 메뉴 'Tools-Generate JavaDoc...' 을 선택합니다.

![alt]({{ site.url }}/assets/images/post/190317-android-javadoc/img-1.png)

1. JavaDoc을 생성할 범위를 선택합니다. (ex. Whole project, Module 'app' ...)
2. JavaDoc을 생성할 Output directory 경로를 설정합니다.
3. JavaDoc에 포함시킬 접근 지정자를 선택합니다. (ex. private, package, protected, public ...)
4. 포함시킬 태그를 선택합니다. (ex. @use, @author, @version ...)
5. 한글 출력을 위해 Other command line arguments: 에 '-encoding UTF-8 -charset UTF-8' 을 추가하였습니다.
6. OK 버튼을 누릅니다.
7. 2에서 지정한 경로에 index.html 을 클릭하면 완성 된 JavaDoc을 확인할 수 있습니다.




### Android Studio에서 JavaDoc 확인하기

Android Studio에서 함수에 마우스를 가져다대면  JavaDoc을 바로 확인할 수 있는 설정이 있습니다.

'Files-Settings...' 를 선택합니다.
![alt]({{ site.url }}/assets/images/post/190317-android-javadoc/img-2.png)
'Editor-General' 에서 Show quick documentation on mouse move를 체크합니다.

설정하면 아래와 같이 Documentation을 확인할 수 있습니다. 
(예제는 Integer.parseInt에 마우스 포인터를 올렸을 때 입니다.)
![alt]({{ site.url }}/assets/images/post/190317-android-javadoc/img-3.png)



---

참조: [https://docs.oracle.com/javase/8/docs/technotes/tools/windows/javadoc.html](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/javadoc.html)

