<!-- Template for PROJECT REPORT of CapstoneDesign 2025-2H, initially written by khyoo -->
<!-- 본 파일은 2025년도 컴공 졸업프로젝트의 <1차보고서> 작성을 위한 기본 양식입니다. -->
<!-- 아래에 "*"..."*" 표시는 italic체로 출력하기 위해서 사용한 것입니다. -->
<!-- "내용"에 해당하는 부분을 지우고, 여러분 과제의 내용을 작성해 주세요. -->

# Team-Info
| (1) 과제명 | 위성인터넷 해킹을 통한 프라이버시 유출 방지를 위한 Fingerprinting 기법을 활용한 Starlink 네트워크 취약점 분석
|:---  |---  |
| (2) 팀 번호 / 팀 이름 | 16-RexT |
| (3) 팀 구성원 | 곽현정(2171003): 팀장, Llama/Triplet기반 모델 학습, 계획발표 <br> 강호성(2171002): 팀원, LaserBeak 기반 모델 학습, 하이퍼 파라미터 튜닝, 중간 발표 <br> 홍지우(2171053): 팀원, DF 기반 모델 학습, 데이터 전처리/분석, 기말발표			 |
| (4) 팀 지도교수 | 오세은 교수님 |
| (5) 과제 분류 | 연구 과제 |
| (6) 과제 키워드 | Starlink Network, AI for security, Cyber Security, Website Fingerprinting  |
| (7) 과제 내용 요약 | **연구 목적**<br> 본 과제는 Starlink와 같은 저궤도(LEO) 기반 위성 인터넷 환경에서도 Website Fingerprinting(WF) 공격이 가능한지를 실증적으로 분석하고, 해당 환경에 특화된 공격 기법을 제안하는 것을 핵심 목표로 하는 연구이다.<br><br> **WF 공격의 개요**<br> NAT(Network Address Translation)와 패킷 암호화로 인해 개별 패킷 내용을 복호화하여 분석하는 것은 사실상 불가능에 가깝지만, 패킷의 크기, 전송 간격, 방향 등 메타데이터를 활용하면 암호화된 트래픽에서도 사용자의 웹사이트 방문 패턴을 추론할 수 있다. 이러한 방식의 공격을 Website Fingerprinting(WF)이라 한다.<br><br> **기존 기법의 한계와 Starlink의 특수성**<br> 기존 WF 공격 기법으로는 CNN 기반의 DF(Deep Fingerprinting), VarCNN 등이 활용되어 왔으나, 위성 인터넷은 전통적인 유선망(Fiber network)과 달리 고지연, 위성 간 핸드오프(hand-off), RTT 변동성 등 특수한 네트워크 특성을 가지므로 기존 기법을 그대로 적용할 경우 성능 저하가 발생한다. 실제로 기존 유선 인터넷망에서는 WF 모델이 95% 이상의 정확도를 보이는 반면, Starlink 환경에서는 정확도가 40%대로 급감하는 것을 baseline 실험을 통해 확인하였다.<br><br> **연구 방법 및 모델 설계**<br> 본 연구는 특히 Starlink 환경에서의 네트워크 특성이 WF 공격 성능에 미치는 영향을 분석하고, 이를 바탕으로 위성 통신 환경에 최적화된 Transformer 기반의 새로운 공격 기법을 설계하는 데 초점을 맞추고 있다. 이를 위해 실제 Starlink 환경에서 75개 웹사이트를 대상으로 수집한 데이터셋을 기반으로 CNN 기반 DF, VarCNN 등의 기존 기법을 baseline으로 설정하여 학습하였으며, 최신(SOTA) 성능 확보를 위해 Transformer 및 강화학습 기반 모델도 학습 중에 있다.<br><br> **초기 실험 결과 및 앞으로의 연구 계획**<br> 초기 실험 결과, Starlink 환경에서도 약 56%의 공격 정확도를 달성할 수 있음을 확인하였고, 위성 인터넷의 특성을 반영한 feature engineering을 통해 모델 성능을 추가적으로 향상시키고자 하고 있다. 이 과정에서 도출된 위성망 고유의 취약점을 토대로, Transformer 기반의 모델을 학습하여 Firefox on Starlink 환경에서의 WF 최신(SOTA) 모델을 제안하는 것을 궁극적인 목표로 한다.<br><br> **연구 의의 및 추가 연구 방향**<br> 본 연구는 기존 WF 연구들이 다루지 않았던 위성 인터넷 환경의 보안 취약성을 최초로 체계적으로 분석하고, 해당 환경에 특화된 공격 기법을 제안했다는 점에서 의의가 있다.<br> 졸업 프로젝트 이후에는 본 연구를 확장하여 LEO 기반 위성 통신 환경에 적합한 보안 프레임워크를 제시하고, 위성 네트워크 보안 정책 수립과 대응 전략 마련에 기여하는 것을 목표로 한다.<br> |

<br>

# Project-Summary
| 항목 | 내용 |
|:---  |---  |
| (1) 문제 정의 | 기존 인터넷은 주로 지상 광케이블을 통해 제공되고 있으나, 이 방식은 지형적·경제적 한계로 인해 전 세계 모든 지역에 동일한 품질의 인터넷을 제공하기 어렵다. 이에 대한 대안으로 등장한 것이 저궤도(LEO) 위성 기반의 인터넷이며, 대표적인 서비스로 Starlink가 있다. Starlink는 지구 저궤도에 수천 개의 위성을 배치하여 인터넷을 제공하는 서비스로, 이미 전 세계 많은 국가에서 사용 중이며 한국에도 조만간 상용화될 예정이다.<br><br>그러나 Starlink 네트워크는 긴 지연 시간(latency), 빈번한 위성 간 핸드오프(handover), 높은 패킷 손실률(packet loss) 등 지상 기반 네트워크와는 근본적으로 다른 트래픽 특성을 가진다. 본 연구는 이러한 특성을 이용한 웹사이트 핑거프린팅(Website Fingerprinting, WF) 공격 가능성을 분석하고, 이로 인해 발생할 수 있는 사용자 프라이버시 침해의 보안 위협을 탐구한다.<br><br>Target Customer는 Starlink 사로, 본 연구는 Starlink 네트워크의 보안 위험성을 분석하고 이를 통해 Starlink 사에 새로운 보안 위협에 대응할 수 있는 방안을 제안하여 B2B 모델로 확장할 예정이다. |
| (2) 기존연구와의 비교 | 웹사이트 핑거프린팅 공격은 암호화된 트래픽의 메타데이터(패킷 방향, 크기, 타임스탬프 등)를 기반으로 사용자가 방문한 웹사이트를 식별하는 공격 방식이다. 대표적인 초기 연구인 Deep Fingerprinting(DF)[[1]](#ref1)은 CNN 기반의 심층학습으로 높은 정확도를 달성했으며, 이후 Tik-Tok 및 Var-CNN[[3]](#ref3)은 트래픽 특징(방향성, 시간정보)을 별도의 CNN 모델로 학습하여 성능을 높였다. 최근 발표된 Laserbeak[[4]](#ref4) 모델은 다중 채널 입력 방식을 통해 기존 모델 대비 성능을 크게 향상시켰다.<br><br>그러나 기존 연구들은 안정적인 지상 네트워크의 트래픽 패턴에 기반하고 있으며, Starlink와 같은 위성 네트워크 환경의 특수성을 고려하지 못했다. 본 연구는 Starlink의 고유한 트래픽 특성을 분석하여 기존 연구와의 차별성을 강조하고, Starlink 환경에 최적화된 핑거프린팅 모델 개발 필요성을 제시한다.<br><br>**[참고 논문]**<br><span id="ref1">[1] [A. Sirinam et al., “Deep Fingerprinting: Undermining Website Fingerprinting Defenses with Deep Learning,” ACM CCS, 2018.](https://dl.acm.org/doi/10.1145/3243734.3243768) </span><br><span id="ref2">[2] [A. Panchenko et al., “Website Fingerprinting at Internet Scale,” NDSS, 2021.](https://www.ndss-symposium.org/wp-content/uploads/2017/09/10_3-ndss2016-slides.pdf)</span><br><span id="ref3">[3] [ S. Bhat, D. Lu, A. Kwon, and S. Devadas, "Var-CNN: A data-efficient website fingerprinting attack based on deep learning," Proc. Privacy Enhancing Technol., vol. 2019, no. 4, pp. 292-310, Jul. 2019.](https://arxiv.org/abs/1802.10215) </span><br><span id="ref4">[4] [N. Mathews et al., “LASERBEAK: Multi-Channel Representations and Transformer-Based Models for High-Accuracy Website Fingerprinting,” 2024 IEEE Symposium on Security and Privacy (S&P), 2024.](https://dl.acm.org/doi/10.1109/TIFS.2024.3468171) </span> |
| (3) 제안 내용 | - Starlink 환경에서 실제 발생하는 트래픽 데이터를 실시간 수집하고 분석하여 기존 모델(DF, Var-CNN)의 공격 성능을 평가한다.<br>- Starlink 네트워크의 고유한 특성(긴 latency, 잦은 핸드오프, 높은 패킷 손실)을 바탕으로 특화된 feature engineering을 실시하고, 트랜스포머 기반의 StarPrint 모델을 개발하여 기존 핑거프린팅 모델 대비 성능을 향상한다.<br>- (추후 연구 과제) Starlink 환경에서 효과적인 방어 전략(Padding, Obfuscation, Adversarial Training)을 설계하여 실험적으로 검증한다. |
| (4) 기대효과 및 의의 | 본 연구는 Starlink와 같은 위성 인터넷 환경에서의 WF 공격 가능성을 세계 최초로 분석하고, 기존 지상 네트워크와의 근본적 차이점 및 보안 취약성을 규명한다. 이를 통해 Starlink 사에 보안 위험성을 명확히 제시하고, Starlink 및 유사 위성 인터넷 서비스를 위한 보안 전략 수립에 기초 자료로 활용될 수 있을 것으로 기대된다. 결과적으로 본 연구는 다가올 6G 시대에 위성 인터넷 보안 인프라 구축의 초석이 될 것으로 예상된다. |
| (5) 주요 기능 리스트 | 1. Starlink 특화된 트래픽 feature 추출 및 분석(timestamp, 패킷 크기, Inter-Packet Delay, burst)<br>2. 기존 CNN 기반 모델(DF, Var-CNN 등)의 WF 공격 성능 평가<br>3. Starlink 환경에 최적화된 트래픽 feature engineering 및 트랜스포머 기반 StarPrint 모델 개발<br>4. TSNE, HDBScan 등의 데이터 시각화 도구를 활용한 트래픽 특징 분석<br>5. Data augmentation을 통한 모델 성능 향상 및 과적합(overfitting) 문제 해결<br>6. Open-world 환경에서 추가 데이터셋 수집 및 실험을 통한 성능 확대 및 검증 |


<br>
 
# Project-Design & Implementation
| 항목 | 내용 |
|:---  |---  |
| (1) 요구사항 정의 | *프로젝트를 완성하기 위해 필요한 요구사항을 설명하기에 가장 적합한 방법을 선택하여 기술* <br> 예) <br> - 기능별 상세 요구사항(또는 유스케이스) <br> - 설계 모델(클래스 다이어그램, 클래스 및 모듈 명세서) <br> - UI 분석/설계 모델 <br> - E-R 다이어그램/DB 설계 모델(테이블 구조) |
| (2) 전체 시스템 구성 | *프로젝트를 위하여, SW 전체 시스템의 구조를 보인다. (가능하다면, 사용자도 포함) <br> 주요 SW 모듈을 보이고, 각각의 역할을 기술한다. <br>만약, 오픈소스 혹은 외부 모듈을 사용한다면 이또한 기술한다.* |
| (3) 주요엔진 및 기능 설계 | *프로젝트의 주요 기능 혹은 모듈의 설계내용에 대하여 기술한다 <br> SW 구조 그림에 있는 각 Module의 상세 구현내용을 자세히 기술한다.* |
| (4) 주요 기능의 구현 | *<주요기능리스트>에 정의된 기능 중 최소 2개 이상에 대한 상세 구현내용을 기술한다.* |
| (5) 기타 | *기타 사항을 기술*  |

<br>
