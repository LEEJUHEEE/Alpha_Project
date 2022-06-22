# Alpha_Project
https://github.com/LEEJUHEEE/Alpha_Project
## 프로젝트 개요
#### * Matlab Simulink에서 예제를 활용하여 Reinforcement Learning을 활용한 Lane Change 모델을 구현
#### * 기존 알고리즘을 통해 차선 변경 하던 모델을 강화학습 block으로 대체   
&nbsp; 
#### * Highway Lane Change Test Bench 강화학습 변경 모델
![image](https://user-images.githubusercontent.com/107983583/175031344-a903d2e2-2810-4854-9c79-b88c3c41030e.png)
#### 기존 모델에서 Steering Angle을 조절하는 부분을 강화학습으로 대체 
&nbsp; 
## 실행 순서
## 0. Test Bench download

```
openExample('autonomous_control/HighwayLaneChangeExample')
```
#### 위와 같은 명령어를 Matlab 명령창에 실행하면 Example이 실행됨
&nbsp; 
## 1. Simulink file
https://github.com/LEEJUHEEE/Alpha_Project/blob/main/HighwayLaneChangeTestBench.slx
#### 파일을 다운로드 후 아래와 같은 경로에 파일을 넣어줌
#### HighwayLaneChangeExample\HighwayLaneChange\HighwayLaneChange\TestBench\HighwayLaneChangeTestBench.slx
&nbsp; 
## 환경 설정
(https://github.com/LEEJUHEEE/Alpha_Project/blob/main/EnvironmentSetting.md)

#### 환경 설정 후 Simulink를 실행시키면 강화학습 모델 학습
&nbsp; 
## 실행 결과

![image](https://user-images.githubusercontent.com/107983583/175028304-33fcfe14-88c8-41db-a410-960b013c1adc.png)
![image](https://user-images.githubusercontent.com/107983583/175028375-8a0950e5-5203-4b59-a5fb-d38481fd7a78.png)

##### 현재는 약 22번 훈련된 후 매트랩이 강제 종료 —> 매트랩에 문의
&nbsp; 
## 참고한 사이트 인용
#### MATLAB 및 Simulink에서의 Reinforcment learning 환경 모델링
https://kr.mathworks.com/solutions/deep-learning/deep-reinforcement-learning.html

#### 차선 유지 보조를 위한 DQN 에이전트 훈련
https://kr.mathworks.com/help/reinforcement-learning/ug/train-dqn-agent-for-lane-keeping-assist.html

#### Highway Lane Change
https://kr.mathworks.com/help/driving/ug/highway-lane-change.html
