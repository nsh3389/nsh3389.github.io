---
layout: post
title: 임베디드 AI를 위한 새로운 프로그래밍 언어의 필요성
categories:
  - Development Supports for AI
---
자율주행자동차나 로봇 등의 임베디드 시스템에서 딥러닝 기반 AI 기술이 필수 요소로 자리잡았습니다. 임베디드 AI 응용의 개발에는 두 가지 접근 방식이 있습니다. 첫 번째 방식은 end-to-end approach이고, 두 번째 방식은 modular approach입니다. End-to-end approach는 하나의 거대한 neural network를 사용하여 응용의 입력에 대한 출력을 바로 얻어내는 방법입니다. 반면 modular approach는 응용을 기능에 따라 여러 개의 module로 나누고, 각 module에 필요에 따라 AI 기술을 적용하는 방식입니다. 혹자는 end-to-end approach을 지향하여야 한다고 주장하지만, (1) 응용 내에 딥러닝이 효과적이지 않은 부분이 있을 수 있고 (2) 학습해야 하는 파라미터의 수가 너무 방대해지며 (3) 예외 상황에 대한 대처가 쉽지 않다는 점에서 한계가 있습니다. 이에 따라 현재 대부분의 회사/연구소들은 modular approach를 채택하고 있고, 이런 경향은 당분간 지속될 것 같습니다.

![placeholder](https://i.imgur.com/Gatb5Qo.png "Figure 1")
*Figure 1. End-to-end Approach vs Modular Approach in Autonomous Driving*

임베디드 AI 응용의 개발 과정은 기존의 임베디드 시스템 상의 응용 개발 과정과는 매우 다르기 때문에 이에 대한 지원이 필수적입니다. Modular approach에 따른 임베디드 AI 응용 개발은 크게 (1) 각 모듈을 개발하는 programming small과 (2) 개발된 모듈들을 합치는 programming large로 나눌 수 있습니다. 우리가 잘 알고 있는 TensorFlow, Caffe, PyTorch 등의 deep learning framework는 programming small에서 딥러닝 기반 알고리즘의 효과적인 개발을 지원합니다. 하지만, 아직 programming large에 대한 지원은 충분히 고민되지 않고 있습니다.

임베디드 AI 응용을 개발하는 회사들이 정보를 공개하지 않기 때문에 파악이 쉽지 않지만, 제한된 정보를 분석해본 결과 이들이 취하는 접근법은 크게 두 가지로 보입니다. 첫번째 접근법은 C/C++ 등 기본적인 프로그래밍 언어와 ROS(Robot Operating System) 등에서 제공하는 통신 라이브러리를 사용하여 각 모듈을 합치는 작업을 직접 수행하는 것입니다. 두번째 접근법은 component-based development 도구를 사용하여 개발을 진행하는 것입니다. 이런 도구들은 매우 많이 존재하지만, 임베디드 AI 응용 개발에는 Matlab의 Simulink, Intempora의 RTMaps, UC Berkely의 Ptolemy II가 많이 쓰이고 있습니다.

하지만 이런 접근에는 한계가 있습니다. 두 가지 접근법 모두 임베디드 AI의 핵심 요구사항인 real-time stream processing에 대한 고려가 되어 있지 않기 때문입니다. Real-time stream processing을 위해서는 임베디드 기기에 수많은 센서 데이터가 연속적으로 흘러 들어와서 처리되는 과정에서 timing constraints를 만족 시켜야 합니다. 이를 위해 개발 과정에서 지원해야 하는 요소들을 정리해보면 아래와 같습니다.

1. Visual programming
    - 스트림 데이터의 처리 과정을 개발자에게 시각적으로 표현할 수 있어야 함
2. Timing annotation
    - Stream processing의 timing constraints를 개발자가 명시할 수 있어야 함
    - 네 종류의 timing constraints가 존재[^Noh18]
        1. Deadline
        2. Minimum rate constraint
        3. Freshness constraint
        4. Correlation constraint
3. Exception handling
    - 개발자가 예외상황을 정의하고 각각에 대한 처리를 명시할 수 있어야 함
4. Support for sensor fusion
    - 센서 퓨전의 시간 동기화 이슈들을 간단하고 명확하게 기술할 수 있어야 함
5. Support for multiple modes
    - 조건에 따라 선택적으로 알고리즘을 수행하는 모드 선택 기능을 지원해야 함
6. Integration support
    - Data-driven, time-driven, event-driven 등 서로 다른 프로그래밍 방식으로 개발하는 도메인 간의 integration을 지원해야 함

*Table 1. Limitations of Existing Component-based Development Tools [^Exp1]*
![placeholder](https://i.imgur.com/jtML0qV.png "Table 1")

위의 표는 대표적인 component-based development 도구들이 위에서 언급한 지원 요소들을 어느 정도 고려하는지를 분석한 결과입니다. 어떤 도구도 real-time stream processing을 위한 고려를 충분히 하고 있지 않습니다. 결과적으로 대부분의 기업과 연구소들은 요구사항 충족을 세부 알고리즘 개발자들에게 전적으로 맡기고 있습니다. 이렇게 될 경우 개발자는 과중한 부담으로 인해 개발 효율이 떨어지게 됩니다. 또한, 요구사항에 체계적으로 접근할 수 없기 때문에 제대로 요구사항을 만족시키기도 어렵습니다.

결론적으로 임베디드 AI 응용을 제대로 개발하기 위해 필요한 것은 real-time stream processing을 지원할 수 있는 새로운 개발 방법론입니다. 현존하는 어떤 도구와 개발 방법론도 이를 충분히 고려하고 있지 않습니다. 가장 바람직한 방법은 위에서 정리한 real-time stream processing을 위한 세부 요소들을 모두 지원하는 새로운 프로그래밍 언어를 정의하고, 이에 기반한 개발 방법론을 만드는 것일 겁니다. 다음 포스트에서는 현존하는 stream processing을 위한 programming language들을 정리하고 이들을 활용할 수 있을지에 대해 고민해보도록 하겠습니다.

[^Noh18]: Soonhyun Noh and Seongsoo Hong, "Splash: Stream processing language for autonomous driving," 15th International Conference on Ubiquitous Robots, 2018.
[^Exp1]: RTMaps의 경우, 공개된 documentation이 부족하여 정확한 분석이 아직 되지 않음
