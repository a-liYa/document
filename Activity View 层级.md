Activity window view hierarchy

> extends Activitiy

```
.
|____DecorView
| |____LinearLayout
| | |____ViewStub{android:id/action_mode_bar_stub}
| | |____FrameLayout{android:id/content}
| | | |____根布局
| |____View{android:id/navigationBarBackground}（虚拟导航）
| |____View{android:id/statusBarBackground}
```


> extends AppCompatActivity

```
.
|____DecorView
| |____LinearLayout
| | |____ViewStub{android:id/action_mode_bar_stub}
| | |____FrameLayout
| | | |____FitWindowsLinearLayout{app:id/action_bar_root}
| | | | |____ViewStubCompat{app:id/action_mode_bar_stub}
| | | | |____ContentFrameLayout{android:id/content}
| | | | | |____根布局
| |____View{android:id/navigationBarBackground}（虚拟导航）
| |____View{android:id/statusBarBackground}
```

windowBackground被设置在DecorView上。