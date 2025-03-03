# 分发顺序

> **Activity**->**ViewGroup**->**View**

## Activity

- **分发事件**：`Activity` 通过 `dispatchTouchEvent` 方法分发事件
- **消费事件**：如果根视图及其子视图没有消费事件，`Activity` 会调用自己的 `onTouchEvent` 方法处理事件。因此，`Activity` 具备兜底消费事件的能力。

## ViewGroup

- **分发事件**：`ViewGroup` 通过 `dispatchTouchEvent` 方法将事件分发给子视图
- **拦截事件**：`ViewGroup` 拥有 `onInterceptTouchEvent` 方法，可以决定是否拦截事件。拦截事件后，事件不会再传递给子视图，而是直接由 `ViewGroup` 自己处理。
- **消费事件**：当 `ViewGroup` 需要处理自己拦截的事件时，会调用其 `onTouchEvent` 方法来消费事件。

## View

- **消费事件**：`View` 只能消费事件，而没有分发和拦截事件的能力。当 `dispatchTouchEvent` 将事件传递给 `View` 时，`View` 只能选择在 `onTouchEvent` 中处理和消费该事件，或者将事件交回父视图。

> - `Activity`：负责整体的事件分发和兜底消费。
> - `ViewGroup`：负责在视图层级中分发事件，具备拦截和消费事件的灵活性。
> - `View`：仅具备事件消费能力。

# 关键方法

## 分发事件

 `dispatchTouchEvent`

- `dispatchTouchEvent(MotionEvent event)` 方法是事件分发的入口。
- 每当事件产生时（如点击、滑动），系统会将该事件封装成一个 `MotionEvent` 对象，并通过 `dispatchTouchEvent` 方法传递给根视图（通常是 `Activity` 中的 `DecorView`）。
- 在 `dispatchTouchEvent` 中，事件会根据层级逐层传递给子视图，直到找到可以处理事件的视图为止。
- 若 `dispatchTouchEvent` 返回 `true`，则事件处理停止；若返回 `false`，则事件会继续传递。

## 拦截事件 

**`onInterceptTouchEvent`**

- `onInterceptTouchEvent(MotionEvent event)` 主要用于 `ViewGroup` 及其子类。
- `ViewGroup` 可以选择是否拦截事件并防止它传递给子视图。
- 如果 `onInterceptTouchEvent` 返回 `true`，则事件会直接交由 `ViewGroup` 自己处理，子视图将无法接收到事件；如果返回 `false`，则事件会继续传递到子视图。

## 处理事件

 **`onTouchEvent`**

- `onTouchEvent(MotionEvent event)` 方法是实际处理事件的地方。

- 视图可以在该方法中根据 `MotionEvent` 的类型（如 `ACTION_DOWN`、`ACTION_MOVE`、`ACTION_UP` 等）进行具体的操作（如点击处理、滑动等）。

- 如果 `onTouchEvent` 返回 `true`，表示视图已处理该事件；如果返回 `false`，则事件会继续向上层视图传递，直到被某个视图消费或到达根视图为止。

  

# 源码分析

## Activity的事件分发

> **Activity 是事件分发的起点，负责接收系统传递过来的触摸事件并开始事件分发。**

```java
public boolean dispatchTouchEvent(MotionEvent ev) {
    if (ev.getAction() == MotionEvent.ACTION_DOWN) {
        onUserInteraction();
    }
    if (getWindow().superDispatchTouchEvent(ev)) {
        return true;
    }
    return onTouchEvent(ev);
}
```

1. `getWindow().superDispatchTouchEvent(ev)`

```java
if (getWindow().superDispatchTouchEvent(ev)) {
    return true;
}
```

```java
Activity#dispatchTouchEvent() ->
PhoneWindow#superDispatchTouchEvent() ->
DecorView#superDispatchTouchEvent() ->
ViewGroup#dispatchTouchEvent()
```

- `getWindow()`返回当前`Activity` 的`Window`对象（`Window`对象的唯一实现类是`PhoneWindow`类），调用其`superDispatchTouchEvent`方法来进一步分发事件
- `DecorView#superDispatchTouchEvent()` 方法内部会将事件传递给根视图（一般是 `DecorView`），并由该视图将事件沿视图层次分发下去，此方法调用父类`ViewGroup#dispatchTouchEvent()`

 `ViewGroup` 中`dispatchTouchEvent` 

- `ViewGroup.dispatchTouchEvent` 返回 `true` 表示事件已经在 `ViewGroup` 或其子视图中被消费，不再向上传递。
- `ViewGroup.dispatchTouchEvent` 返回 `false` 表示事件未被处理，最终会由 `Activity` 兜底处理。

2. `onTouchEvent(ev)`

```
return onTouchEvent(ev);
```

- 如果 `superDispatchTouchEvent(ev)` 返回 `false`，即所有的视图和组件都未处理该事件，`dispatchTouchEvent` 会将事件传递给 `Activity` 自身的 `onTouchEvent` 方法。
- `onTouchEvent` 是 `Activity` 处理事件的最后一步，通常用于处理默认的触摸行为。

![Activity事件分发](https://raw.githubusercontent.com/betteryuxuan/Image/main/90411b476d5c850e1e70f8657aeb49a2.png)

## ViewGroup的事件分发

> **ViewGroup决定是否拦截事件或传递给子 View。**

**2.1**

```java
public boolean dispatchTouchEvent(MotionEvent ev) {
    /*  按下的初始化
        完整事件从DOWN开始，所以说明这是一个新的事件序列，在此初始化
        resetTouchState内设置 mFirstTouchTarget = null，表示这是一个新的事件序列*/
    if (actionMasked == MotionEvent.ACTION_DOWN) {
        cancelAndClearTouchTargets(ev);
        resetTouchState();
    }

    /*  检查拦截情况
        mFirstTouchTarget是一个链表的头指针，用于管理当前正在处理触摸事件的子视图。
        mFirstTouchTarget = null时表示ViewGroup拦截了事件，没有子视图，链表为空
        (如果down拦截了，后续就都会交给ViewGroup处理)
        mFirstTouchTarget != null时表示不拦截
        所以这里第二个条件表示不拦截*/
    final boolean intercepted;
    if (actionMasked == MotionEvent.ACTION_DOWN
        || mFirstTouchTarget != null) {
        /*
            FLAG_DISALLOW_INTERCEPT
            通过子View的requestDisallowInterceptTouchEvent方法来设置
            表示不允许拦截ACTION_DOWN以外的事件（DOWN事件处理前会重置标志位）
            这里检查设置FLAG_DISALLOW_INTERCEPT则disallowIntercept为true
            intercepted设置为false表示不拦截
            */
        final boolean disallowIntercept = (mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0;
        if (!disallowIntercept) {
            intercepted = onInterceptTouchEvent(ev);
            ev.setAction(action);
        } else {
            intercepted = false;
        }
    } else {
        intercepted = true;
    }
    ...
}
```

* **`ViewGroup.onInterceptTouchEvent()`**

  **默认返回false**不拦截，需手动重写onInterceptTouchEvent()返回true来表示拦截

总结：当一个事件开始时（`ACTION_DOWN`），`mFirstTouchTarget`初始化为空，所以根据条件（`actionMasked == MotionEvent.ACTION_DOWN`）为真进入if中。如果 `FLAG_DISALLOW_INTERCEPT` 没有设置，`disallowIntercept`为`false`不拦截，交给`onInterceptTouchEvent`方法判断是否拦截（默认不拦截），`intercepted = false`；但如果拦截的话，后续的（`UP,MOVE`）满足条件（`mFirstTouchTarget != null`），`intercepted = true`，后续将不再调用`onInterceptTouchEvent`。

**2.2**

```java


public boolean dispatchTouchEvent(MotionEvent ev) {
    ...
        // 从后往前遍历子视图
        final View[] children = mChildren;
    for (int i = childrenCount - 1; i >= 0; i--) {
        final int childIndex = getAndVerifyPreorderedIndex(childrenCount, i, customOrder);
        final View child = getAndVerifyPreorderedView(preorderedList, children, childIndex);

        // 判断触摸点是否在子View的范围内或者子View在播放动画，两个条件均不符合就跳过
        if (!child.canReceivePointerEvents()
            || !isTransformedTouchPointInView(x, y, child, null)) {
            ev.setTargetAccessibilityFocus(false);
            continue;
        }

        // 获取当前子视图的触摸目标
        newTouchTarget = getTouchTarget(child);
        if (newTouchTarget != null) {
            newTouchTarget.pointerIdBits |= idBitsToAssign;
            break;
        }

        // 1 将触摸事件分发给当前子视图
        // 调用child的dispatchTouchEvent方法
        if (dispatchTransformedTouchEvent(ev, false, child, idBitsToAssign)) {
            mLastTouchDownTime = ev.getDownTime();
            if (preorderedList != null) {
                for (int j = 0; j < childrenCount; j++) {
                    if (children[childIndex] == mChildren[j]) {
                        mLastTouchDownIndex = j;
                        break;
                    }
                }
            } else {
                mLastTouchDownIndex = childIndex;
            }
            mLastTouchDownX = x; 
            mLastTouchDownY = y;
            // 当child处理了点击事件，那么mFirstTouchTarget会在addTouchTarget被赋值
            newTouchTarget = addTouchTarget(child, idBitsToAssign); 
            alreadyDispatchedToNewTouchTarget = true; 
            // 子View处理了事件，跳出循环
            break; 
        }
    }
}
```

1. **遍历子视图**：
   - 首先遍历 `ViewGroup` 的所有子视图，从后向前（为了优先处理最上层的视图）找到可以接收触摸事件的视图。

2. **判断是否能够接收点击事件**：
   - **动画状态**：如果子视图正在播放动画，该视图将被跳过。
   - **触摸坐标**：检查触摸事件的坐标是否落在子视图的区域内。通过计算触摸点与子视图边界的关系来判断。
   
3. **传递触摸事件**：
   - 如果找到一个满足条件的子视图，该视图将接收触摸事件。此时调用 `dispatchTransformedTouchEvent` 方法，实际上是调用了子视图的 `dispatchTouchEvent` 方法，将触摸事件传递给它进行处理。

![image-20241103204811747](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241103204811747.png)

```java
// 1处的方法
private boolean dispatchTransformedTouchEvent(MotionEvent event, boolean cancel,
        View child, int desiredPointerIdBits) {
    final boolean handled;
    final int oldAction = event.getAction();
    if (cancel || oldAction == MotionEvent.ACTION_CANCEL) {
        event.setAction(MotionEvent.ACTION_CANCEL);
        if (child == null) {
            handled = super.dispatchTouchEvent(event);
        } else {
            handled = child.dispatchTouchEvent(event);
        }
        event.setAction(oldAction);
        return handled;
    }
    // ...
    return handled;
}
```

## View

> **View 是事件分发的终点，负责响应用户的触摸操作。**

从ViewGroup事件分发最后可以看出，View事件分发从View的dispatchTouchEvent()开始

```java
public boolean dispatchTouchEvent(MotionEvent event) {  
    // ...
    if (onFilterTouchEventForSecurity(event)) {
        if ((mViewFlags & ENABLED_MASK) == ENABLED && handleScrollBarDragging(event)) {
            result = true;
        }
        /* 
            条件3：
            判断当前点击的控件是否enable
            条件124:
            onTouchListener不为null，并且onTouch方法返回true
            就表示事件被消费，不会再调用onTouchEvent
            可以看出onTouch优先级高于onTouchEvent  */
        ListenerInfo li = mListenerInfo;
        if (li != null && li.mOnTouchListener != null
            && (mViewFlags & ENABLED_MASK) == ENABLED
            && li.mOnTouchListener.onTouch(this, event)) {
            result = true;
        }
        
        //result为false调用自己的onTouchEvent方法处理
        if (!result && onTouchEvent(event)) {
            result = true;
        }
    }
    // ...
}
```

**onTouch方法：**

![image-20241106141105121](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241106141105121.png)

```java
button.setOnTouchListener(new OnTouchListener() {  
    @Override  
    public boolean onTouch(View v, MotionEvent event) {  
        return false;  
        // 若在onTouch()返回true，就会让上述三个条件全部成立，从而使得View.dispatchTouchEvent（）直接返回true，事件分发结束
        // 若在onTouch()返回false，就会使得上述三个条件不全部成立，从而使得View.dispatchTouchEvent（）中跳出If，执行onTouchEvent(event)
    }  
});
```

**onTouchEvent方法：**

```java
public boolean onTouchEvent(MotionEvent event) {
    final int action = event.getAction();

    /*  
        CLICKABLE和LONG_CLICKABLE任一存在，clickable为true
        设置setOnClickListenser或设置setOnLongClickListenser会自动设置对应的
    */
    final boolean clickable = ((viewFlags & CLICKABLE) == CLICKABLE
                               || (viewFlags & LONG_CLICKABLE) == LONG_CLICKABLE)
        || (viewFlags & CONTEXT_CLICKABLE) == CONTEXT_CLICKABLE;

    // 1.CLICKABLE 和LONG_CLICKABLE只要有一个为true就消费这个事件
    // 调用performClickInternal方法中的performClick方法
    if (clickable || (viewFlags & TOOLTIP) == TOOLTIP) {
        switch (action) {
            case MotionEvent.ACTION_UP:
                // ...
                // 检查之前是否按下了控件
                boolean prepressed = (mPrivateFlags & PFLAG_PREPRESSED) != 0;
                if ((mPrivateFlags & PFLAG_PRESSED) != 0 || prepressed) {

                    // 如果控件是可聚焦的并且不具有焦点，则尝试请求焦点。
                    boolean focusTaken = false;
                    if (isFocusable() && isFocusableInTouchMode() && !isFocused()) {
                        focusTaken = requestFocus();
                    }

                    if (prepressed) {
                        setPressed(true, x, y);
                    }

                    // 如果控件没有执行过长按事件，并且下一个 UP 事件不被忽略，则移除长按回调。
                    if (!mHasPerformedLongPress && !mIgnoreNextUpEvent) {
                        removeLongPressCallback();

                        /*
                        如果没有处理焦点，且没有执行过长按，则执行点击事件。
                        如果控件没有 mPerformClick 对象，创建一个并将其加入到消息队列中,
                        若失败则直接调用 performClickInternal() 执行点击操作
                        */
                        if (!focusTaken) {
                            if (mPerformClick == null) {
                                mPerformClick = new PerformClick();
                            }
                            if (!post(mPerformClick)) {
                                // 2.在ACTION_UP方法发生时调用performClick()方法
                                performClickInternal();
                            }
                        }
                    }
                    break;
                    // ...
                }
                return true;
        }
        return false;
    }
```

> **执行顺序**：**onTouchListener > onTouchEvent > onLongClickListener > onClickListener**

![image-20241106165931791](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241106165931791.png)

如果设置了点击事件，onClick方法就会被执行

> `onLongClickListener` 和 `onClickListener`的处理过程：当`ACTION_DOWN`到达一个view后，他会想通过handler post一个500ms消息（长按消息）给主线程，这个消息里面封装着`LongClickListener`的处理任务，如果在500ms之内收到了`ACTION_UP`事件，则首先会移除掉主线程消息队列中的长按消息，然后执行`onClickListener`对`Click`的执行逻辑。因为主线程中长按消息已经被移除，所以500ms后不会执行`LongClick`的任务。

![img](https://raw.githubusercontent.com/betteryuxuan/Image/main/00cf8d8e9a2926ce1bb31ecfac74b3a1.png)



# 总结

![img](https://raw.githubusercontent.com/betteryuxuan/Image/main/7baabcd3b1c96d3304cc544582b6404a.png)



---

---

==***感谢您的阅读
如有错误烦请指正***==

---

>  参考：
>
>  1. [Android 事件分发机制详解（上）_安卓事件分发流程-CSDN博客](https://blog.csdn.net/u010345983/article/details/135426633?ops_request_misc=%7B%22request%5Fid%22%3A%22312197EE-5B71-421A-8123-43C303AF423F%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fblog.%22%7D&request_id=312197EE-5B71-421A-8123-43C303AF423F&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_ecpm_v1~rank_v31_ecpm-1-135426633-null-null.nonecase&utm_term=分发&spm=1018.2226.3001.4450)
>  2. [Android事件分发机制-CSDN博客](https://blog.csdn.net/qq_41095045/article/details/122266940?ops_request_misc=%7B%22request%5Fid%22%3A%227b9488d41a0bf36f41c1aa07d47e1b46%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=7b9488d41a0bf36f41c1aa07d47e1b46&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-2-122266940-null-null.142^v100^pc_search_result_base1&utm_term=事件分发&spm=1018.2226.3001.4187)
>  3. 《Android进阶之光》

---

