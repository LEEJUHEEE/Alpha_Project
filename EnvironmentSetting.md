# Reinforcement Learning Environment Setting

### Ego Car용 Simulink 모델
```
m = 1575;   % 총 차량 질량 (kg)
Iz = 2875;  % 관성 모멘트 (mNs^2)
lf = 1.2;   % 무게중심에서 앞 타이어까지의 종방향 길이 (m)
lr = 1.6;   % 무게중심에서 뒷 타이어까지의 종방향 길이 (m)
Cf = 19000; % 앞 타이어의 코너링 강성 (N/rad)
Cr = 33000; % 뒷 타이어의 코너링 강성(N/rad)
Vx = 15;    % 종방향 속도(m/s)
```
### 샘플 시간 Ts와 시뮬레이션 기간 T를 초 단위로 정의
```
Ts = 0.1;
T = 15;
```
### 시스템의 출력: ego car의 steering angle - 조향 범위를 -0.5~0.5로 설정
```
u_min = -0.5;
u_max = 0.5;
```
### 모델을 열어줌
```
%mdl = 'rlLKAMdl';
mdl = 'HighwayLaneChangeTestBench';
open_system(mdl);
agentblk = [mdl '/RL Agent'];
```
### 환경 인터페이스 생성
```
observationInfo = rlNumericSpec([6 1],'LowerLimit',-inf*ones(6,1),'UpperLimit',inf*ones(6,1));
observationInfo.Name = 'observations';
observationInfo.Description = 'information on lateral deviation and relative yaw angle';
actionInfo = rlFiniteSetSpec((-15:15)*pi/180);
actionInfo.Name = 'steering';
```
### 환경 인터페이스를 시뮬링크 모델에 생성
### 인터페이스에 -15도에서 15도까지 31가지 가능한 조향 각도
### 관찰: 측면 편차, yaw 각도, 시간에 대한 미분값, 적분값 등 6차원 벡터
```
env = rlSimulinkEnv(mdl,agentblk,observationInfo,actionInfo);
env.ResetFcn = @(in)localResetFcn(in);
```
### DQN 에이전트 생성
### 입력(6차원 벡터)와 31개 요소(-15도에서 15도 사이의 균일한 간격의 조향 각도)가 있는 심층 신경망 구성 
```
nI = observationInfo.Dimension(1);  % number of inputs (6)
nL = 24;                            % number of neurons
nO = numel(actionInfo.Elements);    % number of outputs (31)

dnn = [
    featureInputLayer(nI,'Normalization','none','Name','state')
    fullyConnectedLayer(nL,'Name','fc1')
    reluLayer('Name','relu1')
    fullyConnectedLayer(nL,'Name','fc2')
    reluLayer('Name','relu2')
    fullyConnectedLayer(nO,'Name','fc3')];
dnn = dlnetwork(dnn);
```
```
figure
plot(layerGraph(dnn))
```
### 샘플 시간 Ts와 시뮬레이션 기간 T를 초 단위로 정의
```
Ts = 0.1;
T = 15;
```
![image](https://user-images.githubusercontent.com/107983583/175027399-272655a4-2a98-4774-9794-8573a1982f5d.png)

```
criticOptions = rlOptimizerOptions('LearnRate',1e-4,'GradientThreshold',1,'L2RegularizationFactor',1e-4);
critic = rlVectorQValueFunction(dnn,observationInfo,actionInfo);
```
### Agent 옵션 지정
```
agentOptions = rlDQNAgentOptions(...
    'SampleTime',Ts,...
    'UseDoubleDQN',true,...
    'CriticOptimizerOptions',criticOptions,...
    'ExperienceBufferLength',1e6,...
    'MiniBatchSize',64);
```
```
agent = rlDQNAgent(critic,agentOptions);
```
### Train - 에피소드 보상에 도달하면 훈련 중지
```
maxepisodes = 5000;
maxsteps = ceil(T/Ts);
trainingOpts = rlTrainingOptions(...
    'MaxEpisodes',maxepisodes,...
    'MaxStepsPerEpisode',maxsteps,...
    'Verbose',false,...
    'Plots','training-progress',...
    'StopTrainingCriteria','EpisodeReward',...
    'StopTrainingValue',-1,...
    'SaveAgentCriteria','EpisodeReward',...
    'SaveAgentValue',-2.5);
```
```
maxepisodes = 5000;
maxsteps = ceil(T/Ts);
trainingOpts = rlTrainingOptions(...
    'MaxEpisodes',maxepisodes,...
    'MaxStepsPerEpisode',maxsteps,...
    'Verbose',false,...
    'Plots','training-progress',...
    'StopTrainingCriteria','EpisodeReward',...
    'StopTrainingValue',-1,...
    'SaveAgentCriteria','EpisodeReward',...
    'SaveAgentValue',-2.5);
 ```
