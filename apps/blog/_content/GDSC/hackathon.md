---
title: 'GDSC SKHU 해커톤 in F-Lab'
subtitle: '약 12시간의 개발 마라톤 후기'
date: 2023-02-01 03:36:53
category: 'GDSC'
---

## 주최는 해봤지만, 참가는 처음이라!

특성화 고등학교를 졸업한 저에게 해커톤은 익숙한 대회입니다. 밤을 새워가며 마라톤처럼 쉬지 않고 프로그래밍 하는 해커톤은 특성화고 학생에게는 꽤 가까이, 그리고 많이 있었습니다. 제 친구들은 고등학생 시절부터 해커톤, 앱잼, 게임잼 가리지 않고 참여했습니다.

당연히 저에게도 여러 해커톤 대회에 참여할 기회는 있었습니다. 그러나 저는 '내가 팀의 짐덩어리가 되면 어떡하지'와 같은 생각으로 지레 겁 먹고 참여하지 않았습니다. 물론 고등학생 때는 지금보다 실력적으로도 떨어졌었고, 어떤 분야를 중점적으로 공부할지도 정하지 못해 다소 방황했었습니다. 지금 생각해보면 오히려 이런 태도가 제 실력 성장을 더디게한 것 같아 아쉽습니다. 누구보다 앞서 나갈 수 있었던 시간을 허투루 보내버린 셈입니다.

그래서 고등학교 3년 동안 하지 못한 것, 대학에서는 꼭 해보자는 마음을 가지고 고민할 것도 없이 개발 동아리에 가입했습니다. 저는 동기나 선배들보다 앞서 고등학교에서 경험한 것들을 공유하며 열심히 활동했고, 결국엔 회장직도 맡았습니다. 제가 회장이 된 해 겨울, 드디어 저는 인생 첫 해커톤에 참여했습니다.

그러나 저는 이번에도 '주최자'이자 '운영진'이라는 핑계로 개발은 커녕 팀에 소속되지도 않았습니다. 제가 그토록 아쉬워했던 경험의 기회를 제가 만들고 제가 걷어차버린 격입니다. 지금 생각해보면 '내 실력이 동아리원들의 기대에 못 미치는 실력이면 어쩌지'라는 두려움에 그런 선택을 했던 것 같습니다. 저는 또 다시 후회할 행동을 한 것입니다. 의외로 동아리원들은 제 실력에 별 생각이 없었고, 저 혼자 남 눈치를 보며 소중한 기회를 잃어버렸습니다.

그래서 저는 이번 GDSC 겨울 해커톤이 처음 "제대로" 참가하는 해커톤이었습니다. 저보다 뛰어나거나 저와 비슷한 수준의 개발 실력을 갖춘 사람들과 밤새 협업하면서 고민하고 토론하는 첫 기회였기에 기대도 됐고 긴장도 됐습니다. 이번엔 직접 팀에 참가했고, 팀의 리더를 맡아 주도적으로 프로젝트를 이끌었습니다. 팀원 모두 의욕이 가득했고, 해커톤은 활기찬 분위기 속에 시작됐습니다.

## 코딩 올 나잇

이번 해커톤은 개발자 멘토링 서비스 기업 [F-Lab](https://f-lab.kr/?utm_source=gdn&utm_medium=sa&utm_campaign=mentee&utm_content=mentoring&utm_term=&gad=1&gclid=CjwKCAjwjMiiBhA4EiwAZe6jQzLzQwsWxl_JmBl6eD_elDyRf7yHzubwZqbkEw3C8ioFkobBFwHXsBoCIEsQAvD_BwE)에서 해커톤 장소와 맛있는 야식을 후원해주셨습니다. 23년 1월 27일, 추운 겨울 바람을 뚫고 F-Lab 사무실에서 모인 GDSC 멤버들은 후원사 연설과 간단한 브리핑을 들은 후, 팀 빌딩 과정을 거쳐 본격적인 해커톤을 시작했습니다.

백엔드 파트 코어 멤버였던 저는 이번 해커톤에 백엔드 개발자로 참여했습니다. 팀장이었던 저는, 프론트엔드 담당 2명과 백엔드 담당 1명과 팀을 이루었습니다. 개발을 시작하기에 앞서, 다른 백엔드 멤버와 각자 책임지고 개발할 영역을 나누고 시간 계획을 세웠습니다. 개발에 주어진 시간이 그리 넉넉하지 않아 시간이 낭비되지 않게 해야 했습니다.

또한, 틈틈히 페어 프로그래밍하며 코드 컨벤션을 통일하고 서로의 코드를 바로 이해해가며 최대한 시간을 효율적으로 사용하며 협업했습니다. 다행히 GDSC 백엔드 파트에선 멤버 모두 Spring 프레임워크를 함께 스터디했기 때문에 이번 프로젝트에서 사용할 기술 스택은 큰 어려움 없이 빠르게 결정할 수 있었습니다.

![hackathon01](https://github.com/hangillee/kotlin-practice/assets/14046092/72afd62f-6e3b-4b25-b82a-ad5c67771c78)

<div align="center"><I>서비스 개발 계획에 대해 발표하는 나</I></div>

개발 준비를 모두 마치고 약 12시간 동안 Spring Boot 뿐만 아니라 Spring Data JPA, Spring Security 등, 다양한 Spring 프레임워크 생태계의 프로젝트들을 통해 열심히 웹 어플리케이션 서버(WAS)를 구축해나갔습니다. 이번에 저와 제 팀원들이 기획한 서비스는 소비 기록을 통해 소비 습관 조언 서비스, "달빗"이었습니다. 사용자가 한 달, 혹은 하루에 쓸 수 있는 금액의 한도를 정해두고 매일 가계부를 작성하면 예산 초과 여부에 따라 다양한 소비 습관 개선 방법을 제안하는 서비스입니다. 이 서비스를 구현하면서 가장 어려웠던 점은 "날마다 작성할 가계부를 어떻게 데이터베이스에 저장할 것인가?"였습니다. Spring을 공부하면서 예제로 다뤘던 상품 주문 서비스나 게시판 서비스와는 다르게 년, 월, 일, 예산 등, 다양한 기준으로 조회가 가능해야했고, 프론트엔드 페이지가 달력으로 구성되어 있기 때문에 한 번에 여러 데이터를 조회해야 했습니다. 덕분에 엔티티 클래스에 작성한 필드가 상당히 많아져 제대로 하고 있는게 맞는지 헷갈리기도 했습니다. 그래도 프론트엔드 팀원들과 소비 내역을 어떻게 화면에 그릴 것인지 충분히 상의하고, 프론트엔드 팀원들의 기술 수준과 요구 사항에 맞춰 데이터베이스를 구축한 후, API를 구현하는데 성공했고, 야식을 먹기 전까지는 순조롭게 완성될 것이라 생각했습니다.

![hackathon02](https://github.com/hangillee/kotlin-practice/assets/14046092/5f95bb29-3d0a-46eb-a9ba-b651aecb3af0)

<div align="center"><I>이 사진이 찍힐 때까지는 여유로웠지만...</I></div>

그러나 프론트엔드와 백엔드 간의 통신에서 예상치 못한 오류가 발생했고, 마감 기한 1시간 전까지도 제대로 통신하지 못해 힘들었습니다. 이 문제를 해결하는 과정에서 CORS의 개념에 대해 알게됐고, API 문서화의 필요성을 느끼게 됐습니다. 그동안 로컬 환경에서만 개발하고 프론트엔드와 직접 통신해본 적이 없어 마주치지 못했던 여러 문제들을 파악할 수 있었습니다.

해커톤 덕분에 CORS로 인해 발생하는 문제들을 해결하는 여러 방법을 알게 되었고, 이러한 문제 해결 과정에서 Spring Security가 정말 큰 도움이 되었습니다. 또한, API 문서화 도구들도 어렴풋이 알고만 있었고 어떻게 활용해야하는지, 왜 활용해야하는지 잘 알지 못했는데, 프론트엔드 팀원들이 개발 과정 내내 API의 정확한 사용법을 알지 못해 차질이 생겼던 것을 보며 API 문서화 도구는 필수이며, 사용하는 방법을 꼭 익혀야겠다고 생각했습니다.

서로 API에 대해 이해한 내용이 다르니 서비스가 제대로 동작할 수 없는 것은 너무나 당연한 사실이었습니다. 결국, 프론트엔드는 제대로 배포하지 못한 채 마감 기한이 다가왔고, "달빗"은 미완성으로 남게 됐습니다. 백엔드 역시 CI/CD 파이프라인 구축에 실패하면서 반쪽짜리 완성이었던 아쉬움이 큰 12시간이었습니다.

## 여전히 알아야할 것은 많았다.

저는 지난 하반기 동안 GDSC의 코어 멤버로 활동하면서 일반 멤버들에게 Java, Spring, DB 관련 지식들을 공유하기 위해 열심히 공부했고, 덕분에 백엔드 개발과 서비스 배포 과정을 간단하게나마 홀로 해낼 수 있었습니다. 해커톤에 앞서 동아리원들끼리 진행한 미니 프로젝트까지 성공적으로 완성해 제 실력에 상당한 자신감을 가지고 있었습니다. 그러나, 넘쳐나던 자신감이 무색하게, 아직도 제가 갈길이 멀었다고 느낀 해커톤이었습니다.

특히, Dokcer와 Github Actions를 통한 CI/CD 파이프라인 구축은 완전 처음 해보는 작업이어서 그런지 많은 시행착오를 겪었습니다. 결국 제대로 알지 못하고 사용한 탓에 프로젝트 제출 제한 시간까지 CI/CD 파이프라인은 원하는 대로 동작하지 않았습니다. 좋은 개발자를 꿈꾼다는 사람이 사실상 표준이 되어가는, 백엔드 개발자라면 잘 다뤄야만 하는 기술을 아직도 어려워한다는게 부끄러웠습니다. 대학생이라, 시간이 모자라서, 아직 그렇게 중요하지 않아서 같은 변명은 이제 전혀 통하지 않는 고학번이 되었고, 정말 변해야겠다는 생각이 강하게 들었습니다.

그동안 저는 주변 사람들보다 조금 앞서 나갔다고 해서 절대 뛰어난 실력을 가진게 아닌데, 스스로 '이 정도 했으면 됐다'며 안주했고, 이번 해커톤에서 제 실력의 민낯을 보고 말았습니다. 구글링을 통해 구현하는 것도 제대로 못하는 제 자신에게 화도 나고 여러모로 실망스러웠습니다. 앞으로는 여러 외부 개발 활동에 참여해 최대한 많은 사람들과 다양한 경험을 해보고 제 실력의 척도를 객관화할 필요성을 느꼈습니다. 우물 안 개구리가 되어서는 절대 저의 목표인 "좋은 개발자"가 될 수 없을 것입니다. 여러모로 아쉬운 점이 많았지만 정말 얻어가는게 많았던 해커톤이었습니다.
