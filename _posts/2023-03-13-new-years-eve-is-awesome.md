---
layout: post
title:  "Livedata의 Event wrapper란 무엇일까?"
---

>### Event Wrapper
Event 래퍼 클래스는 LiveData를 이용하여 중복된 데이터 처리 문제를 간단하게 해결할 수 있는 방법입니다.

LiveData를 사용할 때, 비활성화된 화면에서 발생한 이벤트가 활성화된 후에 다시 받아와서 중복 처리 문제가 발생할 수 있습니다. 이를 해결하기 위한 방법 중 하나가 Event Wrapper 클래스입니다. Event Wrapper 클래스를 사용하면 이전에 발생한 이벤트를 처리하지 않도록 하여 중복 처리 문제를 간단하게 해결할 수 있습니다.

#### 아래는 Event Wrapper 클래스와 이를 사용하는 Observer 클래스의 코드 예시입니다.
> ### Event Wrapper
```java
public class Event<T> {

    private final T content;

    private boolean hasBeenHandled = false;

    public Event(T content) {
        this.content = content;
    }

    public T getContentIfNotHandled() {
        if (hasBeenHandled) {
            return null;
        } else {
            hasBeenHandled = true;
            return content;
        }
    }

    public T peekContent() {
        return content;
    }
}
```
<br>



<br>

> ### Event Observer 코드
```java
public class EventObserver<T> implements Observer<Event<? extends T>> {

    public interface EventUnhandledContent<T> {
        void onEventUnhandledContent(T t);
    }

    private final EventUnhandledContent<T> content;

    public EventObserver(EventUnhandledContent<T> content) {
        this.content = content;
    }

    @Override
    public void onChanged(Event<? extends T> event) {
        if (event != null) {
            T result = event.getContentIfNotHandled();
            if (result != null && content != null) {
                content.onEventUnhandledContent(result);
            }
        }
    }
}
```
