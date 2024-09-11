# Web Audio API
---
## AudioContext

AudioContext 인터페이스는 오디오 API의 중심 객체로 오디오 처리를 위한 환경을 제공한다. 브라우저에서 오디오 모듈로부터 만들어진 오디오 프로세싱 그래프를 구성하고, 생성된 오디오를 처리하며 조작도 할 수 있다.

## MediaDevices

MediaDevices 인터페이스는 카메라, 마이크, 공유 화면 등 현재 연결된 미디어 입력장치로의 접근 방법을 제공하는 Web API중 하나이다.
사용자 녹음 등 입력 장치에 접근하고 제어할 수 있는 메서드들을 정의하고 제공한다.

```jsx
const handleRecord = async () => {
	const { mediaDevices } = navigator;
	const stream = await mediaDevices.getUserMedia({ audio: true });
```

AudioContext와 차이점은 MediaDevices는 오디오의 상세한 처리는 불가능하며 미디어 장치 접근이 주요 목적이고 AudioContext는 오디오 자체의 처리 및 생성이 주요 목적이다.
따라서 MediaDevices는 audio 파일을 저장하는 형식에 있어서도 audio/webm등으로 제한이 있고 audio/wav등 다양한 파일로 처리를 하려면 AudioContext를 사용해야 한다.