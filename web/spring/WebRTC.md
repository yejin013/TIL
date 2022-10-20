# WebRTC

> WebRTC (Web Real-Time Communication) 란, 웹 브라우저 간에 플러그인의 도움 없이 서로 통신할 수 있도록 설계된 API

- 화상통화, 화상 공유 등을 구현할 수 있는 오픈소스
- 비디오, 음성 및 일반 데이터가 P2P 방식으로 피어간의 전송되도록 지원
- JavaScript API로 제공 

## 장점
- 오픈 소스 프로젝트
- 지속적인 발전과 개선 : 계속 버전 릴리즈가 되고 있다.
- P2P 통신 단순화
- 낮은 대역폭 소비 및 대기 시간 : Latency가 짧아 거의 지연시간 없는 REAL-TIME과 비슷한 방송이 가능하다.
- 보안 데이터 전송 : 암호화 되어 전송된다.
- 여러 장치에서 사용 가능
- 따로 프로그램 설치 필요 없음
- 운영체제에 부담을 주지 않음 

## 단점
- 크로스 브라우징 문제 : 사파리와 특정 브라우저, 예전 버전의 브라우저의 경우 사용이 불가능하다.
- STUN/TURN 서버 필요 : 방화벽 문제. 다른 네트워크 상에서는 이 서버가 필요하다.

## 사용 API 
### MediaDevices.getUserMedia()
```javascript
async function getMedia(constraints) {
  let stream = null;

  try {
    stream = await navigator.mediaDevices.getUserMedia(constraints);
    /* use the stream */
  } catch (err) {
    /* handle the error */
  }
}
```

### RTCPeer 연결
로컬 컴퓨터와 원격 피어 간의 WebRTC 연결 
```javascript
// 호출 측
async function makeCall() {
    const configuration = {'iceServers': [{'urls': 'stun:stun.l.google.com:19302'}]}
    const peerConnection = new RTCPeerConnection(configuration);
    signalingChannel.addEventListener('message', async message => {
        if (message.answer) {
            const remoteDesc = new RTCSessionDescription(message.answer);
            await peerConnection.setRemoteDescription(remoteDesc);
        }
    });
    const offer = await peerConnection.createOffer();
    await peerConnection.setLocalDescription(offer);
    signalingChannel.send({'offer': offer});
}
```
```javascript
// 수신 측
const peerConnection = new RTCPeerConnection(configuration);
signalingChannel.addEventListener('message', async message => {
    if (message.offer) {
        peerConnection.setRemoteDescription(new RTCSessionDescription(message.offer));
        const answer = await peerConnection.createAnswer();
        await peerConnection.setLocalDescription(answer);
        signalingChannel.send({'answer': answer});
    }
});
```

### RTC 데이터 채널 
RTCPeerConnection를 통해 임의의 데이터를 전송하기 위한 API
```javascript
const peerConnection = new RTCPeerConnection(configuration);
const dataChannel = peerConnection.createDataChannel();

const peerConnection = new RTCPeerConnection(configuration);
peerConnection.addEventListener('datachannel', event => {
    const dataChannel = event.channel;
});
```
메시지 송수신 
```javascript
// 송신
const messageBox = document.querySelector('#messageBox');
const sendButton = document.querySelector('#sendButton');

// Send a simple text message when we click the button
sendButton.addEventListener('click', event => {
    const message = messageBox.textContent;
    dataChannel.send(message);
})

// 수신
const incomingMessages = document.querySelector('#incomingMessages');

const peerConnection = new RTCPeerConnection(configuration);
const dataChannel = peerConnection.createDataChannel();

// Append new messages to the box of incoming messages
dataChannel.addEventListener('message', event => {
    const message = event.data;
    incomingMessages.textContent += message + '\n';
});
```

참고자료 <br>
https://webrtc.org/

