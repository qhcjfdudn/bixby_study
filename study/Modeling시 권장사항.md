# Modeling시 권장사항

1. 지원할 variation을 미리 생각해 볼 것

   내일 일출 시간 알려줘, 내일 일몰 시간 알려줘

   -> 일출 시간, 일몰 시간을 enum에 넣고 사용하면 될 것 같다.

   but, 실제 어떤 식으로 발화를 할까?

   내일 해 언제 떠? 언제 져?

   variation을 모두 다 vocab에 넣어 태깅하는 것은 어렵다.

   -> Sunrise, Sunset과 같은 goal을 만들고, 날짜만 태깅을 한 뒤 **동사는 별도의 태깅 없이** 여러 표현으로 학습하면 다양한 표현으로 학습 가능.

2. 가능한 태깅되지 않은 text만으로 다른 goal과 잘 구분되도록 모델링

   태깅한 부분을 x로 대치해서 생각해도 utterance가 구분이 잘 되는지 생각해 보아야 한다.

   ex) 고기 들어간 음식 추천해줘

   고기 태깅, 음식 태깅, 추천해줘 태깅을 하고 싶다? -> 태깅이 안 된 발화로는 goal 구분이 어렵고, vocab에만 전적으로 의존; 특히 vocab에 다양한 표현을 너헝ㅆ으니 학습 한 두개만 넣자! 이러면 안댐. vocab 단어들도 다양하게 골고루 학습해주어야 한다.

   -> 꼭 필요한 input만 태깅하고, 나머지는 text형태로 그대로 두어 학습시킬 수 있도록 한다.

3. 유사한 goal 통합

   TurnUpVolume, TurnDownVolume처럼 비슷한 것은 추후 확장성을 위해 모델링하는 것이 필요.

   -> IncreaseSettingValue goal, SettingType의 vocab의 형태라면,

   기존에 소리에 관련된 내용이 vocab에 있고 화면에 관한 내용을 추가한다면, 단순히 vocab에만 적용하면 끝. 이 경우 학습도 goal을 만들어서 더 추가하는 것보다 적게 만들어도 된다.

4. 꼭 필요한 input만 required 설정

   이 경우 자동으로 prompt dialog를 빅스비가 생성하지만, 다소 모호할 수 있다. 이를 위해 사용자가 이해할 수 있도록 dialog를 선언하고

   ```
   input-view {
   	match: stationName(target) {
   		to-input: getRoute
   	}
   	
   	message("어느 전철역까지 가시나요?")
   	...
   }
   ```

   추가적으로, 사용자가 텍스트로 입력한 게 아닌 발화로 입력받은 것을 처리하고 싶다면 prompt 발화를 학습한다.

   1. goal에 넣고 싶은 value를 넣고
   2. specification에 At prompt for를 선택
   3. speialization 노드에도 동일 컨셉을 입력
   4. 발화에 value 이름도 태깅을 해야 한다.
   5. 이 또한 variation도 고려!