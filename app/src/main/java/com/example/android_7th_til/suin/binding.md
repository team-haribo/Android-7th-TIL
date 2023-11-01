viewbinding
----------------
- view와 상호작용하는 코드를 쉽게 작성할 수 있음
- 모듈에서 설정된 뷰 결합은 XML 레이아웃 파일의 결합 클래스 생성
- ( 장점 ) 데이터 바인딩보다 용량 절약
#### 사용법
##### 1. [build.gradle]파일에 밑 코드를 복붙한다.
```
android {
        buildFeatures {
            viewBinding = true
        }
    }
```
##### 바인딩 파일 무시할려면 밑 코드를 레이아웃에 추가한다.
```xml
<LinearLayout
    tools:viewBindingIgnore="true"
</LinearLayout>
```
##### 2. [Kotlin] Activity에서 binding 선언
```kt
class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
    }
}
```
##### 3. [kotlin] binding 사용
```kt
    binding.id.text = "뷰바인딩이에용"
```

databinding
----------------
- 데이터와 뷰 연결 작업을 레이아웃 즉 xml파일에서만 처리함
- ( 장점 ) 뷰 바인딩 기능 + 양방향 데이터 바인딩, 동적 UI

#### 사용법
##### 1. [build.gradle]파일에 밑 코드를 복붙한다.
```
android {
    buildFeatures {
        dataBinding = true
    }
}
```
##### 2. [kotlin] Activity에서 binding 선언
```kt
class MainActivity : AppCompatActivity() {
 
    private lateinit var binding: ActivityMainBinding
 
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)
 
        binding.user = UserProfile("suin", "han")
    }
}
```
##### 3. [xml] 레이아웃에 데이터 태그, 뷰 정의
```xml
<data>
        <variable
            name=""
            type="" />
    </data>
``` 
- type : 저장된 데이터 경로
- name : 사용될 이름
```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools">

    <data>

        <variable
            name="user"
            type="com.example.emh.UserProfile" /> 
    </data>


    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{user.firstName}" />

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@{user.lastName}" />
    </LinearLayout>
</layout>
```
- 변수 사용시 @{} 안에 넣기
