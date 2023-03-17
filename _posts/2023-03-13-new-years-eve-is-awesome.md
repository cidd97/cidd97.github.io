---
layout: post
title:  "Livedata의 Event wrapper란 무엇일까?"
---

>### Event Wrapper
Event 래퍼 클래스는 LiveData를 이용하여 중복된 데이터 처리 문제를 간단하게 해결할 수 있는 방법입니다.

LiveData는 데이터의 변경을 구독하다가, 구독 중인 화면이 비활성화 상태가 되면 다시 활성화되었을 때 마지막으로 변경된 값을 다시 받아옵니다. 이때, 이전에 발생한 이벤트에 대해서도 다시 받아오게 되는데, 이를 방지하기 위해 Event 래퍼 클래스를 사용합니다.

##Event Wrapper 코드

​```java

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

​```

##Event Observer 코드

​```java

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

​```
