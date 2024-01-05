---
title:  "GPG 첫 걸음부터"
date:   2021-09-06
url: 'gpg'
---

## 서문

아치 리눅스 설치 가이드를 작성할 때도 마찬가지였지만, 기본적으로 새 글을 작성할지 결정할 때는 '내가 실제로 공부하기 까다로웠는가'를 기준으로 설정합니다. 이미 영문/한글로 작성된 양질의 가이드가 충분히 존재해서 스스로 공부하는데도 어려움이 없었고, 뒤에 공부할 사람들도 쉽게 배울 수 있을 것으로 예측되는 것들에 대해 작성하는 것은 '내가 이만큼 신기한 것도 안다'하는 따위의 자랑밖에 안된다고 생각했기 때문입니다.

GPG는 '매우' 까다로웠습니다. 온갖 자료가 분산되어 흩뿌려져 있습니다. 특히 한글 자료는 전무하다고 봐도 무방합니다. 이는 교보문고, yes24에서 'gpg'를 검색해도 쉽게 알 수 있습니다. 따라서 뒤에 이 주제를 공부할 분들이 기본 개념을 쉽게 파악하는 것을 돕고자 공부한 자료를 교차 검증하고 재검토하여 한 문서에 모았습니다.

개념 파트에서는 이어지는 실제 예시들을 읽을 때 처음 접하는 정보가 없도록 하기 위해 세세하게 정리하였습니다. 당장 따라해야할 명령어들만 필요하신 분들은 생략하시면 됩니다.

부디 도움이 되기를 기대합니다.

## 감사 말씀

이 작은 글이 기념비적 업적을 남기는 다른 글과 비교하면 보잘 것 없지만 작성자 본인에게는 의미가 큽니다. 이 짧은 글을 작성하는 데도 많은 분들의 도움이 있었습니다. 아래에 특별히 언급한 분들뿐아니라 온라인과 오프라인으로 접근할 수 있었던 각종 문서를 앞서 작성하여 지식을 전해준 모든 분들께 감사할 따름입니다.

Michael W. Lucas의 책이 아주 큰 도움이 되었습니다.

준비, 작성, 수정 과정에 도움을 준 경희대학교 행정학과 김진영 학우께 특별히 감사 말씀을 드립니다. 까다롭고 수준 높은 기준으로 글을 검토하고 수정할 사항들이 있을 때마다 가감 없이 건의해주었습니다.

마지막으로, 혼자 뭔가 하는가 싶더니 뜬금없이 '이건 정리가 필요하다'면서 컴퓨터 앞에 앉아 며칠이고 자판을 두드리고 문서와 책을 옮겨 뒤져가며 무언가를 작성하는데도 응원해준, 또, 관심 분야가 아닌데도 불구하고 비전공자 입장에서 이해가 어려울만한 부분을 지적해준 와이프에게 감사하다는 인사를 전합니다.

## 미리 알아야 하는 것

수학, 컴퓨터 관련 배경 지식이 없어도 이해하기 쉽게 작성했습니다. 수학적인 개념이 등장할 때도 그 자리에서 쉽게 설명했습니다. 더 설명이 필요한 부분이 있으시면 '[jeon@ptrtoj.com](mailto:jeon@ptrtoj.com)'으로 알려주시기 바랍니다.

## 개념

### 배경

모든 것의 시작점이 되는 PGP(Pretty Good Privacy; 꽤 좋은 보안)는 1991년 필 짐머만(Phil Zimmermann)에 의해 개발되었습니다. 이는 당시 냉전 상황에서 적국이 수준 높은 보안 기술을 갖게 될 것을 우려한 미국 정부의 각종 규제에 직접적으로 반하던 것입니다. 짐머만은 소프트웨어의 외부 반출을 금하는 법의 허점을 이용해 출판물로써 PGP 기술들을 반출합니다. PGP 기술을 이식하기 위한 소스 코드와 개념이 적힌 책을 출판한 것입니다. 이로 인해 상당한 기간 법정 소송에 휘말렸으나 오히려 정부의 소 제기로 인해 짐머만은 자유 진영의 영웅이 되었습니다. 더 자세한 역사는 [링크](https://www.openpgp.org/about/history/)를 통해 확인하시면 좋겠습니다.

### 용어 정리

GPG를 포함한 다양한 암호 관련 문서에서는 용어가 상당히 혼란스럽게 쓰입니다. 따라서, 이 글에서만큼은 통일성을 유지하기 위하여 본문에서 사용할 용어들을 미리 정리하겠습니다. 이미 GPG를 이해하고자 여러 글들을 읽으며 씨름하셨던 분들은 오히려 '용어 정리'를 읽으며 개념이 이해될 수도 있습니다. 아래 용어 정리를 읽으며 무슨 뜻인지 전혀 이해가 되지 않더라도 본문을 읽으신 후 재확인하시면 이해가 수월할 것으로 기대합니다.

- **평문**: 지금 이 문서처럼 글을 이해할 수 있는 사람이 보는 즉시 내용을 파악할 수 있게 노출된 글
- **암호문**: 'R3i21+UlSPUlOVcT6AJ98popJr4TA5mlUSX'과 같이 특수한 방법을 이용하지 않고는 그 내용을 파악하기 힘든 글
- **암호**: 상호 간에 정한 약속이나, 혹은 내가 암호문을 만들거나 해독하기 위해 만든 '단어' 혹은 '구절' 등 (예: "발송한 암호화된 문서의 비밀번호는 '1234'입니다."에서 '1234'')
- **대칭 암호**: 'Symmetric Cryptography'로 표현하며 상대와 내가 동일하게 알고 있는 '암호'를 통해 구현하는 기법. (예: "발송한 암호화된 문서의 비밀번호는 '1234'입니다." 에서 사용하고 있는 기법)
- **비대칭 암호**: 'Asymmetric Cryptography'로 표현하며 상대와 내가 암호화된 문서 교환을 위해 서로 다른 키를 갖는 기법.
- **공개키**: 'Public Key'로 표현하며 외부에 알려져도 되고 통신 상대방이 알아야 활용할 수 있는 키
- **개인키**: 'Private Key'로 표현하며 외부에 절대 알려져서는 안되는 나의 비밀키
- **키 쌍**: 'Key Pair'로 표현하며 공개키와 개인키가 하나의 쌍으로 동시에 생성되고 관리됨
- **키 링**: 'Key Ring'으로 표현하며 하나의 키 링에 여러개의 키 쌍(주요키 한 쌍과 다수의 서브키 쌍들)을 가질 수도 있고, 그냥 하나의 키 쌍(주요키 한 쌍)만을 가질 수도 있음
- **주요키**: 'Primary Key' 혹은 'Master Key', 'Main Key' 등으로 혼용되고 있으며 키 링 자체를 만들 때 반드시 한 쌍 생성됨. 가장 기본이 되고 중요한 키 쌍. **주요키는 중요하다는 강조로 해석될 경우 마치 '중요한 키'처럼 이해될 우려가 있습니다. 이 문서에서 주요키는 마스터키로 사용되고 있는 해당 '키'를 지칭합니다. 주의하시기 바랍니다(하단 표 참조).**
- **서브키**: 'Sub Key'로 표현하며 주요키로 관리되는 키 링 아래에 한 개 혹은 여러개로 존재할 수 있는 키 쌍(들)을 '서브키 쌍'이라고 부름
- **해쉬**: 'Hash' 함수로 표현하며 길든 짧든, 문자든 숫자든 인풋을 받아 '고정 크기(길이)'의 값으로 반환하는 함수. 대표적인 것들은, 'MD5', 'SHA-256', 'SHA-512' 등.
- **키 서명**: '다른 사람의 키를 안전한 것으로 확인한다'는 '인증(Certificate)'을 표현하고자 '키 서명(Signing a key)'이라는 표현을 씁니다. 이것은 다른 기능을 하는 '서명(S)'이라는 표현과 중복됨에도 함께 쓰이고 있습니다. 실제로 '키 서명' 과정에서는 '인증'에 해당하는 'C' 기능이 부여된 키가 활용됩니다. 문제는 하필, 이 'C' 기능이 부여된 키가 'S'도 함께 부여받는 경우가 잦다는 것입니다(하단 표 참조). **여기서의 '서명(C)'은 암호화(E)와 함께 활용하는 '서명(S)'과 다릅니다. '서명'이 쓰인 곳 앞이나 뒤에 '타인의 키', '키' 같은 말이 같이 쓰이는 경우 '그 키가 안전하다는 것을 서명한다'는 의미(C)로 사용됩니다.**

혼란을 유발하는 가장 근본적인 개념으로 보이는 것이라서 다시 한번 강조하고 넘어가겠습니다. 공개키/개인키와 주요키/서브키는 서로 다른 기준에서 구분됩니다.

- 주요키
  - 공개키
    - 외부에 공개 가능
    - 보통 'C'기능(다른 키의 신뢰를 인증하는 '키 서명'에 활용) 부여
    - 보통 'S'기능(문서가 발송자의 것임을 확인할 수 있도록 '서명'하는데 활용됨)도 부여
  - 개인키(<-- 비밀키)
    - 외부에 절대 알려져서는 안됨
    - 주요키에 서명 기능이 부여된 경우, 서명 과정에서 실제로 활용되는 키
- 서브키
  - 공개키
    - 외부에 공개 가능
    - GPG는 통상적으로(`gpg --full-generate-key`사용시) 'E'기능(문서 암호화)만을 갖는 서브키를 자동으로 생성함
  - 개인키(<-- 비밀키)
    - 외부에 절대 알려져서는 안됨
    - 상대방이 내 공개키로 암호화한 문서를 발송한 경우, 해당 문서를 복호화(암호를 품)하는데 사용하는 키

### 대칭 암호

암호의 종류는 크게 두 가지로 나눌 수 있습니다. 대칭 암호와 비대칭 암호입니다. 대칭 암호의 경우, 학생 시절에 친구와 비밀 쪽지를 주고 받기 위해서 사용한 경험이 있을겁니다. 예를 들어, 각 알파벳에 순차적으로 숫자를 부여하는 기법을 상호 간에 미리 규약하는 방법입니다. 따라서, A를 1로 시작하고 각 알파벳에 순차적으로 숫자를 부여하기로 약속한 경우, '3, 1, 18'의 쪽지를 보내면 친구가 자연스럽게 'CAR'로 해독할 수 있는 것입니다.

학생들도 자연스레 비밀을 주고 받기 위해 떠올릴만큼 단순하고 명료한 방법입니다. 그리 중요하지 않은 문서를 주고 받을 때에는 매우 효과적인 방법일 수 있습니다. 문서 자체를 특정 숫자로 암호화한 후 발송하고 수신자에게 전화 혹은 문자 등을 이용해 비밀번호만을 전송해도 수신자가 해당 문서를 풀 수 있기 때문입니다.

이 단순해보이는 알고리즘도 쉽게 복잡도를 증대시킬 수는 있습니다. 쪽지 암호화 기법에서 한 단계 더 나아간 것이 군대에서 활용한 암구호입니다. 거동이 수상한 사람을 발견할 경우, 초소 근무 병사가 묻는 '문어'와 해당 '문어'에 적합한 답인 '답어'로 구성하는 방법입니다. 이를 더욱 발전시켜, '답어'를 여러 가지로 구성한 이후, 각 '답어'를 제시하는 '부서'가 다르게 하여 정체를 파악하는 방법으로 나아갈 수도 있고, 혹은 24시간으로 재설정되는 간격을 더욱 짧게 하여, 노출 위험의 지속 시간을 줄이는 방법이 있습니다. 물론, 관리의 비용도 증가할 것입니다.

### 대칭 암호의 약점

하지만 진짜 문제는, 관리 비용 증가 같은 '불편한' 수준의 번거로움이 아니라, 대칭 암호 자체를 관통하는 위험이 도사리고 있다는 점입니다. 이는 중간에서 정보를 탈취하는 공격에 매우 취약하다는 문제입니다. 친구간의 쪽지 암호화에서 알파벳 'A'가 어떤 숫자로 변환될지를 가운데 앉은 친구가 들은 경우 전달 과정에서 모든 암호를 해독할 수 있고, 암구호의 경우 아무리 자주 변경하더라도 해당 암구호 자체를 적에게 알리는 첩자가 존재하는 경우 보안은 아예 무효화됩니다.

### 비대칭 암호

이 문제를 도대체 어떻게 해결할 수 있을까요? 해당 문제를 해결하기 위해 매우 적합한 암호화 기법이 바로 두 번째 구분으로 소개할 비대칭 암호(Asymmetric encryption)입니다. 이름은 어렵지만 개념은 매우 간단합니다. 남들에게 알려져도 전혀 무방한 공개키와 절대 알려져서는 안되는 개인키 두 개를 활용하는 기법입니다. 따라서, 비대칭 암호는 '공개키 암호'로 통칭되기도 합니다. A가 B에게 암호화된 쪽지를 전달하고자 하는 경우 알려진 B(수신자)의 공개키를 이용해 쪽지를 암호화한 이후 B에게 전달하면 B가 본인만 알고 있는 개인 키를 이용해 이를 해독하는 것입니다. 중간에 아무리 많은 도청자들이 있다고 하더라도 도청될 것들은 A도 마찬가지로 알고 있는 B의 공개키 뿐이므로 해독에 전혀 도움이 되지 않습니다.

아주 효과적인 방법을 찾아내었습니다! 다만 이런 기법이 실제로 가능할까요? B의 공개키로 암호화된 문서가 어떻게 B의 공개키로 해독되지 않고 B의 개인키로만 해독이 된다는 말일까요? 이 해답은 '수학'이 제시하였습니다. **도저히 이런 것이 가능하다고 믿기지 않고 직접 확인해보고 싶으신 분들은 '알고리즘 종류' 단락을 읽어보시면 되고 '난 납득할 수 있다' 하시는 분들은 '알고리즘 종류' 단락을 생략하시면 됩니다.**

### 알고리즘 종류

마술과 같은 공개키/개인키 방법을 실제로 구현하기 위해서는 '알고리즘'이 필요합니다. 즉 해당 방법이 가능케할 '실제 방법론'이 필요한 것이죠. 개념상으로 아무리 완벽하다고 하더라도 구현할 방법이 없다면 의미가 없으니까요. 그런데 이를 구현할 아주 다양한 알고리즘이 실제로 제작되고 발전해왔습니다.

> 각 알고리즘 종류의 이름 끝 알파벳인 'A'가 영문으로 '알고리즘(Algorithm)'에 해당하기 때문에 'DSA 알고리즘'식의 표기 대신 'DSA' 자체로 표기합니다.

과거에는 'DSA(Digital Signature Algorithm; 디지털 서명 알고리즘)'를 활용하였습니다. 현재는 'RSA'를 가장 널리 사용하고 있습니다. 'RSA'의 경우, 제작자의 이름인 'Rivest, Shamir, Adleman' 세 명의 이름 앞글자를 따왔습니다. 그리고, 앞으로 쓰일 것으로 예측되는 'ECDSA(Elliptic Curve Digital Signature Algorithm)'를 벌써부터 적극적으로 사용하는 사람들이 늘어나는 추세입니다. 'ECDSA'는 'ECC(Elliptic-Curve Cryptography)'로 불리는 기하학적 방법론으로 'DSA'를 개선한 알고리즘입니다. 'ECC'는 다양한 암호학 분야에서 아주 관심이 많은 새로 등장한 접근법입니다.

여기서는 현재 가장 광범위하게 쓰이고 있는 RSA에 대해서 설명하겠습니다.

### **원리**

>쉬운 설명을 위해 단순화한 것으로 원리상으론 올바르지만 실제 사례와 괴리가 있습니다.

다른 암호화 기법도 마찬가지지만, RSA도 '소수'를 활용합니다. 여기서 소수란 '1.2345..'와 같이 매우 작은 수(小數)가 아니라 1과 자기 자신을 제외하고는 나누어 떨어지지 않는 수(素數, prime number)를 의미합니다. 예를 들어, 2, 3, 5, 7, 11, 13, 17 등이 있습니다. 정의한 것처럼 나열한 숫자들은 1과 자신만을 약수로 가집니다.

RSA는 두 개의 소수를 가지고 시작합니다. 실제 상황에서는 엄청나게 긴 소수 두 개를 가지고 연산하지만, 예를 들기 위해 작은 소수 2와 7을 가지고 설명하겠습니다. p, q를 각각 2, 7로 하겠습니다.

우선, 두 소수의 곱인 'N'을 연산합니다. 즉, $$N = p *q = 2* 7 = 14$$ 입니다.

그리고나서, 1부터 N까지의 수 중 N과 서로 소수 관계(coprime with N)인 것을 찾습니다. 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14 중 2는 14를 나눌 수 있으므로 제외합니다. 마찬가지로 2의 배수들도 14와 공약수(2)를 가지므로 제외합니다. 지금까지 남는 것은 1, 3, 5, 7, 9, 11, 13 입니다. 7도 14와 공약수를 가지므로 제외합니다. 결과적으로, 1, 3, 5, 9, 11, 13이 남습니다. 개수는 6개 입니다.

이 서로 소수 관계인 것들의 개수를 찾는 함수(φ)가 존재합니다. 그 함수는 '$$φ(n) = (p -1) *(q -1)$$'입니다. p, q로 약속한 2, 7을 대입해보면 1* 6 으로 6개라는 것을 알 수 있습니다.

이제 암호 키를 만들 차례입니다. 암호키 e는 두 가지 조건을 만족해야 합니다.

1. 1보다 크면서 φ(n)보다 작고 ( 1 < e < φ(n) )
2. N, φ(n)과 소수 관계여야 합니다.

φ(n)은 6이었습니다. 따라서, 첫 번째 조건은 어렵지 않습니다. 2, 3, 4, 5면 충분합니다. 14, 6과 소수 관계여야 한다는 두 번째 조건에 따르면 2의 배수는 제외됩니다. 3, 5가 남지만 3은 6과 공약수를 가지므로 제외됩니다. 따라서, e = 5 입니다.

암호키는 (e, N)으로 구성합니다. 즉, (5, 14) 입니다. 왜 두 가지가 필요한지, 각각이 무슨 역할인지는 '적용' 단락에서 확인하실 수 있습니다.

추가로, 이 암호키 (5, 14)가 '공개키 암호 방식'에서 공유할 '공개키'에 해당합니다.

이제 해독키를 만듭니다. 해독키 d는 e와의 곱 이후 φ(n)과 mod 연산이 1인 값으로 정합니다. 수식으로 쓰자면

$$d * e (modφ(n)) = 1$$이어야 합니다.

'mod' 연산은 나머지 연산입니다. 시계를 연상하시면 쉽습니다. 시간을 표기하는 것은 mod(12) 연산입니다. 1부터 11까지는 12로 나눈 나머지가 스스로이고, 13부터는 다시 1로 돌아갑니다. 다시 말해서, 1과 13은 mod(12) 연산에서 합동인 셈입니다.

원래 수식으로 돌아가서 이미 결정된 변수인 e, φ(n) 자리를 채워보면, $$d *5 (mod 6) = 1$$입니다. 이것을 만족하는 d를 설정해야 합니다. d를 1부터 차례로 증가시켜보면, d* 5는 5, 10, 15, 20, 25, 30, 35로 늘어납니다. 이를 6으로 나눈 나머지들은 각각 5, 4, 3, 2, 1, 0, 5가 됩니다. 처음으로 만족하는 25를 만드는 d 값인 5는 너무 작으므로 그냥 11로 정하겠습니다. 11도 5를 곱하면 55, 6으로 나눈 나머지가 1로 수식을 만족합니다.

해독키는 (d, N)으로 구성합니다. 따라서, 해독키는 (11, 14)입니다. 이것은 공개키 암호 방식에서 공유하면 안되는 '개인키'에 해당합니다.

### **적용**

암호키 (5, 14)로 텍스트 B를 암호화합니다. 컴퓨터가 알파벳 순서로 인식한다고 예를 들겠습니다. 그럼 'B = 2'가 됩니다.

변환된 2에 암호키 앞자리(5) 제곱을 한 후, 뒷자리(14)로 나머지 연산을 합니다. 즉, $$2^5 (mod 14)$$입니다. $$2^5$$은 32이므로 14로 나눈 나머지는 4입니다. 따라서 암호문은 'D'로 전송됩니다.

수신자가 해독을 시작합니다. 해독에 쓰이는 연산 방법은 동일합니다. 단지 쓰이는 숫자가 다를 뿐입니다. 해독키는 (11, 14)입니다. 'D = 4'를 받아 $$4^{11} mod 14$$를 연산합니다. $$4^{11}=4,194,304$$이고, 14로 나눈 나머지는 2 입니다. 즉, 'B'로 정상적인 해독에 성공하였습니다.

정리하면 아래 그림과 같습니다.

![Illustration by. Hackernoo)](/gpg-rsa.png)
*(image source: [How does RSA work?](https://medium.com/hackernoon/how-does-rsa-work-f44918df914b))*

### 비대칭 암호의 약점

하지만 비대칭 암호 혹은 공개키 암호에 약점이 전혀 없는 것은 아닙니다. 가장 유명한 것으로 중간자 공격(Man In The Middle attack; MITM attack)이 있습니다. A와 B가 통신을 원할 때, 이를 알게된 X가 중간에 본인의 공개키를 서로에게 상대방인 것처럼 전달합니다. A가 B로 발송한 메시지가 실제로는 B의 것으로 알려진 X의 공개키로 암호화되어 있고, 이를 X가 본인의 개인키를 이용해 해독, 내용을 파악한 이후에 실제 B의 공개키로 암호화하여 재발송하는 경우 B는 중간에 가로채진 내용이 있지는 않은지 확인할 수가 없습니다.

이것을 방지하고자 '서명' 방법이 사용됩니다. 즉, 암호화된 메시지의 해쉬값(메시지의 길이와 관계없이 특정한 길이로 반환되는 섞기 함수의 반환값) 자체를 발송자가 본인의 '개인키'로 암호화해서 '서명'으로써 함께 동봉합니다. 수신자는 발송자의 '서명'은 발송자의 공개키로, '메시지' 자체는 수신자 본인의 개인키로 해독합니다. 해독한 메시지를 같은 해쉬 함수를 이용해 산출되는 해쉬값과 비교하여 동일하면, 이는 발송자가 보낸 메시지임이 확실합니다. '서명'은 '개인키'로 암호화되고 '개인키'는 본인만 가지기 때문입니다.

사실, 서명에 쓰이는 공개키/개인키 알고리즘은, 눈치 채셨을 수도 있지만, 암호화와 동일한 것을 사용해도 무방합니다. 즉, RSA를 서명을 위해서도 활용하고 암호화를 위해서도 활용할 수 있죠. 다만, 하나는 발송자의 '개인키', 다른 하나는 수신자의 '공개키'를 활용하기만 하면 됩니다.

GPG를 더 자주 활용하는 사람들은 둘을 위한 키 각각을 생성해 사용하기도 합니다. GPG 소프트웨어는 이러한 사항을 위해 암호화에 사용될 키는 'E'(Encryption)로 서명에 사용될 키는 'S'(Signing)로 약자를 활용합니다. 추가로, 'A'는 '승인(Authentication)', 'C'는 '인증(Certification)'을 의미합니다.

### 키링

그렇다면, 고급 사용자의 경우 E/S/A/C 각각에 해당하는 키를 관리하고자 할 것입니다. GPG의 기본 명령을 활용하면 하나의 주요키(마스터키)가 생성되는 이유가 바로 이것입니다. 주요키 쌍인 공개키/개인키 아래에 각각의 역할을 할 서브키를 생성할 수 있습니다. 해당 서브키는 주요키 키링에 딸려오는 것이 됩니다.

정리하자면, '인증'기능만을 가질 주요키 한 쌍(공개키/개인키)을 생성합니다. 보통 이 주요키는 유효 기간을 두지 않습니다. '인증'기능을 갖는 만큼 타인이 이 키가 본인이 맞다는 것을 주기적으로 확인할 필요가 없도록 하기 위해서입니다. 다만, 그만큼 보안에 훨씬 신경을 써야할 것입니다. 이 주요키가 변형된다면 피해는 상상할 수 없습니다.

해당 주요키 키링에 E/S/A 각각을 수행할 서브키들을 만듭니다. 이 서브키들은 유효기간을 가지는 것이 보통입니다. 유효 기간이 지나면 새로운 키를 생성하여 보안을 유지합니다.

주요키와 서브키 총 4개는 각각 KEY-ID가 다릅니다.

GPG의 경우 서브키가 여러개일 경우, 연산에 필요한 것을 스스로 찾아서 동작합니다. 서명이 필요한 경우, 'S'가 부여된 키를 이용합니다. 또, 동일한 역할을 수행하는 서브키가 여러개일 경우, 가장 최근에 생성된 것을 사용합니다.

보통 'C' 연산인 인증을 위해 활용하는 주요키의 역할은 새로 가져온 상대방의 공개키를 인증할 때 씁니다. 내가 새로 공유받은 상대방의 공개키가 아래에서 확인할 '지문 확인' 등의 방법으로 상대방 본인의 것이 맞음을 확신할 때, 그 인증을 하기 위함입니다.

### 신뢰망

GPG와 같은 공개키 암호화 방식은 상호 간의 신뢰망(Web of Trust)을 통해 구축됩니다. A가 B라는 사람의 공개키를 본인이 실제로 지인이고 지문도 확인했다고 인증한 경우, A를 아는 C도 B라는 사람의 공개키를 신뢰할 수 있게 되는 것입니다. 이것은 바꾸어 말하면, 본인이 타인의 공개키를 인증할 때에 매우 신중을 기해야 함을 뜻합니다. 내가 제대로 확인하지 않고 인증한 공개키가 사실 그 사람의 것이 아님이 나중에 알려진 경우에는 본인의 공개키마저도 신뢰를 져버리게 됩니다.

### 키서버

공개키는 공유할 때 의미가 있습니다. 나에게 보내고자 하는 메시지를 암호화할 때 필수적으로 요구되기 때문입니다. 그런데, 나에게 처음 메시지를 보내는 사람마다 나에게 연락을 취해 공개키를 요구할 수는 없는 노릇입니다. 따라서, '키서버'가 존재합니다. GPG 프로그램을 이용해서 쉽게 키서버에 본인의 공개키를 공유할 수도 있고, 웹 브라우저를 통해 접속해서 등록할 수도 있습니다.

다만, 키서버의 기본 동작 원리를 이해해야만 합니다. 키서버에 한번 등록된 키는 '삭제'할 수 없습니다. 파기되었다는 정보를 추가하여 나중에 공개키를 검색하는 사람에게 이 키를 더이상 사용하고 있지 않음을 알릴 수는 있으나, 서버에서 키 자체를 삭제하는 기능은 없습니다. 이는, 여러 개의 키서버가 서로의 키서버 상에 있는 정보를 복사하여 저장하는 기본 동작 원칙에 의해서도 불편한 기능입니다. 한 서버에서 삭제한다고 해도, 금방 다른 서버의 목록과 대조하여 없는 것을 채우기 때문에 다시 추가될 것입니다.

따라서, 키서버에 키를 등록할 때에는 내가 정말 이것을 관리할 것인지 등을 잘 따져보시기 바랍니다.

마지막으로, 공개키에 사용자 정보를 넣는 것이 일반화 되어있다는 점을 이용해 스팸 메일 발송을 위한 이메일 주소 수집 목적으로도 자주 활용됩니다. 키서버에 본인의 공개키를 등록함에 따라, 추후에 상대적으로 더 잦은 스팸 메일을 받게 될 수도 있다는 점을 기억하시기 바랍니다.

### 지문

사람의 신원을 확인할 때 지문을 확인하는 이유는 단순합니다. 지문이 같은 사람이 없기([64조 분의 1](https://www.scientificamerican.com/article/the-chance-of-identical-fingerprints-1-in-64-trillion/))때문이지요. 공개키를 공유하고 나서도 이 키가 상대방 것이 맞는지 확인하기 위한 방법으로 지문을 활용합니다. 물론, '키'의 지문을 사용합니다. 여기서의 지문은 키 ID로도 활용합니다. 일반적으로, 키 ID를 필요로할 때는 공백이 없이, 지문을 확인할 때는 4자리(1바이트)씩 끊어서 활용합니다.

공개키를 공유 받은 후, 상대방과 통화, 메신저 등 본인임을 확신할 수 있는 방법으로 지문을 한글자씩 대조하여 확인하실 것을 당부드립니다.

## GPG: 기본

> **PGP? GPG? OpenPGP?**
>
> PGP는 암호화 제품을 판매하는 회사로 발전하였습니다. 1991년 처음 쓰인 공개 키 암호화 방법인 PGP를 기반으로 암호화 관련 제품을 판매하는 상업 회사입니다. GPG는 GnuPG로, 각종 암호화 도구들을 무료로 배포, 활용할 수 있는 소프트웨어입니다. 다만, GNU의 유틸리티가 모두 그러하듯이, 라이선스가 매우 공격적입니다. 따라서, 상업적으로 판매할 제품에 GPG를 활용한다면, GPL(그누 공개 라이센스)를 사용해야만 합니다. GPL은 해당 라이센스가 붙은 제품을 활용하여 부차적인 제품을 제작하는 경우, 마찬가지로 GPL을 사용하도록 하고 있습니다. 소스 코드를 반드시 공개할 것을 강제하는 라이센스입니다. OpenPGP란 PGP, GPG를 아울러 공개키 암호화 알고리즘을 칭하는 말입니다.
>
> PGP 제품을 GPG라고 하면 화낼 것이고, GPG 제품을 PGP라고 하면 자유 소프트웨어 진영에서 화를 내겠지만 OpenPGP라고 하면 양쪽 다 적당히 수긍합니다.

> **설명을 돕기 위한 허술한 비유**
>
> 주요키, 특히 **주요키의 지문**은 본인이 초등학생 때부터 사용했고 앞으로도 변경할 의사가 전혀 없는 네이버 아이디라고 생각하시면 됩니다. 학교 생활을 마치고 대학 생활 중에도, 직장 생활 중에도 사용하던 '아무개13579'는 이제 본인의 분신과도 같습니다.
>
> 자 이때, 트위터에 관심이 생겨 계정을 하나 만들었습니다. 트위터 계정 '아무개111'에서 아무리 본인이라고 알려도 의심이 많은 지인들은 수상할 수 밖에 없습니다. 하지만, 네이버 계정으로 "트위터 '아무개111'이라고 계정 만들어봄 ㅋㅋ"라는 메일을 연락처에 모두 전송하면, 해당 메일을 받은 사람들은 '아무개13579'로부터 수신한 이메일을 통해 '아무개111'도 당신이라는 것을 확신할 수 있습니다.
>
> 여기서 트위터 계정 '아무개111'이 서브키 입니다. 서브키는 새로운 것을 만들수도, 원래 것을 삭제할 수도, 원래 것의 유효 기간을 늘릴 수도 있지만, 그 작업이 본인으로부터 이루어졌음을 주변에서 확신하기 위해서는 '주요키'가 잘 보관되어야 가능합니다.

### 키 생성

그럼 실제로 GPG 키를 만들어보겠습니다. (`$ gpg --version` 으로 gpg 출력이 없는 경우 설치가 되어 있지 않은 것입니다. 맥 에서는 `$brew install gnupg`, 리눅스에서는 본인의 패키지 매니저를 이용해 `#emerge gpg` 등을 통해 설치할 수 있습니다. `$ gpg --help` 를 통해 다양한 옵션을 확인할 수 있습니다.)

```shell:
gpg --full-generate-key
```

아래와 같은 출력이 나옵니다.

```shell:
gpg (GnuPG) 2.2.27; Copyright (C) 2021 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
  (1) RSA and RSA (default)
  (2) DSA and Elgamal
  (3) DSA (sign only)
  (4) RSA (sign only)
 (14) Existing key from card
Your selection?
```

해석해보면

```shell:
gpg (GnuPG) 버전; 저작권
자유 소프트웨어입니다: 변형하거나 재배포할 수 있습니다.
법적으로 허용된 이상의 어떤 보증도 하지 않습니다.

어떤 종류의 키를 원하는지 선택하십시오:
  (1) RSA 와 RSA (기본)
  (2) DSA 와 Elgamal
  (3) DSA (서명만)
  (4) RSA (서명만)
 (14) 카드에 존재하는 키
선택?
```

3번이나 4번 선택지에서 제공하는 '서명만'의 의미는 '더 자세히' 단락에서 설명하겠습니다. 여기서는 이해를 위해 기본값인 1번을 선택하겠습니다. 1을 입력하고 Enter.

추가로 2 줄의 출력이 나옵니다.

```shell:
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
```

RSA 키의 길이를 묻습니다. 설명을 위해 4096을 입력하고 Enter.

> **RSA 2048? 3072? 4096?**
>
> RSA 키 사이즈를 2048비트로 해야할까요? 아니면 깃허브, 깃랩의 gpg 키 추가 도움말 페이지에서 추천하듯 4096비트로 해야할까요? [GPG 자주 묻는 질문](https://gnupg.org/faq/gnupg-faq.html#default_rsa2048)의 11.2 항목부터 그 아래 11.3, 11.4 에서 상세히 설명하고 있듯이, 4096은 약간의 보안을 증대시켜 주지만 연산 속도, 파일 크기 측면에서의 비용 증가와 비교했을 때, 무의미합니다. 즉, 보안적 측면에서 우려가 되어 4096을 쓰겠다는 사람들은 차라리 ECC 알고리즘을 고려하라고 하고 있습니다.

추가 출력이 나옵니다.

```shell:
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0)
```

첫 줄을 통해 입력 키 사이즈가 4096 비트라는 것을 확인합니다.

그 아래부터는 이 키가 유효한 기간을 설정합니다. 여기서는 학습을 하는 중이므로, 7를 입력해 7일동안만 유효하도록 합니다. 0을 입력하면 파기되지 않고, 숫자만 입력한 경우 해당 숫자'일' 동안, w는 '주', m은 '개월', y는 '년'동안 유지됩니다.

> 연습을 하는 동안에는 **파기 기한이 없는 키는 제작하지 않기를 권장**합니다. 즉, 0만을 입력하지 마십시오. 추후, 파기용 키를 잃어버리거나, 이미 키서버에 업로드한 키에 접근 방법이 없어지는 경우, 키를 제거할 수 없습니다. 따라서, 충분한 이해를 하기 전에는 가급적 짧은 시간 동안만 유효한 키를 만들기를 권장합니다.

추가 출력은 설정한 7일이 지난 시점에 파기되는 정확한 날짜와 시간이 표기됩니다.

```shell:
Key is valid for? (0) 7
Key expires at Mon 13 Sep 2021 11:56:46 AM KST
Is this correct? (y/N)
```

`y`를 입력합니다.

추가 출력은 개인 정보를 입력하기 위함입니다.

```shell:
Real name: WooHyung Jeon
Email address: jeon@ptrtoj.com
Comment:
You selected this USER-ID:
    "WooHyung Jeon <jeon@ptrtoj.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit?
```

실명과 이메일 주소를 입력하고, 코멘트 부분에는 아무것도 입력하지 않습니다. 마지막에 `O`를 입력해 완료합니다.

비밀번호를 입력하는 창이 나옵니다. 이 비밀번호가 공개키 암호화 기법에서 가장 취약한 부분에 해당합니다. 따라서, 신중히 어려운 비밀번호를 선택하시기 바랍니다.

이제 아래 명령어를 통해 생성된 키를 확인할 수 있습니다.

일반적인 옵션인 `--list-keys`, `--list-secret-keys` 등은 마지막 `keys`를 `key`로 입력해도 작동합니다.

> **Password? Passphrase?**
>
> 국내에서는 일반적으로 '비밀번호'로 통용되지만, 엄밀히 '비밀번호'는 각각 '비밀 단어'와 '비밀 구절'로 구분할 수 있습니다. 우리가 평소 사용하는 것은 '비밀단어'에 해당하고 '비밀구절'은 빈 칸(스페이스바)를 허용하고 그 길이도 문장~단락 수준까지 허용됩니다.

뿐만 아니라 `--list-keys`의 경우 `-k` 옵션으로 줄여서 사용 가능합니다.

```shell:
gpg --list-keys
```

여기서 출력되는 결과는 모두 공개키입니다. 즉, 다른 사람과 공유해도 되는 것들입니다.

키가 저장된 기본 설정 디렉터리인 `/home/(유저이름)/.gnupg/pubring.kbx`가 출력됩니다. (`.gpg` 확장자는 이제 사용되지 않습니다. `.kbx`(키박스)가 아닌 파일은 별도의 검색을 통해 `.kbx`로 변환할 수 있습니다.)

그 아래에 `pub`는 'public(공개)'키임을 표현합니다.

이어서 사용된 알고리즘과 키 길이가 등장합니다(`rsa4096`).

제작된 날짜가 나오고 마지막에 알파벳이 나옵니다.

길게 숫자와 알파벳으로 이어지는 것이 키 ID 입니다. 키 지문/긴 형식 ID/짧은 형식 ID는 모두 이 키 ID의 일부입니다. `$ gpg --list-key --keyid-format long`과 `$ gpg --list-key --keyid-format short`를 보시면 각 16자리, 8자리로 줄어든 것을 확인할 수 있습니다.

그리고 `sub`는 서브키의 공개키 부분입니다. 이 서브키는 GPG가 키를 생성할 때, 암호화만을 이용하기 위해 기본적으로 추가 생성됩니다. 우측의 '`[E]`'를 통해 암호화용 키라는 것을 확인할 수 있습니다.

개인키는 `--list-keys`와 유사하지만 대문자 `-K` 옵션으로 줄일 수 있습니다.

```shell:
gpg --list-secret-keys
```

이번엔 개인키의 목록을 보겠습니다.

주요키의 개인 부분은 '`sec`'로 표기되고, 서브키는 '`ssb`'로 표기됩니다. '`ssb`'는 '**s**ecret **s**u**b**key'의 줄임으로 알려져있습니다.

### 지문 확인

내 키, 혹은 상대방의 공유받은 키 지문을 확인하기 위해서 아래의 명령을 활용합니다.

```shell:
gpg --fingerprint your@email.address
```

### 키 가져오기

키서버에서 키를 가져오는 방법입니다. 지인의 이메일 계정 등을 알 때 키서버에서 해당 이메일을 검색하여 공개키를 가져올 수 있습니다.

```shell:
gpg --keyserver pgp.mit.edu --recv OTHERS_KEY_ID
```

타인의 공유 받은 공개키를 인증(키 서명)하고자 하는 경우 아래의 명령을 활용합니다.

```shell:
gpg --sign-key someone@email.address
```

다음은 별도의 USB등에 저장해 두었던 본인의 개인키를 가져오는 방법입니다. 이 방법은 손상된 키링을 복구할 때 뿐만 아니라 본인의 다른 기기에 같은 키를 저장하기 위해서도 유용합니다.

```shell:
gpg --import ./my-secret-gpg-key.asc
```

### 키 내보내기

개인키의 경우 절대로 노출되어서는 안되기 때문에, USB 등에 옮겨 두는 사용자가 많습니다. 다른 무엇보다도 **절대로 네트워크에 노출되지 않도록 주의**하시기 바랍니다. 즉, 웹 브라우저, 클라우드 저장소 등에 절대로 옮기면 안됩니다. 네트워크를 통하는 순간 항시 패킷을 확인하고 있는 맬웨어(악성 프로그램) 등에는 노출된다고 봐도 무방합니다.

GPG의 경우, 일반 키 출력값은 사람이 읽을 수 없는 쓰레기 값으로 보입니다. 따라서, 사람이 읽기 쉽도록 출력하는 `--armor` 옵션을 추가합니다(`-a`로 줄여 사용 가능). 이로 출력된 값을 파일에 추가할 때는 나중에 알아보기 편하게 하기 위해서 'ascii'의 줄임인 '`asc`' 확장자로 보통 저장합니다.

```shell:
gpg --export-secret-keys --armor YOUR_KEY_ID > ./my-secret-gpg-key.asc
```

작업 환경 디렉터리를 보시면 `my-secret-gpg-key.asc`파일이 생성된 것을 확인할 수 있습니다. 이 파일을 절대 네트워크에 노출하지 않고 본인의 안전한 환경에 저장하시기 바랍니다.

별도로 `paperkey` 등의 소프트웨어를 활용해 출력에 용이하게 만들 수 있습니다.

### 파기용 키 제작

키가 위험한 환경에 노출되었음을 나중에 알게 되거나, 아예 본인의 키를 잃어버린 경우, 기존 키를 효과적으로 파기하기 위해서 파기용 키가 필요합니다. 이를 위해 생성된 파일 역시, 위에서 마련한 안전한 개인키 저장 공간에 함께 보관하시길 권장합니다. 작업 디렉터리 안에 `my-revoc-key.asc` 파일이 생성됩니다.

> **GnuPG 2.1 Update**
>
> 2017년 7월 출시 GPG 2.1 버전부터 [키 생성 시 파기용 키를 자동 생성](https://gnupg.org/faq/whats-new-in-2.1.html#autorev)합니다. 이 파기용 키는 `openpgp-revocsc.d` 디렉터리 안에 저장됩니다. 따라서, 앞으로 파기용 키 제작 과정은 강한 권장 사항에서 선택 사항으로 바뀔 것 같습니다.

```shell:
gpg --output my-revoc-key.asc --gen-revoke YOUR_KEY_ID
```

### 키 삭제

키의 파기가 아니라 기기에서 키를 삭제하기 위한 방법입니다. 모든 키를 삭제하기 위함이라도 반드시 개인키 먼저 삭제해야 합니다.

```shell:
gpg --delete-secret-keys YOUR_KEY_ID
```

다음은, 공개키 삭제입니다.

```shell:
gpg --delete-keys YOUR_KEY_ID
```

### 키서버로 내보내기

>
> 앞에서 설명했듯이 공개키가 서버에 등록되고 나서는 파기 표시를 하는 것 외에 '삭제'를 하는 방법은 없습니다. 내용을 충분히 이해하고 진행하시기 바랍니다.

키서버에 공개키를 내보내는 방법입니다.

```shell:
gpg --keyserver hkp://pool.sks-keyservers.net --send-key YOUR_KEY_ID
```

키서버의 종류는 다양하니 별도로 검색해보셔도 무방합니다. 다만, 어차피 키서버가 서로를 확인하여 같은 상태로 유지하기 때문에 어디에 업로드하더라도 결과는 같아집니다.

### 암호화

### **대칭 암호화**

GPG 키로 반드시 비대칭 암호화만 할 수 있는 것은 아닙니다. 대칭 암호화를 공유하는 비밀번호로 하는 방법은 아래와 같습니다.

```shell:
gpg --symmetric message.txt
```

이후, 출력에서 비밀번호를 입력합니다.

- `--cipher-algo` 옵션을 통해 다른 알고리즘을 활용할 수 있습니다.
- `--sign` 옵션을 통해, 서명을 할 수도 있습니다.

### **비대칭 암호화**

원래 목적인 비대칭 암호화 방법입니다. `--encrypt` 옵션은 `-e`로 줄여 사용할 수 있습니다.

```shell:
gpg --encrypt message.txt
```

- `--recipient` 옵션을 통해 받는 사람의 이메일 주소를 명시할 수 있습니다.
- `--sign` 옵션을 여기서도 활용할 수 있습니다.

### 해독

암호화된 문서를 수신하고 해독하는 방법입니다.이 작업은 서명을 해독하는데에도 똑같이 적용됩니다.`--decrypt` 옵션은 `-d`로 줄여 사용할 수 있습니다.

```shell:
gpg --decrypt message.txt.gpg > decypted.txt
```

### 서명

```shell:
gpg --sign message.txt
```

다른 작업과 마찬가지로 `--symmetric`, `--encrypt`, `--recipient` 등의 옵션을 활용할 수 있습니다.

서명의 해독만을 위해서는 `--decrypt` 옵션을 활용할 수 있지만 해독을 넘어 확인을 한 번에 처리하고자 하는 경우 `--verify` 옵션을 쓸 수 있습니다.

## GPG: 더 자세히

아래는 E/S/A/C 키를 각각 관리하고자 하는 분들을 위한 명령어들을 소개합니다.

참조하시면 좋은 [데비안 서브키 문서](https://wiki.debian.org/Subkeys)입니다. 아래 과정은 [Yubikey 가이드](https://github.com/drduh/YubiKey-Guide)를 참조하였습니다.

### 주요키 생성

키를 생성할 때 `--expert`옵션을 함께 줍니다. 주요키(마스터키)는 굳이 유효기간을 설정하지 않는 것이 일반적입니다. 평소에 대부분의 작업으로 서브키를 이용하기 때문입니다. 주요키는 지문만 이용하더라도 본인이라는 것을 알릴 수 있을 정도로 오랜 기간 메인으로 활용합니다. 따라서 관리에 더 주의해야 합니다.

```shell:
gpg --expert --full-generate-key
```

```shell:
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
   (9) ECC and ECC
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (13) Existing key
Your selection?
```

(8)을 입력하여 RSA를 활용하되 기능은 직접 선택하는 것으로 알립니다.

`expert` 옵션이 부여된 경우 작동 방식은 토글입니다. 결과 출력값들 중에서 입력한 기능이 미포함되어 있다면 추가되고, 이미 포함되어 있다면 제거됩니다. 아래 예를 직접 확인하겠습니다.

```shell:
Your selection? 8

Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Sign Certify Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection?
```

'`Current allowed actions`'를 참조하시면 기본값으로 `Sign, Certify, Encrypt`가 포함되어 있습니다. 처음 만드는 주요키는 Certification 기능만을 줄 것이므로, Sign과 Encrypt는 제거해야 합니다. 우선, `(E)`를 입력하여 Encrypt를 제거합니다.

```shell:
Your selection? E

Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Sign Certify

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection?
```

Encrypt가 없어진 것을 확인할 수 있습니다. 이제`(S)`를 입력하여 Sign도 제거합니다.

```shell:
Your selection? S

Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Certify

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection?
```

`(Q)`를 입력해 완료합니다.

```shell:
Your selection? Q
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y
```

이후 과정은 `expert` 옵션이 없는 것과 동일하게 이루어집니다. 개인 정보를 입력해 줍니다.

```shell:
GnuPG needs to construct a user ID to identify your key.

Real name: Dr Duh
Email address: doc@duh.to
Comment: [옵션 사항 - 빈 칸으로 두기 가능]
You selected this USER-ID:
    "Dr Duh <doc@duh.to>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o

We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

gpg: /tmp.FLZC0xcM/trustdb.gpg: trustdb created
gpg: key 0xFF3E7D88647EBCDB marked as ultimately trusted
gpg: directory '/tmp.FLZC0xcM/openpgp-revocs.d' created
gpg: revocation certificate stored as '/tmp.FLZC0xcM/openpgp-revocs.d/011CE16BD45B27A55BA8776DFF3E7D88647EBCDB.rev'
public and secret key created and signed.

pub   rsa4096/0xFF3E7D88647EBCDB 2017-10-09 [C]
      Key fingerprint = 011C E16B D45B 27A5 5BA8  776D FF3E 7D88 647E BCDB
uid                              Dr Duh <doc@duh.to>
```

### 서브키 생성

이제 S/E/A 각각의 키를 생성합니다. 서브키는 유효기간을 생성하는 경우가 일반적입니다.

이미 생성한 키의 변경이나 수정을 위해서는 아래의 명령어를 활용합니다.

```shell:
gpg --expert --edit-key YOUR_KEY_ID
```

### **서명용 서브키 생성**

서명용과 암호화용 서브키 모두 별도의 옵션으로서 제공됩니다. 아래 예시에서 어떤 종류의 키를 원하는지 묻는 앞 부분을 보시면 `(4)`옵션에 서명만을 위한 RSA와 `(6)` 옵션에 암호화만을 위한 RSA 옵션이 있습니다. 각 단계에서 필요한 것을 입력하면 됩니다.

```shell:
gpg> addkey
Key is protected.

You need a passphrase to unlock the secret key for
user: "Dr Duh <doc@duh.to>"
4096-bit RSA key, ID 0xFF3E7D88647EBCDB, created 2016-05-24

Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
Your selection? 4
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at Mon 10 Sep 2018 00:00:00 PM UTC
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  rsa4096/0xFF3E7D88647EBCDB
    created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: 2018-10-09       usage: S
[ultimate] (1). Dr Duh <doc@duh.to>
```

출력된 마지막 내용을 유심히 보시면, '`sec`'에 해당하는 주요키+개인키 하나가 `사용처: C`로 생성되었고, 그 아래에 '`ssb`'에 해당하는 서브키+개인키 하나가 `사용처: S`로 생성되었습니다.

### **암호화용 서브키 생성**

```shell:
gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
Your selection? 6
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at Mon 10 Sep 2018 00:00:00 PM UTC
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  rsa4096/0xFF3E7D88647EBCDB
    created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: 2018-10-09       usage: S
ssb  rsa4096/0x5912A795E90DD2CF
    created: 2017-10-09  expires: 2018-10-09       usage: E
[ultimate] (1). Dr Duh <doc@duh.to>
```

출력된 마지막 내용을 유심히 보시면, 기존에 있던 두 개 아래에 '`ssb`'에 해당하는 서브키+개인키 하나가 `사용처: E`로 생성되었습니다.

### **승인용 서브키 생성**

승인용 키는 조금 다릅니다. GPG 유틸리티가 암호화 기능을 기본 제공하지 않기 때문에 마치 `--expert` 옵션을 통해 새 키를 만들 때 처럼 접근하여 암호화용 기능만 켜주고 나머지는 모두 꺼 주어야 합니다.

```shell:
gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
  (10) ECC (sign only)
  (11) ECC (set your own capabilities)
  (12) ECC (encrypt only)
  (13) Existing key
Your selection? 8

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Sign Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? S

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? E

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions:

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? A

Possible actions for a RSA key: Sign Encrypt Authenticate
Current allowed actions: Authenticate

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? Q
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at Mon 10 Sep 2018 00:00:00 PM UTC
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  rsa4096/0xFF3E7D88647EBCDB
    created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: 2018-10-09       usage: S
ssb  rsa4096/0x5912A795E90DD2CF
    created: 2017-10-09  expires: 2018-10-09       usage: E
ssb  rsa4096/0x3F29127E79649A3D
    created: 2017-10-09  expires: 2018-10-09       usage: A
[ultimate] (1). Dr Duh <doc@duh.to>
```

출력된 마지막 부분에 C(인증)를 위한 주요키+개인키 하나와 S/E/A 각각의 기능을 위한 서브키+개인키(ssb)가 생성되었습니다.

### 개인 정보 추가

앞에서 서브키 추가할 때 한 것처럼 `--edit-key` 옵션을 통해 프롬프트에 진입합니다.

`$ gpg --expert --edit-key YOUR_KEY_ID`

프롬프트에 `adduid` 를 입력해 추가 정보 입력으로 들어갑니다.

아래 예시에서는 추가 이메일 주소를 입력하고 승인합니다.

```shell:
gpg> adduid
Real name: Dr Duh
Email address: DrDuh@other.org
Comment:
You selected this USER-ID:
    "Dr Duh <DrDuh@other.org>"

sec  rsa4096/0xFF3E7D88647EBCDB
    created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: never       usage: S
ssb  rsa4096/0x5912A795E90DD2CF
    created: 2017-10-09  expires: never       usage: E
ssb  rsa4096/0x3F29127E79649A3D
    created: 2017-10-09  expires: never       usage: A
[ultimate] (1). Dr Duh <doc@duh.to>
[ unknown] (2). Dr Duh <DrDuh@other.org>

gpg> trust
sec  rsa4096/0xFF3E7D88647EBCDB
    created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: never       usage: S
ssb  rsa4096/0x5912A795E90DD2CF
    created: 2017-10-09  expires: never       usage: E
ssb  rsa4096/0x3F29127E79649A3D
    created: 2017-10-09  expires: never       usage: A
[ultimate] (1). Dr Duh <doc@duh.to>
[ unknown] (2). Dr Duh <DrDuh@other.org>

Please decide how far you trust this user to correctly verify other users' keys
(by looking at passports, checking fingerprints from different sources, etc.)

  1 = I don't know or won't say
  2 = I do NOT trust
  3 = I trust marginally
  4 = I trust fully
  5 = I trust ultimately
  m = back to the main menu

Your decision? 5
Do you really want to set this key to ultimate trust? (y/N) y

sec  rsa4096/0xFF3E7D88647EBCDB
    created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: never       usage: S
ssb  rsa4096/0x5912A795E90DD2CF
    created: 2017-10-09  expires: never       usage: E
ssb  rsa4096/0x3F29127E79649A3D
    created: 2017-10-09  expires: never       usage: A
[ultimate] (1). Dr Duh <doc@duh.to>
[ unknown] (2). Dr Duh <DrDuh@other.org>

gpg> uid 1

sec  rsa4096/0xFF3E7D88647EBCDB
created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: never       usage: S
ssb  rsa4096/0x5912A795E90DD2CF
    created: 2017-10-09  expires: never       usage: E
ssb  rsa4096/0x3F29127E79649A3D
    created: 2017-10-09  expires: never       usage: A
[ultimate] (1)* Dr Duh <doc@duh.to>
[ unknown] (2). Dr Duh <DrDuh@other.org>

gpg> primary

sec  rsa4096/0xFF3E7D88647EBCDB
created: 2017-10-09  expires: never       usage: C
    trust: ultimate      validity: ultimate
ssb  rsa4096/0xBECFA3C1AE191D15
    created: 2017-10-09  expires: never       usage: S
ssb  rsa4096/0x5912A795E90DD2CF
    created: 2017-10-09  expires: never       usage: E
ssb  rsa4096/0x3F29127E79649A3D
    created: 2017-10-09  expires: never       usage: A
[ultimate] (1)* Dr Duh <doc@duh.to>
[ unknown] (2)  Dr Duh <DrDuh@other.org>

gpg> save
```

### 주요키에서 개인키 제거

주요키의 개인키를 내보내두고, 서브키만을 활용하기 위해 기기에서 주요키의 개인키를 삭제합니다. 이를 위해서 주요 개인키의 키그립 ID를 확인한 이후 아래의 명령을 수행합니다.

> **Warning**
>
> 반드시 파기용 키와 개인키들을 안전한 곳에 보관한 이후 수행하시기 바랍니다. 아예 USB에 `$HOME/.gnupg`디렉터리 자체를 복사해 두는 것도 좋은 방법입니다.
>
> 네트워크에 노출이 안되도록 반드시 주의해주세요. 클라우드 저장소 등을 많이 쓰게 되면서 실수로 클라우드 등에 올리는 실수를 자주 확인합니다.

아래에서 쓰일 KEY_GRIP_ID는 다음을 입력해 얻을 수 있습니다.

```shell:
gpg --with-keygrip --list-key your@email.address
```

출력 중에서 주요키에 해당하는 Keygrip 값을 복사하고 아래 명령을 수행합니다.

```shell:
rm -i $HOME/.gnupg/private-keys-v1.d/KEY_GRIP_ID.key
```

이후, `$ gpg -K`를 통해 주요 개인키 부분에 표기되는 `sec` 옆에 `#`에 추가되었는지 확인합니다. 이 `#`이 해당 값에 해당하는 개인키가 실제로 존재하지 않음을 의미합니다.

혹시, 기본 GPG 디렉터리(보통`$HOME/.gnupg`) 아래에 `secring.gpg` 디렉터리가 있다면 그 디렉터리도 삭제해줍니다.

### USB의 주요키로 작업

USB에 복사한 `.gnupg`디렉리의 주요+개인키를 이용해 작업이 필요한 경우 (예를 들어, 새로 들여오기한 지인의 공개키를 승인하는 경우 등) 아래를 통해 작업합니다. 우선, USB를 마운트한 이후,

```shell:
export GNUPGHOME=/media/USB_MOUNT_POINT
gpg -K
```

혹은, 환경값 변경 필요 없이 아래와 같이 작업하는 방법도 있습니다.

```shell:
gpg --homedir=/media/USB_MOUNT_POINT -K
```

### 서브키 비밀번호 변경

데비안 문서는 여기서 한 걸음 더 나아갑니다. 주요키의 개인키를 삭제하고 서브키의 비밀번호를 변경합니다. 혹시 비밀번호 자체가 변형되어도 주요키의 비밀번호는 살아남도록 계획합니다. 원하는 분은 이 과정까지 진행하시면 됩니다.

```shell:
gpg --edit-key PRIMARY_KEY_ID passwd
```

## 문제 발생 시

>
> 이 방법들은
> (1) **주요+개인 키가 USB 등 안전한 매체에 저장**되어 있고,
> (2) **이는 공격받지 않았음**,
> (3) 마지막으로 이미 **키서버에 키가 업로드 되어있음**을 전제로 합니다.

USB를 마운트 합니다.

```shell:
mount /dev/USB_DEVICE_PARTITION /mnt/usb
```

USB나 본인의 개인키가 저장된 디렉터리를 임시 홈 환경으로 지정합니다.

```shell:
export GNUPGHOME=/mnt/usb/DIRECTORY
```

키 수정 환경으로 진입합니다.

```shell:
gpg --edit-key YOUR_KEY_ID
```

GPG 프롬프트에서 목록을 확인 후 해당 하는 키를 선택합니다. 파기 인증을 생성하고 저장합니다.

```shell:
gpg> list
...(list)...
gpg> key 123
...(select key)...
gpg> revkey
```

업데이트 된 키 목록을 키서버에 업로드합니다.

## 키서버 재검토

위에서 초보자의 실수를 방지하고자 키서버를 아주 위험하고 악랄한 것처럼 묘사하였습니다만, 사실 이는 오해의 소지가 있습니다. 키서버 중에는 외부와 대조/복사를 하지 않는 독립망도 있고, 최근에는 수신된 메일의 링크를 클릭해 본인임을 인증하는 사실상의 가입 단계를 거치는 서버도 존재합니다.

필자는 [keys.openpgp.org](https://keys.openpgp.org/)를 사용하고 있습니다. (1) 공개키를 등록하면, 이메일 주소로 승인 확인용 링크가 전송됩니다. 링크를 클릭해야 정상 등록됩니다. (2) [자주 묻는 질문](https://keys.openpgp.org/about/faq#sks-pool)에 의하면 SKS 풀과 독립적으로 작동하며 앞으로도 일부가 될 계획이 없다고 합니다.

여전히 등록을 원하시는 경우, 아래 명령어를 활용하시면 되겠습니다.

```shell:
gpgpg --export-options export-minimal --export YOUR_KEY_ID | curl -T - https://keys.openpgp.org
```

## 참조

- 마이클 루카스(Michael W. Lucas): [PGP & GPG: Email for the Practical Paranoid](https://www.amazon.com/PGP-GPG-Email-Practical-Paranoid/dp/1593270712/) (ISBN: 1593270712)
- [RFC 4880 - OpenPGP Message Format](https://datatracker.ietf.org/doc/html/rfc4880)
- 젠투 리눅스 개발자 정책인 GLEP의 63번째 항목 - OpenPGP 키 제원 섹션에서 최소 요구 사항과 권장 사항을 번역하겠습니다. ([GLEP 63: Gentoo OpenPGP policies](https://www.gentoo.org/glep/glep-0063.html))
  - 최소 요구 사항
        1. 아웃풋 다이제스트는 SHA-2 일 것 (SHA-1 다이제스트도 내부적으로 허용됨), 최소 256 비트여야 함. 모든 서브키는 반드시 이 다이제스트를 이용해 셀프-서명 되어야 함.
        2. 서명용 서브키는 주요키와 달라야만 하고, 다른 어떠한 기능도 부여되어서는 안됨
        3. 주요키와 서명용 서브키는 둘 모두 아래의 타입 중 한가지여야 함:
            1. RSA, >=2048 bits (OpenPGP v4 키 양식이나 더 최근 버전),
            2. ECC 25519.
        4. 모든 서브키의 유효 기간은 900일 이상 남지 않았을 것
        5. 키 유효 기간은 마지막 유효 기간으로부터 최소 2주 전에는 갱신되어야 함.
        6. 키에는 @gentoo.org를 사용하는 이메일 주소가 UID로 포함되어야 함.
        7. [권장] 키는 반드시 젠투 키서버에 업로드 될 것
        8. 주요키는 Certify(인증) 기능만 설정되어 있을 것
        9. 주요키와 서명용 서브키는 모두 RSA, 2048 bits여야함. (OpenPGP v4 key 양식이나 더 최근 버전).
        10. 키 유효 기간은 매해의 일정한 날짜로 갱신될 것
        11. 파기 인증서를 제작하고 안전한 곳에 물리적인 형태로 보관할 것( ~300 바이트 정도 크기일 것임)
        12. 개인키의 암호화된 백업이 존재할 것
        13. SKS나 다른 키서버에 업로드할 것