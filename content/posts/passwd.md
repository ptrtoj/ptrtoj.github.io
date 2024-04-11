---
title:  "올바른 비밀번호 설정 방법"
date:   2021-10-20
url: 'passwd'
---

$ 올바른 비밀번호 설정 방법

## 서문

사이버 보안에 관심이 많거나, 개발 직종에 근무하시는 분들은 이미 알고 있는 내용이겠지만 그렇지 않은 분들이 너무 많은 오해를 하고 있거나 관심이 없는 것 같아서 적습니다.

아래에서 쉬운 이해를 위해 ‘해킹’, '추측'까지 소요되는 예상 시간이라고 적고 있으나, 엄밀하게 말하면 그 자체로 ‘해킹’이라기 보다는 ‘비밀번호 찾는 것만을 목표로 했을 때 정답을 찾을 때까지 소요될 예상 시간’입니다.

최근에는, 보안을 위한 다양한 방법 중 웹사이트 측 방법론인 ‘공격 시도와 또 다른 시도 사이의 물리적인 시간 자체를 늘리는 쪽’으로 개발하기 때문에, 현실에서는 훨씬 오랜 시간이 걸릴 수 있습니다.

예를 들어, 비밀 번호 n회 연속 오류 시, 물리적으로 x분 간의 로그인 시도 자체를 차단하는 것, 은행들의 경우 아예 창구를 방문해야만 접근 해제를 풀어주는 방법까지 사용합니다.

그러나 이 글에서 소개하는 여러 가지 방법을 활용하면, 시스템 측에 의존하지 않고 스스로도 안전한 비밀번호를 생성할 수 있을 것으로 기대합니다.

## 현 상황

일반적으로 알고 있는 '안전한 비밀번호'의 개념은 완전히 잘못되어 있습니다.

최근 한국의 일부 웹사이트에서 요구하는 비밀번호 요구 사항은

- 특수 문자를 0~2개(사이트 별로 상이) 요구
- 대문자 요구 ← 큰 의미가 없음
- **글자 수 20자 이내 ← ‘이내’ 자체가 말도 안 되는 요구임**

등 입니다.

저 조건들을 만족한다고 안전한 것이 아닙니다.

더불어, 저 조건들을 더 잘 조합했다고 안전해지는 것도 아닙니다.

## 예시

그럼 예를 들어 보겠습니다.

아마도 김 아무개씨가 쓸 것으로 보이는 비밀번호 'kimcat02'의 경우, 추측에 '14.04분'이 소요됩니다. 요즘엔 저런 비밀번호로는 가입 단계에서 거절되므로, 조건을 하나씩 부합시켜 보겠습니다.

각 단어를 대문자로 변경한 'KimCat02'의 경우, '53.25 '으로 늘어났습니다. 1시간이 채 걸리지 않습니다.

> **예상 소요 시간 관련**
>
> 등장할 예상 소요 시간은 모두 [유틸 사이트](https://passwordmonster.com/)의 예상치입니다만 계산 사이트가 다르다고 결과가 크게 상이하지는 않습니다.

아! 그런데 특정 웹사이트들은 IT 업체 답게 특수 문자를 요구하죠! 그것도 3개를 넣으라고 합니다. '!Kim@Cat#02'를 했더니 '27년'이 걸립니다. 27년이면 괜찮아 보이나요? 어떤가요? 이 글을 끝까지 읽으시면 매우 놀라실 겁니다.

## 또 다른 문제: 비밀번호 3개월/6개월마다 변경?

그럼 비밀번호를 일정 기간마다 변경하는 것은 의미가 있을까요?

**아니요.**

해킹 공격은 '당했다/안 당했다'로 나뉘지, '비밀번호가 새로운 거다/조금 오래된 거다/아주 오래된 비밀번호다'로 나뉘지 않습니다. 해킹 공격자가 대상으로 삼은 웹사이트에 내가 회원인데 **마침 그 때 내 비밀번호 자체가 쉬운 것으로 되어 있는 것이 문제**지 바로 이틀 전에 변경했다는 점은 해킹 방어에 그 어떤 도움도 되지 않습니다.

## 해답

해답은 '길이'에 있습니다.

비밀번호의 복잡도가 한정된 길이에서 증가하는 것보다 차라리 물리적인 길이 자체가 늘어나는 게 더 효율적입니다.

아까 27년이라 괜찮아 보였던 예시를 가져오겠습니다. 해당 비밀번호는 '!Kim@Cat#02'였습니다.

차라리 대문자, 특수 문자를 빼고 원래 11 글자였던 비밀번호의 길이를 22 글자로 늘리면 어떻게 될까요?

'kimcatwoodskywaterpark'는 정확히 22 글자이고 해킹에 소요되는 시간은 '3,100년'이 걸린다고 나옵니다.

전체 길이가 늘어나면 한 자리, 한 자리에 소요되는 경우의 수가 ‘알파벳 개수의 제곱 형태(자릿수를 22 자리로 한 경우 $26^{22}$)로 증가하기 때문에 훨씬 효율적입니다.

대소문자와 특수 문자를 포함 시켜 봐야 늘어나는 자리당 경우의 수 열 댓가지 보다 차라리 제곱을 11 제곱에서 22 제곱으로 늘리는 것이 자릿수만 충분하다면 무조건 더 큰 숫자이기 때문입니다.

그렇다면, 길이를 늘리는 것은 알겠는데 그렇게 긴 비밀번호를 잘 외울 수 있을까요? 여기서 아래 두 파트 '한국인이라 갖는 장점 '과 '나만의 알고리즘 '이 중요합니다.

> **한국의 상황**
>
> 그런데도 한국의 일부 웹사이트는 가입 시점부터 이미 20글자 이내로 설정하도록 요구합니다.
>
> 더 어이 없는 경우는 가입시점엔 20글자 이상을 입력했는데도 필터링 되지 않고 로그인 시에 20 글자까지만 입력되어 로그인을 할 수 없는 촌극이 벌어지는 것입니다.
>
> 해당 사이트 이름을 밝히고 싶지만, 고소가 무서워서 여기서 적진 않겠습니다. 컴퓨터 소프트웨어 제조사 홈페이지 소수와 정부 부처 홈페이지 다수가 그렇습니다.
>

## 한국인이라 갖는 장점

한국인이라서 갖는 특이한 강점이 하나 있습니다.

물론 따지고 보면 모든 비 영어권 국가 사람들이 갖는 강점이긴 합니다.

바로, 외국인들이 사전 공격(Dictionary Attack; 완전히 무작위한 단어를 사용하지 않을 것을 감안해 사전에 등장하는 단어를 통해 공격하는 기법)을 하기 위해 사용하는 영단어를 쓰지 않을 수 있다는 점입니다.

집에서 키우는 반려견 이름을 꼭 비밀번호로 활용하고 싶더라도 (하필 또 그 이름이 영단어인 '초코 '일지라도) 비밀번호를 'choco '로 쓰기보다 'chzh '로 쓰는게 낫습니다.

> **억지 비교**
> 아주 무식한 케이스지만 그냥 단순 비교를 위해 ‘choco’와 ‘chzh’ 둘 자체의 비밀번호 공격 소요 시간을 비교해보면 **1.61초 vs 22.85초**로 산출됩니다.
>
> 놀라운 점은 한글을 단순히 영타로 친 'chzh '의 경우 **오히려 한 글자가 적다**는 점입니다.

따라서 비밀번호에 사용할 단어들이 영단어일지라도 가급적 한글 발음으로 적듯이 적는 것이 낫습니다.

(조바심에 더 적는 예시: 'password ' - > 'votmdnjem ' 또는 ‘qlalfqjsgh’ , 'bank '- > ‘qodxm’ 또는 'dmsgod ' 등)

## 나만의 알고리즘

그래도, 모든 웹사이트에서 동일한 비밀번호를 사용한다면, 한 군데에서 혹시나 해킹에 성공하거나, 또는 내 비밀번호가 실수로 유출되는 경우(예. 은행 비밀번호가 적힌 통장 자체를 분실) 모든 사이트에서 공격을 받게 됩니다.

따라서, 본인만의 알고리즘을 생성하면 매우 유용합니다. '알고리즘 '이라는 단어를 사용해서 막 어려울 것 같은 기분이 들지만 전혀 그렇지 않습니다. 안 그래도 길어져서 외우기 힘들어진 비밀번호를 외우기 쉽게 해주기도 합니다.

알고리즘을 만든다는 뜻은 본인이 웹사이트 가입을 할 때 혹은 이 글을 읽고 설득되어 현재 가입한 사이트들의 비밀번호를 변경하고자 할 때, 규칙을 정하는 것입니다.

자, 이런 식입니다.

`(각 사이트 비밀번호 = (내이름)#(내가제일좋아하는동물)#(내가태어난도시)#(가입하려는사이트이름)`

적용한다면 이렇습니다.

네이버를 가입한다고 하면, 비밀번호는 `(전우형)#(강아지)#(서울)#(네이버)`.

이렇게 보면 너무 쉬워 보이지요? 그러나 전체를 전환해보면 다음과 같아집니다.

`wjsdngud#rkddkwl#tjdnf#spdlqj`

> **특수 문자**
> 에이, 너도 특수문자 넣네? 라고 하실수도 있는데, 특수 문자 넣는 것 자체에 반대하는 것도 아닐 뿐더러 파트를 구분하기에도 용이해서 적극 활용할 수 있습니다.
>
> 본인이 좋아하는 숫자 순서가 있다면 해당 순서로 특수 문자를 삽입하는 것도 좋습니다.
>
> 예를 들어, 3278을 비밀번호로 자주 사용하는 사람이라면 '가나다#비밀번호 @예시&특수문자 * '와 같습니다.

아까 활용하던 사이트에 입력해볼까요? 소요 예측 시간은 '4 hundred trillion trillion trillion years'로 나왔습니다.

이게 그러니까 검색해보니, '**400 * 조 * 조 * 조 년**'이 걸린다는 얘기네요.

이렇게 웹사이트마다 다른 비밀번호를 손쉽게 만들어낼 수 있습니다.

## 그래서 결론은?

1. **무조건 길이를 길게 만드는 것이 좋습니다.**
2. 한국인이라는 장점을 십분 활용해야 합니다.
3. 본인만의 비밀번호 생성 알고리즘이 필요합니다.
4. **일부 한국 웹사이트의 보안 요구 사항은 반드시 수정 되어야 합니다.**

![비밀번호 강도 관련 밈](/pw-strength.png)