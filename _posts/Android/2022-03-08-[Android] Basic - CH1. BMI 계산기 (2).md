---
layout: single
title: "[Android] Basic - CH1. BMI 계산기 (2)" 
categories: ['Android']
tag: ['Android', 'Project', 'BMI 계산기']
toc: true
toc_sticky: true
---





## 프로젝트 만들기

만약 프로젝트를 진행하시지 않은 상태라면 안드로이드 스튜디오를 실행 시켰을 때 아래와 같은 화면이 나오게 됩니다.

"Create New Project"를 선택하여 프로젝트를 만들어 봅시다.

![image](https://user-images.githubusercontent.com/79521972/157248310-15ca9622-428f-4f5f-adb9-06b98866dfd2.png)

만약 진행중이던 프로젝트가 있었다면 File->Close Project를 눌러 첫 번째 사진으로 갈 수 있습니다.

![image](https://user-images.githubusercontent.com/79521972/157249810-289a6d70-4ca7-4081-a7da-88ad05b5c635.png)

<br>

그리고 Empty Activity(아무 것도 없는 화면, 하지만 Hello, World라는 화면이 뜰 것임)을 클릭하여 줍니다.

![image](https://user-images.githubusercontent.com/79521972/157250118-860f9c84-a09b-4f2a-9ab9-5f55b510703f.png)

<br>

여러 프로젝트를 생성할 것이기 때문에 적당한 이름(저는 `aop-part2-chapter01`로 하였습니다.)으로 지어주고 package name 또한 잘 지어줍시다.

언어는 kotlin으로 해 주고 Minimum SDK는 아래와 같은 API23: Android 6.0 (Marshmallow) 버전으로 해 줍시다. 

![image](https://user-images.githubusercontent.com/79521972/157250333-66be3127-79f1-45dd-b364-929c18e1d3f8.png)

<br>

## 가상 폰 만들기

내가 만든 앱이 잘 돌아가는 지 확인하기 위한 가상의 폰(혹은 본인 핸드폰을 연결하여 사용하셔도 됩니다.)을 만들어 볼 겁니다.

![image](https://user-images.githubusercontent.com/79521972/157251766-f363cec9-dd4b-4eca-ae7d-8c7612f3b5f2.png)

다음 커서가 가리키는 곳(AVD Manager)을 클릭하면 나오는 창의 아래 부분에 "Create Virtual Device"라는 버튼을 클릭해 주고 여러 모바일 기기들 중에서 Pixel 2 (API 30 버전) 기기를 만들어 줍시다.

## Layout

이제 앱의 Layout을 만들도록 하겠습니다.

 ![image](https://user-images.githubusercontent.com/79521972/157251293-405414fe-b21f-4ece-a656-e9926ca031e5.png)

layout의 파일은 왼쪽 디렉토리에서 res > layout > activity.xml 에 작성합니다.

<br>

쓸데 없는 Hello, World! 문구가 들어가 있는데 이는 클릭하고 delete 버튼을 눌러서 삭제해 주도록 합시다. 그러면 흰 배경만 남아있는 상태가 될 것입니다. 

앞으로 이 activity 파일에서는 코드를 작성하면서 디자인이 어떻게 바뀌는 지 볼 수 있는 Split 형식을 사용할 것입니다.

![image](https://user-images.githubusercontent.com/79521972/157253270-a5822bc7-53e8-4c1b-b23a-a0443dfe2709.png)

<br>

초기 main activity 코드는 다음과 같이 작성 되어 있습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"/>
```

우리는 LinearLayout이라는 레이아웃을 사용할 것이기 때문에 위 코드를 아래와 같이 바꾸어 줍시다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
</LinearLayout>
```



















