# Android Architecture Components

Google IO 2017发布Android Architecture Components，当时的 Android Architecture Components组件还处于Alpha版本，不久前发布了Release版，Lifecycle也植入了v7包，


## Lifecycle

生命感知

```
public enum Event {

    ON_CREATE,

    ON_START,

    ON_RESUME,

    ON_PAUSE,

    ON_STOP,

    ON_DESTROY,

    ON_ANY
}
```  

```
public enum State {
    // [onDestroy, +)
    DESTROYED,
    
    // (+, onCreate)
    INITIALIZED,
    
    // [onCreate, onDestroy)
    CREATED,

	 // [onStart, onStop)
    STARTED,

	 // [onResume, onPause)
    RESUMED;
    
    public boolean isAtLeast(@NonNull State state) {
    	return compareTo(state) >= 0;
    }
}
```

```
public class Observer implements LifecycleObserver {
    @OnLifecycleEvent(Lifecycle.Event.ON_RESUME)
    public void onResume() {
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_PAUSE)
    public void onPause() {
    }
}
aLifecycleOwner.getLifecycle().addObserver(new Observer());
```


## LiveData
#### 释义
LiveData是一个用于持有数据并支持数据可被监听（观察）。和传统的观察者模式中的被观察者不一样，LiveData是一个生命周期感知组件，因此观察者可以指定某一个LifeCycle给LiveData，并对数据进行监听

```
public class AppLiveData extends LiveData {

    /**
     * Called when the number of active observers change to 1 from 0.
     */
    @Override
    protected void onActive() {
    }

    /**
     * Called when the number of active observers change from 1 to 0.
     */
    @Override
    protected void onInactive() {
    }

    @Override
    protected void setValue(Object value) {
        super.setValue(value);
    }

    @Override
    protected void postValue(Object value) {
        super.postValue(value);
    }
    
}
```

```
liveData.observe(owner, data -> {
		// update UI
	}
);
```

```
public class MutableLiveData<T> extends LiveData<T> {
    @Override
    public void postValue(T value) {
        super.postValue(value);
    }

    @Override
    public void setValue(T value) {
        super.setValue(value);
    }
}
```

#### LiveData的转换
* Transformations.map();

* Transformations.switchMap();

* 自定义转换 MediatorLiveData

#### 案例：
1、全局定位的封装  

2、关键词检索

LiveData 有以下优点：

* 减少内存泄漏：这是因为LiveData能够感知到组件的生命周期，当组件处于DESTROYED状态时，观察者对象会被清除掉。

* 当Activity停止时不会引起崩溃：这是因为组件处于非激活状态时，不会收到LiveData中数据变化的通知。

* 组件和数据相关的内容能实时更新：组件在前台的时候能够实时收到数据改变的通知，这是可以理解的。当组件从后台到前台来时，LiveData能够将最新的数据通知组件，这两点就保证了组件中和数据相关的内容能够实时更新。

* 针对configuration change时，不需要额外的处理来保存数据：我们知道，当你把数据存储在组件中时，当configuration change（比如语言、屏幕方向变化）时，组件会被recreate，然而系统并不能保证你的数据能够被恢复的。当我们采用LiveData保存数据时，因为数据和组件分离了。当组件被recreate，数据还是存在LiveData中，并不会被销毁。

* 资源共享：通过继承LiveData类，然后将该类定义成单例模式，在该类封装监听一些系统属性变化，然后通知LiveData的观察者，这个在继承LiveData中会看到具体的例子。

* 不需要额外的手动处理来响应生命周期的变化：fragment 只是在需要的时候观察数据，不用担心被停止或者在停止之后启动观察。由于 fragment 在观察数据时提供了其 Lifecycle，所以 LiveData 会自动管理这一切。


## ViewModel
#### 释义  
负责存储、管理UI组件(Fragment/Activity)相关的数据和UI组件间的通讯。  
在ViewModel中不要持有Activity的引用
#### 案例
1、Activity，它包含FragmentA和FragmentB，其中A是用户列表，B是用户的详细数据，点击列表上的某个用户，在B中显示相应的数据。

```
public class SharedViewModel extends ViewModel {
    private final MutableLiveData<Item> selected = new MutableLiveData<Item>();

    public void select(Item item) {
        selected.setValue(item);
    }

    public LiveData<Item> getSelected() {
        return selected;
    }
}

public class MasterFragment extends Fragment {
    private SharedViewModel model;
    public void onActivityCreated() {
        model = ViewModelProviders.of(getActivity()).get(SharedViewModel.class);
        itemSelector.setOnClickListener(item -> {
            model.select(item);
        });
    }
}

public class DetailFragment extends LifecycleFragment {
    public void onActivityCreated() {
        SharedViewModel model = ViewModelProviders.of(getActivity()).get(SharedViewModel.class);
        model.getSelected().observe(this, { item ->
           // update UI
        });
    }
}
```

ViewModel的好处如下： 

* Activity不需要做任何事情，不需要干涉这两个Fragment之间的通信。  
* Fragment不需要互相知道，即使一个消失不可见，另一个也能很好的工作。
* Fragment有自己的生命周期，它们之间互不干扰，即便你用一个FragmentC替代了B，FragmentA也能正常工作，没有任何问题。

