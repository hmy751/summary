# Monorepo
---
## 모노레포 이전의 개발 방식

모노레포 이전의 개발 방식은 모놀리식(monolithic)에서 멀티레포(multi repo)개발 방식으로 진행되었다.

먼저 모놀리식 개발은 하나의 저장소에서 개발하는 방식을 말하며, 멀티레포는 각 고유한 저장소에서 프로젝트를 따로 개발하는 방식을 말한다.

### 모놀리식의 문제
모놀리식은 하나의 저장소에서 개발하기 때문에 프로젝트의 크기가 커지게 된다. 
이를 해결하기 위해서 기능들을 모듈화 하여 분리하고, 재사용하는 로직들은 공유하며 개발하는 방법으로 해결했다. 
모듈들을 분리했기 때문에 각 모듈에 필요한 저장소로 나누어서 개발하는 방식으로 바뀌게 되었다.

### 멀티레포의 문제
멀티레포는 모놀리식에서 단점을 보완하기 위해 모듈화로 분리된 각 프로젝트들을 각 고유한 저장소로 분리하여 보완하는 방식이다.
각 팀마다 고유한 저장소를 가지고 개발하기 때문에 자율성이 높다. 하지만 문제점은 고유한 자율성 때문에 각 레포가 고립되는 문제가 되며, 따로 지정된 설정들 때문에 협업을 하기 어려워지게 된다.

먼저 문제점들은
- 새로운 패키지를 생성할 때마다 
CI/CD, 테스트, 코드 컨벤션등을 따로 지정하여 개발을 하게 된다.
- 패키지 중복 코드
- 번거로운 프로젝트 구성


## 모노레포가 등장한 이유?

기존의 멀티레포의 문제점을 보완하기 위해 나온 개발 방식으로 각 저장소로 분리하여 개발하게 되는 단점 중 자율성과 그로 인한 고립되어 협업하기 어려운 부분을 보완한다.
즉, 제일 큰 이유는 협업을 위해 보완하고자 나온 개발 방식이다.

## 모노레포의 특징

모노레포는 분리된 프로젝트를 하나의 저장소에서 관리하여 개발하는 방식이다.

### 장점
공통된 저장소에서 개발하기 때문에 eslint 설정 및 코드 컨벤션, 
간단한 프로젝트를 구성,
같은 깃 커밋 등으로 히스토리 공유,

코드 공유,  CI/CD 및 테스트, 패키지 의존성 등을 공유할 수 있어 프로젝트를 따로 분리하면서도 팀 간의 협업을 유리하게 만든다.

### 단점
하나의 저장소에서 관리하기 때문에 저장소의 크기가 커져 관리가 어렵다.
빌드 및 테스트시 모든 프로젝트를 같이 하므로 시간이 오래 걸리게된다.

단점을 보완하기 위해 분산 빌드


https://d2.naver.com/helloworld/0923884
https://monorepo.tools/