# **RNN**
> RNN은 히든 노드가 방향을 가진 엣지로 연결돼 순환구조를 이루는 인공 신경망의 한 종류
- 시간적으로 상곤관계가 있는 데이터에 사용됨
- 문제점: 데이터의 시퀀스가 길어지면 가중치를 업데이터 하는 과정에서 1보다 작은 값들이 계속 곱해지면서 기울기가 사라지는 기울기 소실 문제가 발생한다.
  - 기울기 소실 문제: 신경망의 활성함수의 도함수값이 계속 곱해지다보면 가중치에 따른 결과값의 기울기가 0이 되어버리는 문제
- 과거 정보가 학습에 영향을 미치지 못하면서 긴 문장의 경우 적합하지 않다는 문제점이 있다.

# **LSTM (Long Short-Term Memory)**
> LSTM이란, 순환 신경망 (RNN) 기법의 하나로, 기울기 소실 문제를 해결하기 위해 고안된 딥 러닝 시스템
- LSTM은 망각 게이트를 추가로 건다. 이전 단계의 정보를 memory cell에 저장하여 흘려보낸다. 현재 시점의 정보를 바탕으로 과거의 내용을 얼마나 잊을지 곱해주고, 그 결과에 현재의 정보를 더해서 다음 시점으로 정보를 전달한다.
- 망각 게이트: 과거의 정보를 얼마나 잊을지 결정하는 게이트로, 현 시점의 정보와 과거의 은닉층의 값에 각각 가중치를 곱하여 더한 후 sigmoid 함수를 적용하여, 그 출력 값을 직전 시점의 cell에 곱해준다.
- sigmoid 함수는 0이라면 이전 상태의 정보를 잊고, 1이면 이전 상태의 정보를 온전히 기억한다.

# **Sequence-to-Sequence**
> Sequence-to-Sequence란, RNN을 기반으로 란 인코더와 디코더로 구성되어 특히 번역기에서 많이 사용되는 인공지능 모델
- 인코더는 입력 문장의 모든 단어들을 순차적으로 입력받은 뒤에 마지막에 이 모든 단어 정보들을 압축해서 하나의 벡터로 만드는데, 이를 컨텍스트 벡터(context vector)라고 한다. 입력 문장의 정보가 하나의 컨텍스트 벡터로 모두 압축되면 인코더는 컨텍스트 벡터를 디코더로 전송한다. 디코더는 컨텍스트 벡터를 받아서 번역된 단어를 한 개씩 순차적으로 출력한다.

# cf) Attention
- Attention: 인코더에서 전체 입력문장 중 예측 부분과 연관된 입력을 위주로 하여 디코더의 Time Step 마다 다시 참고한다. 아이템을 출력할 때마다 입력 시퀀스에서 어떤 아이템을 주목(Attention)해야하는 지를 가중치 벡터로 표시한다.
- Bahdanau Attetion: 컨텍스트 벡터와 임베딩된 벡터를 연결하는 방법로 가중치를 따로 계산하는 모델이 있다.

<img src="https://user-images.githubusercontent.com/45252618/199831577-8f02782e-7565-4f0e-b538-0376843ece3b.jpeg">

<br>
참고자료: https://ko.wikipedia.org/wiki/%EC%88%9C%ED%99%98_%EC%8B%A0%EA%B2%BD%EB%A7%9D, https://ko.wikipedia.org/wiki/%EA%B8%B0%EC%9A%B8%EA%B8%B0_%EC%86%8C%EB%A9%B8_%EB%AC%B8%EC%A0%9C, https://yjjo.tistory.com/17, https://ratsgo.github.io/natural%20language%20processing/2017/03/09/rnnlstm/, https://wikidocs.net/24996, https://blog.naver.com/sooftware/221784419691, https://yngie-c.github.io/nlp/2020/06/30/nlp_seq2seq/
