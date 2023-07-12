device orientation은 HTML5가 제공하는 매우 유용한 기능으로 중력과의 관계에서 디바이스의 물리적 방향의 변화를 감지할 수 있음. 이것을 이용하면 모바일 디바이스를 회전시켰을 때 이벤트를 감지하여 적절히 화면을 변화시킬 수 있음

디바이스 방향 정보를 다루는 자바스크립트 이벤트는 두 가지가 있음

- DeviceOrientationEvent : 가속도계가 기기의 방향의 변화를 감지했을 때 발생함
- DeviceMotionEvent : 가속도에 변화가 일어났을 때 발생함

**브라우저 별 지원 정보는 caniuse 참조하기(사파리 제외 거의 사용 가능)**

오래된 브라우저를 사용하는 사용자를 위해 브라우저 이벤트 지원 여부를 먼저 확인할 필요가 있음

```
if (window.DeviceOrientationEvent) {
  // Our browser supports DeviceOrientation
} else {
  console.log('Sorry, your browser doesn't support Device Orientation');
}
```

## DeviceOrientationEvent

디바이스 뱡향 변화는 3개의 각도(alpha, beta, gamma)를 사용해 측정됨. DeviceOrientationEvent 이벤트에 리스너를 등록하면 리스너 함수가 주기적으로 호출되어 업데이트된 방향 데이터를 제공함. DeviceOrientationEvent 이벤트는 다음 4가지 값을 가짐

- DeviceOrientationEvent.absolute
- DeviceOrientationEvent.alpha
- DeviceOrientationEvent.beta
- DeviceOrientationEvent.gamma

```
window.addEventListener('deviceorientation', handleOrientation, false);

function handleOrientation(event) {
  var absolute = event.absolute;
  var alpha    = event.alpha;
  var beta     = event.beta;
  var gamma    = event.gamma;

  // Do stuff with the new orientation data
}
```

<div align="center">
<img src="https://poiemaweb.com/img/deviceorientation-angles.png" width="50%" height="50%">
</div>

### absolute

지구 좌표계를 사용하는 지에 대한 boolean 값임. 일반적인 경우는 사용하지 않음

<div align="center">
<img src="hhttps://poiemaweb.com/img/deviceorientation-alpha.png" width="50%" height="50%">
</div>

### alpha

0도부터 360도까지 범위의 z축을 중심으로 디바이스 움직임을 나타냄

<div align="center">
<img src="https://poiemaweb.com/img/deviceorientation-beta.png" width="50%" height="50%">
</div>

### beta

-180도부터 180도(모바일 사파리: -90 ~ 90도)까지의 범위의 x축을 중심으로 디바이스 움직임을 나타냄. 이는 디바이스의 앞뒤 움직임을 나타냄

<div align="center">
<img src="https://poiemaweb.com/img/deviceorientation-gamma.png" width="50%" height="50%">
</div>

### gamma

-90도부터 90도(모바일 사파리: -180 ~ 180도)까지의 범위의 x축을 중심으로 디바이스 움직임을 나타냄. 이는 디바이스의 좌우 움직임을 나타냄
