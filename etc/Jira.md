# Jira
- Jira의 주된 기능 **이슈 트래킹 = 이슈 추적**

## 이슈트래커 장점
- 특정 이슈를 누가 발견했는지, 누가 해결해야 하는지, 이슈는 현재 어떤 상태인지 한눈에 해결 및 관리가 가능하다.
- 실무자의 경우 이슈에 대한 역할과 임무를 분명히 할 수 있으며, 협업 시 불필요한 커뮤니케이션 비용을 줄일 수 있다.
- 이슈 해결에 대한 히스토리가 남기 때문에, 후에 비슷한 이슈에 대한 해결 과정을 되짚어볼 수 있다.
- 개발 단계에서 버그를 관리하거나, 소스 혹은 이미지 수정 내역을 남길 수 있어 편리하다.

## Jira에서 관리하는 이슈의 종류
### User Story
- 사용자가 요구 사항이나 개발의 대상이 되는 기능. 
- User Story를 구현하기 위해서는 각각의 User Story는 구체적인 작업인 Task를 하위작업으로 가지고 있다.

### Task
- User Story의 하위 작업으로 User Story를 위해서 개발자가 실제로 작업해야 하는 각각의 단위 작업을 의미.

### Bugs
- 개발 과정 중에 보고된 버그

### Enhancement Request
- 기능 개선 요청으로 기능 추가 작업을 의미.


## Issue단위의 작업 절차
1. 먼저 PM이 요구 사항을 취합하여 User Story를 작성한다.
2. 다음으로 User Story를 구현하기 위해서 실제 Task들을 해당 User Story아래에 생성한다.
3. 다음으로 생성된 Task들을 개발자에게 지정(Assign)한다.
4. 또는 Assign되지 않은 작업에 대해서 개발자가 스스로 작업을 가지고 가서 작업을 진행한다.

## Jira Issue 생성 절차
1. **Create Issue** 
2. **Project 이슈가 발생한 프로젝트 선택** : 프로젝트는 주로 PM이 생성하고, 실무자들은 해당 프로젝트를 선택하여 이슈를 등록한다.
3. **Issue Type 지정** : 위에 작성한 이슈 종류 (User Story, Task, Bugs...etc)를 선택한다.
4. **Summary 이슈 요약** : 다른 이해관계자들이 이슈에 대한 대략적인 내용을 파악할 수 있도록 이해하기 쉬운 한 문장 정도로 정리한다.
	
	> ex. 속도를 더 빠르게 해야 한다. (X)
		-> 트랜지션 속도 향상을 위한 썸네일 분할 로딩 구현 (O)
5. **Assignee 피할당자 지정** : 이슈를 할당 받아 처리해야 하는 담당자 혹은 책임자를 지정한다.
6. **Description 이슈 설명** : 이슈의 구체적인 내용을 작성한다.

## 이슈의 Status
이슈가 Workflow의 어느 단계에 있는지를 명시해준다.

![생성된 이슈 상세화면의 status항목들](https://66.media.tumblr.com/ce3d8d8a55b592832b0168ba7be83eb0/tumblr_inline_mut3jtpiw01snemdk.png)


## 이슈의 Resoultion
이슈에 대한 작업이 완료되었다면, 어떤 식으로 해결되었는지를 보여준다.
추후에 비슷한 이슈가 발생할 경우, 참고할 수 있는 기록이 된다.
![](https://66.media.tumblr.com/de37924b72d77634e4968a032b21f1a8/tumblr_inline_mut44vIS9f1snemdk.png)


[참고: 소프트웨어 프로세스 관리도구 JIRA와 사용법 정리](http://hellogohn.com/post_one160)

[참고: Jira를 통해 프로페셔널하게 프로젝트 협업하기](http://uxd.team.handstudio.net/post/64286399069/jira%EB%A5%BC-%ED%86%B5%ED%95%B4-%ED%94%84%EB%A1%9C%ED%8E%98%EC%85%94%EB%84%90%ED%95%98%EA%B2%8C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%98%91%EC%97%85%ED%95%98%EA%B8%B0)
