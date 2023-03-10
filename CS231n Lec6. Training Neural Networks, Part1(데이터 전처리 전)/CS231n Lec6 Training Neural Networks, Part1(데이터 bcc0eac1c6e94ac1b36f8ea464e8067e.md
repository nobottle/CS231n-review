# CS231n Lec6. Training Neural Networks, Part1(데이터 전처리 전)

활성화함수

![스크린샷 2023-03-14 오후 2.16.39.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.16.39.png)

활성화함수 영어로는 Activation Function은 f=Wx의 기본식을 가지고 있다 인풋값이 들어오면 이 값을 다음 노드로 보낼 때 값을 변형시켜서 보내준다 이유는 이 함수를 넣지않고 layer만 쌓아주면 그냥 처음 식 wx+b와 다름이 없다 (single layer와 다를게 없다) 즉 이 활성화함수를 통해 중요한 비선형성을 가해주게 된다

(input값은 linear한 값이 들어오지만 이 함수를 통해 non-linear한 값이 나오게 됨)

![스크린샷 2023-03-14 오후 2.20.56.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.20.56.png)

이거는 활성화함수들의 여러 종류들이다 이것들을 하나씩 살펴볼 예정

![스크린샷 2023-03-14 오후 2.21.45.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.21.45.png)

먼저 시그모이드 함수이다

최종출력단(output)에서만 사용하는 함수, 앞 노드에서 wx+b의 linear함수를 시그모이드 함수에 적용하면 0~1로의 값을 갖게 된다

hidden layer에서는 사용하지 않는다 그 이유를 하나씩 살펴보자

![스크린샷 2023-03-14 오후 2.24.24.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.24.24.png)

![스크린샷 2023-03-14 오후 2.24.39.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.24.39.png)

첫번 째 이유는 기울기가 소실되는 gradient vanishing문제가 있다

neural network는 backpropagation이라는 기법으로 강력한 모델을 만드는데 시그모이드 함수를 활성함수를 이용했을 때 backpropatation에 문제가 생긴다

왜…?

저 위 그래프를 봐보자 x가 -10일 경우 기울기는 거의 0인상태이다 또한 x값이  10일 때도 기울기는 거의 0이다 즉 값이 크거나 작으면 기울기는 0에 수렴한다는 뜻

x가 0일때는 0.25로 0에 가깝지는 않은데(0일때 gradien값이 제일 높다)(그런데도 (1-y)y 인 이 식에 대입해보면 이걸 계속 반복해도 0에 금방 수렴해서 기울기가 소실된다)

지난시간 local gradient와 global gradinet를 곱해서 w값을 구했는데 global gradient는 이전 노드의 기울기였다. back porpa과정은 이전의 기울기 값에 계속해서 local gradient값을 곱해주는 식인데, 결국에는 이 함수를 쓸 경우 global gradient가 거의 0이니 모두 0에수렴해서 기울기가 사라짐

즉>>w가 갱신,update가 불가능해진다…

![스크린샷 2023-03-14 오후 2.38.47.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.38.47.png)

![스크린샷 2023-03-14 오후 2.39.15.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_2.39.15.png)

두 번째 문제는 zero-center의 문제이다

시그모이드 함수 그래프를 보면 0이 중심이 아니다 즉 output값은 항상 양수

Neural Network는 층을 계속해서 쌓앗기 때문에 이전 노드의 output값이 다음의 입력값이 된다 즉 시그모이드 함수를 활성화함수로 사용하면 계속해서 양수의 값만 인풋이 되어버린다

또 입력값이 항상 양수일 경우 back prop과정을 진행했을 때 dl/df * df/dw로 w의 기울기를 구하는데 df/dwi는 xi다 그러면 입력갓을 항상 양수이므로 dl/df의 부호를 따라가게 되는데 즉 전수 양수이거나 음수가 되는것이다…!! 

즉 다시 정리하자면 dl/df가 음수면 음수,양수면 양수가 되는것 w의 gradient는 모두 양수이거나 음수이게 된다!!

![스크린샷 2023-03-14 오후 3.06.56.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.06.56.png)

그래서 이 그림을 보게되면 가중치가 빨간색 화살표처럼 지그재그로 가중치가 update가 되기 때문에 굉장히 비효율적이라는 거다(이상적인 방법은 대각선으로 쭉 가는거다 파란색처럼)

![스크린샷 2023-03-14 오후 3.08.32.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.08.32.png)

세번째로 exp의 연산은 힘이 많이 들어간다는 것 그냥 연산의 값이 매우 비싸다,연산이 매우 어렵다라는 거

1.vanishing gradient

2.zero-centered

3.exp연산

이 3가지 때문에 sigmoid룰 활성함로 잘 안쓴다는 것

![스크린샷 2023-03-14 오후 3.10.16.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.10.16.png)

그래서 나온 하이퍼볼릭 탄젠트 함수!

시그모이드와 형태가 유사하지만 도출 값이 1,-1로 zero-centered의 문제를 극복햇지만 시그모이드함수와 똑같이 기울시 소실문제,vanishing gradient의 문제를 해결하지 못해 활성화함수로는 부적격이다 또한 exp연산문제도 똑같이 문제가 있다

![스크린샷 2023-03-14 오후 3.11.39.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.11.39.png)

그 다음 나온것이 ReLu함수이다 이는 가장 대중적으로 많이 사용되는 activation function이다 이 함수는 0이하인 애들은 전부0,그 이상인 값들은 값 그대로 보내준다 식을 보면 이해할 수 있다

f(x)=max(0,x) 단순하지만 잘 동작한다 

그래프를 봤을 때 양의 방향 입력값x를 그대로 배출하기 때문에 기울기가 살아있다

즉 기울기 소실 문제를 어느정도 해결한 것,

또한 양의 영역에서 가중치 w의 update가 가능해짐

exp연산도 사라져 계산적으로도 효율적이다

2012 alex net에서 쓰이기 시작하면서 이 함수는 CNN과 큰 규모의 데이터에서 매우 잘 작동하였다 그러나 단점 역시 존재한다

음의 영역에서는 기울기가0이되는 zero-centered output의 문제

입력값의 음의 부분은 전부0으로 두기 때문에 vanishing gradient,zero-centered output이 여전이 문제이다

(zero-centeder는 지그재그로 가중치를 수정해저 효율성을 저해햇다)

한번 단점이 되는 이유와 과정을 살펴보자

![스크린샷 2023-03-14 오후 3.17.13.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.17.13.png)

![스크린샷 2023-03-14 오후 3.17.23.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.17.23.png)

이렇게 x가 -10 or 0이면 기울기는 소실.

x=10일 때만 기울기가 살아잇음

이를 dead ReLu라고 한다

이 경우 w initialization(가중치 초기화)에서 dead ReLu에 빠져버리면 업데이트가 안될 가능성이 잇고, learning rate를 크게 설정할 경우 w가 너무 크게 비약해서 학습이 안될 수 잇는 상황이 잇을수도 잇다

글을 읽다가 다른사람의 글을 인용한거

---

**이렇게 relu를 활성화함수로 한 모델을 훈련 도중에 network의 일부가 freeze 되는데,**

**이는 10~20% 정도가 dead relu에 빠진 것이라고 합니다.**

**이는 문제이긴 하지만 그럼에도 불구하고, train은 잘 된다고 합니다.**

**즉, 시그모이드에 비해 기울기 소실문제가 현저히 적어서 큰 문제를 일으키지 않고, 학습이 되기 때문에 relu가 상용화된 것이라고 보면 되겠습니다!**

---

이렇다고 한다

![스크린샷 2023-03-14 오후 3.20.41.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.20.41.png)

이거를 보면 약간의 bias값을 주어서 시작하려는 시도가 있었다

첨부터 dead ReLu에 빠뜨리지 않게 하기 위한 시도엿다고 보면 되겟다

그래서 active의 가능성을 높여주는 것인데 반반이라고 한다 사용하는 사람은 효과가 그저그런가봄

그래서 나온것이

![스크린샷 2023-03-14 오후 3.22.48.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.22.48.png)

Leaky ReLu함수이다 relu함수의 효율성을 가지면서, dead relu의 단점을 보완한 함수이다 보면 음의 영역에서도 경미하게 기울기를 주어 vanishing gradient문제점을 해결(0.01x의 값을 줘서 작은 값이라도 주게 하는 것)

그 다음 좀 변형한게 나온것이 위에 그림을 보면

parametric recifier(prelu)인데  뭐 a값을 더해 학습을 하는 그런거…

![스크린샷 2023-03-14 오후 3.27.40.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.27.40.png)

그 다음은 ELU(exponential Linear units)함수

elu는 0에서의 미분 불가점을 미분 가능하게 smooth시킨거

relu의 장점을 가지고 잇고 zero mean과 가까운 결과가 나오게 되는데

하지만 exp계산을 해야하는 단점을 가짐

이거를 leaky relu와 비교했을 때 노이즈에 대해서 더 로버스트 하다고 한다

뭐 relu함수의 장점을 가지고 잇어서 relu와 leaky함수사이의 함수라고 생각하면 된다

이 함수들은 각각의 장단점들이 잇어서 경험을 통해서 결정하면 될 듯

![스크린샷 2023-03-14 오후 3.30.57.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.30.57.png)

이 함수를 보면 파라미터를 더 두었다  max를 통해 다른 함수2개중에 max값을 취하는 함수 maxout은 기울기가 사라지는 문제점은 없지만 연산량이 두배가 되는 단점을 가지고 있다

![스크린샷 2023-03-14 오후 3.35.29.png](CS231n%20Lec6%20Training%20Neural%20Networks,%20Part1(%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%20bcc0eac1c6e94ac1b36f8ea464e8067e/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-14_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.35.29.png)

일반적으로 이 강의에서는 relu,lakey relu를 많이 사용한다고 하고 이거를 먼저 시도해보고 필요에 따라 다른것들을 사용하라고 함

sigmoid는 극혐이라고 한다