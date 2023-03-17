---
layout: post
title:  "Livedata의 Event wrapper란 무엇일까?"
---

>### Event Wrapper
Event 래퍼 클래스는 LiveData를 이용하여 중복된 데이터 처리 문제를 간단하게 해결할 수 있는 방법입니다.

LiveData는 데이터의 변경을 구독하다가, 구독 중인 화면이 비활성화 상태가 되면 다시 활성화되었을 때 마지막으로 변경된 값을 다시 받아옵니다. 이때, 이전에 발생한 이벤트에 대해서도 다시 받아오게 되는데, 이를 방지하기 위해 Event 래퍼 클래스를 사용합니다.
