---
title: "헷갈리는 Git 용어"
date: 2023-11-03
url: 'git'
---

---

This is a translated work.
The original post was written by Julia Evans and you can read it [here](https://jvns.ca/blog/2023/11/01/confusing-git-terminology/). On 03, Nov. 2023, permission was granted via e-mail.

---

이 문서는 번역본입니다.
원본은 줄리아 에반스(Julia Evans)에 의해 작성되었으며, 다음 [링크](https://jvns.ca/blog/2023/11/01/confusing-git-terminology/)를 통해 읽을 수 있습니다.이메일을 통해 23/11/03 번역 허가를 받아 작성 후 업로드 합니다.

---

> 주요 개념, 명령어나 명령어 출력은 독자의 오해를 방지하기 위해 한국어와 영어를 병기합니다
>
> Important concepts, commands or outputs are written in both Korean AND English, so readers can avoid misunderstanding

---

안녕하세요! 저는 Git을 설명하기 위한 작업을 진행 중입니다.
15년간 Git을 사용하는 과정에서 생긴 제게 가장 큰 문제라면,
Git의 특이한 점에 대해 너무 익숙해져 어떤 점들이 헷갈리는지 잊기 쉽다는 점입니다.

따라서 저는 [Mastodond](https://social.jvns.ca/@b0rk/111330564535454510)을 통해 사람들에게 질문을 남겼습니다:

> 어떤 Git 용어들이 헷갈리나요? Git의 요상한 용어 사용에 대한 블로그 포스트를 작성할까 생각 중입니다:
> "떼어진 HEAD 상태(detached HEAD state)", "패스트-포워드(fast-forward)", "인덱스/스테이징 구역/스테이지됨(index/staging area/staged)", "'원격/메인'보다 1 커밋 앞서있음(ahead of ‘origin/main’ by 1 commit)", 등

저는 많은 훌륭한 답들을 받을 수 있었으며 일부를 여기에 요약하고자 합니다.
요약하는 용어들은 아래와 같습니다:

- HEAD와 "헤드들(heads)"
- "떨어진 HEAD 상태(detached HEAD state)"
- 머지(merge), 리베이스(rebase) 과정에서 "우리쪽(ours)"과 "저쪽(theirs)"
- "당신의 브랜치는 '원격/메인' 최신 상태입니다(Your branch is up to date with ‘origin/main’)"
- HEAD^, HEAD~ HEAD^^, HEAD~~, HEAD^2, HEAD~2
- .. 와 ...
- "패스트 포워드 가능(can be fast-forwarded)"
- "레퍼런스(reference)", "심볼릭 레퍼런스(symbolic reference)"
- refspecs
- "tree-ish"
- "인덱스(index)", "스테이지됨(staged)", "캐시됨(cached)"
- "리셋(reset)", "되돌림(revert)", "복원(restore)"
- "트랙되지 않는 파일(untracked files)", "원격-트래킹 브랜치(remote-tracking branch)", "원격 브랜치 트래킹하기(track remote branch)"
- 체크아웃(checkout)
- reflog
- 머지(merge) vs 리베이스(rebase) vs 체리픽(cherry-pick)
- rebase -onto
- 커밋(commit)
- 추가 헷갈리는 용어들

이런 용어들을 설명하고자 최선을 다했으나 사실상 이 용어들이 각각 Git의 주요 기능들에 해당하다보니 하나의 블로그 포스트에 담기에는 너무 양이 많아 일부 설명은 간략할 수 있습니다.

### HEAD와 “헤드들(heads)”

적지 않은 사람들이 `HEAD`라든지 `refs/heads/main`과 같은 용어를 혼란스럽다고 했습니다.
이런 용어들이 너무 복잡하고 기술적인 내부 작업을 표현하는 것처럼 들리기 때문인데요.

핵심 요약은 이렇습니다:
- "헤드들(heads)"은 "브랜치들(branches)"입니다. Git 내부적으로, 브랜치들은 `.git/refs/heads`로 불리는 디렉터리에 저장됩니다.(기술적으로는, [공식 깃 용어집](https://git-scm.com/docs/gitglossary)에 따르면, 브랜치란 해당 브랜치에 있는 모든 커밋(commit)이며 헤드는 그 중 가장 최근의 커밋일 뿐입니다. 같은 것을 표현하는 2가지의 다른 방법이 있는 거죠)
- `HEAD`는 현재 브랜치입니다. `.git/HEAD`에 저장되어 있습니다.

제 생각에 "`head`는 하나의 브랜치이며, `HEAD`는 현재 브랜치이다"는 문장이 Git이 선택한 용어 중 가장 이상한 것 후보가 될만 합니다. 그러나 지금으로써는 이미 더 명확한 이름으로 변경하기엔 확실히 너무 늦어버렸으니 그냥 넘어가죠.

"`HEAD`는 현재 브랜치다"라는 문장에는 일부 중요한 예외가 있습니다. 이는 이후 설명하도록 하겠습니다.

### “떨어진 HEAD 상태(detached HEAD state)”

여러분은 이런 메시지를 봤을 겁니다:

```bash
$ git checkout v0.1
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

[...]
```

```bash
$ git checkout v0.1
현재 'HEAD와 분리된' 상태에 있습니다. 파일들을 확인해 실험적인 변경을 수행하고
커밋하시기 바랍니다. 이 상태에서 수행한 커밋들은 브랜치로 돌아오는 과정에서 다른 브랜치들에
어떤 영향도 끼치지 않고 버릴 수 있습니다.

[...]
```

이 메시지를 이해하는 방법입니다:

- Git에서는 일반적으로, 가령 `main`과 같이, "현재 브랜치"로 체크 아웃된 상태다.
- 현재 브랜치가 저장된 곳은 `HEAD`로 불린다.
- 작성한 새로운 커밋은 모두 현재 브랜치에 추가되며 `git merge other_branch` 명령을 수행한다면
	이것도 현재 브랜치에 영향을 끼친다.
- 그런데 `HEAD`는 브랜치일 **필요는** 없다. 대신 커밋 ID일 수 있다.
- Git은 이런 상태(즉, HEAD가 브랜치가 아니라 커밋 ID인 상태)를 "떨어진 HEAD 상태"로 부른다.
- 예를 들어, 태그로 체크 아웃 함으로써 떨어진 HEAD 상태에 들어설 수 있는데, 이는 태그가 브랜치가 아니라서 가능하다.
- 현재 브랜치가 없다면, 여러가지가 고장난다:
	- `git pull`은 전혀 동작할 수 없다(이는, 명령어의 존재 목적 자체가 현재 브랜치를 업데이트 하는 것이기 때문)
	- 또한 `git push`도 특이한 방식으로 사용하지 않는한 동작하지 않는다
	- `git commit`, `git merge`, `git rebase`, `git cherry-pick`은 **의외로** 잘 작동하지만
		이런 명령어들은 어떤 브랜치와도 연결되어 있지 않은 '고아(orphaned)' 커밋 상태로 남게 만들기 때문에
		해당 커밋들을 찾기 어렵게 만든다
- 떨어진 HEAD 상태로부터 벗어나기 위해서는 새로운 브랜치를 만들거나 존재하는 브랜치로 스위치 하는 방법이 있다.

### 머지(merge), 리베이스(rebase) 과정에서 “우리쪽(ours)”과 “저쪽(theirs)”

머지(merge) 과정에서 충돌(conflict)이 발생하면, `git checkout --ours file.txt` 같은 명령을 수행해
"우리쪽(ours)"에서 `file.txt`의 버전을 고를 수 있습니다. 그런데 뭐가 "우리쪽(ours)"이고 뭐가 "저쪽(theirs)"일까요?

저도 이 부분이 항상 혼란스러웠기 때문에 `git checkout --ours` 명령은 절대 사용하지 않습니다.
그러나 뭐가 뭔지 이번에 찾아봤습니다.

일단 머지의 작동 방식은 이렇습니다: 현재 브랜치는 "우리쪽"이고 머지 해올 브랜치가 "저쪽"입니다. 합리적인데요.

```bash
$ git checkout merge-into-ours # 현재 브랜치가 "우리쪽"
$ git merge from-theirs # "저쪽"으로부터 머지함
```

리베이스는 이와 반대입니다 - 현재 브랜치가 "저쪽"이고 리베이스를 진행하는 타겟 브랜치가 "우리쪽" 입니다:
```bash
$ git checkout theirs # 현재 브랜치가 "저쪽"
$ git rebase ours # "우리쪽"을 리베이스함
```

제 생각에 이런 식으로 작동하는 이유는, 내부에서 `git rebase main`이 현재 브랜치를 메인에 머지하기 때문으로 보입니다
(마치 `git checkout main; git merge current_branch`처럼). 그러나 여전히 혼란스러다고 느낍니다.

이 [사이트](https://nitaym.github.io/ourstheirs/)가 "우리쪽"과 "저쪽" 용어를 설명하고 있습니다.

몇몇 사람들이 VSCode에서는 "ours"/"theirs"를 "현재 변화"/"들어오는 변화"로 부른다는 점을 언급했는데, 이 역시 같은 방식으로 헷갈립니다.

### “당신의 브랜치는 ‘원격/메인’ 최신 상태입니다(Your branch is up to date with ‘origin/main’)”

이 메시지는 직관적인 것처럼 보입니다 - 당신의 `main` 브랜치가 원격(origin)에 최신화,동기화 되어있다는데요!

그러나 여기엔 약간의 오해 소지가 있습니다.
이 메시지를 여러분의 `main` 브랜치가 최신화되어있다는 뜻으로 생각할 수 있습니다.
그러나 그렇지 않습니다.
이것이 **실제로** 의미하는 바는 - 예를 들어 여러분이 `git fetch` 또는 `git pull`을 5일 전에 수행했다고 가정했을 때,
여러분의 `main` 브랜치가 **그 5일 전 변화**에 최신화 되어 있다는 의미입니다.

따라서 이를 깨닫지 못하면, 보안상 안전하다는 잘못된 판단을 하게 만듭니다.

제 생각에 이론적으로는 Git이 여러분에게 "**당신의 마지막 fetch였던 5일 전** 원격(origin)의 `main`과 최신화되어 있음"같은 식의 유용한 메시지를 줄 수도 있어 보입니다. 왜냐하면 가장 마지막 수행한 fetch 시간이 reflog에 저장되기 때문인데요. 그런데 그러지 않네요.

### HEAD^, HEAD~ HEAD^^, HEAD~~, HEAD^2, HEAD~2

저는 오랫동안 `HEAD^`가 직전 커밋을 일컫는 다는 점을 알고 있었습니다.
그러나 `HEAD~`와 `HEAD^`의 차이에 대해서는 오랫동안 혼란스러워 했는데요.

검색해보고 서로와 어떻게 관련되는지 정리했습니다:
- `HEAD^`와 `HEAD~`는 같음(1 커밋 전)
- `HEAD^^^`와 `HEAD~~~`, `HEAD~3`는 같음(3 커밋 전)
- `HEAD^3`은 커밋의 3번째 부모(parent)를 가리키며 `HEAD~3`과 다름

여기서 이상합니다 - 어째서 `HEAD~`와 `HEAD^`가 같은 걸까요?
그리고 "세 번째 부모"가 뭐죠? 혹시 부모의 부모의 부모와 같은 걸까요?(스포일러: 아닙니다)
살펴보죠!

대부분의 커밋은 단 하나의 부모를 가집니다
그러나 머지 커밋(Merge commits)들은 다수의 부모를 갖습니다 -2개 이상의 커밋을 머지하기 때문이죠.
Git에서 `HEAD^`란 "HEAD 커밋의 부모"를 말합니다. 그런데 여기서 만약 HEAD가 머지 커밋(merge commit)이라면 어떻게 될까요?
`HEAD^`는 어떤 것을 가리키죠?

답은 '`HEAD^`는 머지의 **첫 번째** 부모를 가리킨다'입니다. `HEAD^2`는 두 번째 부모를, 마찬가지로 `HEAD^3`는 세 번째 부모를 가리키죠.

그런데 제가 추측하기로 "3 커밋 전"을 가리킬 수 있는 방법도 함꼐 원했던 것 같습니다.
따라서 `HEAD^3`는 현 커밋의 세 번째 부모(머지 커밋이라면 부모를 여럿 가질 수 있음)이며, `HEAD~3`은 부모의 부모의 부모를 가리키게 됩니다.

제 생각에 앞서 진행된 머지 커밋 우리쪽/저쪽 논의에서의 맥락을 이어가자면 `HEAD^`는 "우리쪽", `HEAD^2`는 "저쪽"이죠.

## ..와 ...

여기 두 개의 명령어가 있습니다:
- `git log main..test`
- `git log main...test`

`..`와 `...`의 차이가 뭘까요? 저는 이것을 절대 사용하지 않다보니 [git-range-diff 매뉴얼 페이지](https://git-scm.com/docs/git-range-diff)를 확인해봤습니다. 아래와 같은 경우에:

```bash
A - B main
  \
	C - D test
```

- `main..test`는 커밋 C와 D입니다
- `test..main`은 커밋 B입니다
- `main...test`는 커밋 B, C, D입니다

이보다 더 복잡합니다: 확실히 `git diff` 역시 `..`와 `...`를 지원합니다. 그러나 여기서는 `git log`와 완전히 다른 방식으로 작동합니다. 요약하자면:
- `git log test..main`은 `main`에 존재하는 변화지만 `test`에는 없는 것들을 보여줍니다. 반면,
	`git log test...main`은 양 쪽 변화를 모두 보여줍니다.
- `git diff test..main`은 `test`의 변화와 **더불어** `main`의 변화도 보여줍니다(`B`와 `D`의 차이점을 보여줌). 반면,
	`git diff test...main`은 `A`와 `D`의 차이를 보여줍니다(한 쪽에 존재하는 차이점만 보여줌)

[이 블로그 포스트](https://matthew-brett.github.io/pydagogue/pain_in_dots.html)가 조금 더 자세히 설명하고 있습니다.

### "패스트 포워드 가능(can be fast-forwarded)"

여러분이 `git status`의 출력(output)으로 흔하게 보시는 메시지입니다.

```bash
$ git status
On branch main
Your branch is behind 'origin/main' by 2 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)
```

```bash
$ git status
브랜치 main에 있음
브랜치가 '원격/메인'으로 부터 2커밋 뒤쳐져있으며 패스트 포워드 가능.
  ("git pull"을 통해 로컬 브랜치를 업데이트 하시오)
```

"패스트-포워드(fast-forward)"가 무슨 뜻일까요? 기본적으로 두 브랜치 상황이 아래와 같다는 것을 말하려고 하는 것입니다:(우측일수록 신규 커밋)

```bash
main:        A - B - C
origin/main: A - B - C - D - E
```

다른 방식으로 시각화하자면:

```bash
A - B - C - D - E (origin/main)
		|
	   main
```

여기서 `origin/main`에는 `main`이 갖고 있지 않은 2개의 여분 커밋이 존재합니다.
따라서, `main`을 최신화 하는 것은 쉽습니다 - 그 2개의 커밋을 추가하면 되죠.
말 그대로 어떤 것도 잘못될리가 없습니다 - 머지 충돌(merge conflict)이 발생할 확률도 없구요.
패스트 포워드 머지(fast forard merge)는 훌륭한 겁니다! 2개의 브랜치를 합치는 가장 쉬운 방법이죠.

`git pull`을 수행한 이후, 이런 상황이 됩니다:

```bash
main:        A - B - C - D - E
origin/main: A - B - C - D - E
```

그러나 아래는 패스트-포워드 **할 수 없는** 상황의 예시입니다.

```bash
			 A - B - C - X  (main)
					 |
					 - - D - E  (origin/main)
```

여기서 `main`은 `origin/main`이 갖지 않은 (`X`) 커밋을 갖고 있습니다. 따라서 패스트 포워드를 진행할 수 없습니다. 이런 경우, `git status`는 다음과 같은 메시지를 내보냅니다:

```bash
$ git status
Your branch and 'origin/main' have diverged,
and have 1 and 2 different commits each, respectively.
```

```bash
$ git status
브랜치가 'origin/main'으로부터 분화됨,
각각 1개 2개의 다른 커밋이 존재함.
```

### “레퍼런스(reference)”, “심볼릭 레퍼런스(symbolic reference)”

저는 항상 "레퍼런스(reference)"라는 용어를 헷갈린다고 생각했습니다. 아래는 git에서 "레퍼런스"가 가리키는 최소한 3가지의 용례입니다

- `main`이나 `v0.2`와 같은, 브랜치와 태그
- 현재 브랜치를 의미하는, `HEAD`
- `HEAD^^^` 따위의 git이 커밋 ID 처리를 위해 사용하는 것들.

기술적으로 이런 것들이 "레퍼런스"는 아닐 수 있으며, git은 이런 것들을 "수정 매개변수(revision parameters)"로 [부르는 것으로 보이지만](https://git-scm.com/docs/revisions) 저는 그런 용어를 사용해본적이 없습니다.

"심볼릭 레퍼런스(symbolic reference)"는 굉장히 광범위한 용어 같습니다. 이는 제가 심볼릭 레퍼런스를 사용해 본 유일한 상황이 `HEAD`(현재 브랜치) 관련해서가 전부이기 때문인데요. 더욱이 `HEAD`는 git에서 상당히 핵심에 해당하다보니(git의 주요 명령어들의 연산은 대부분 `HEAD` 값에 의존한다) 이런 일반적인 컨셉을 갖고 있는 목적을 알 수 없네요.

### refspecs

여러분이 `.git/config`에 있는 원격 저장소를 설정할 때, `+refs/heads/main:refs/remotes/origin/main` 같은 것을 볼 수 있습니다.

```bash
[remote "origin"]
	url = git@github.com:jvns/pandas-cookbook
	fetch = +refs/heads/main:refs/remotes/origin/main
```

저는 이게 뭔 뜻이 진짜 모릅니다. 저는 그냥 `git clone` 혹은 `git remote add` 등을 수행하며 적힌 디폴트 값들을 사용해왔고, 이에 대해 더 자세히 배워야겠다든지 디폴트 값을 바꿔야겠다는 동기를 가졌던 적이 전혀 없습니다.

### "tree-ish"

`git checkout` 매뉴얼 페이지에 따르면:

```bash
git checkout [-f|--ours|--theirs|-m|--conflict=<style>] [<tree-ish>] [--] <pathspec>...
```

그런데 `tree-ish`가 뭐죠??? git이 말하고자 하는 바는 `git checkout THING .`을 수행할 때, `THING`이란:
- 커밋 ID(`182cd3f` 따위)
- 커밋 ID를 가리키는 레퍼런스(`main` 또는 `HEAD^^`, `v0.3.2` 따위)
- 커밋 **내부의** 서브디렉터리(`main:./docs` 따위)
- 이 정도인 듯????

개인적으로 "커밋 내부의 디렉터리"는 사용해본 적이 없고 제가 봤을 때 "tree-ish"는 아마 "커밋 혹은 커밋 레퍼런스"를 의미하는 것 같습니다.

### “인덱스(index)”, “스테이지됨(staged)”, “캐시됨(cached)”

아래는 모두 정확히 같은 것을 가리킵니다(파일 `.git/index`, 이는 변경 사항이 `git add`를 통해 스테이지 되었을 때 생성됨):

- `git diff --cached`
- `git rm --cached`
- `git diff --staged`
- `.git/index` 파일

궁극적으로 같은 파일을 가리키지만, 이런 용어들이 실제로 사용되는 다양한 용례가 있습니다:

- 예상하시듯 `--index`와 `--cached` 플래그(flag)들은 일반적으로 같은 것을 의미하지 않습니다.
	저는 개인적으로 `--index`를 전혀 사용해 본적이 없으며 따라서, 이를 더 자세히 설명하지 않을 생각입니다. 그러나 [Junio Hamano의 블로그 포스트](https://gitster.livejournal.com/39629.html)(git 리드 메인테이너)가 세부 사항을 모두 설명하고 있습니다.
-"인덱스"는 트래킹되지 않는 파일들을 목록화 합니다(아마도 성능 이유인 듯), 그러나 여러분은 일반적으로 "스테이징 구역(staging area)"가 "트래킹되지 않는 파일들"을 포함한다고 생각하지 않습니다

### “리셋(reset)”, “되돌림(revert)”, “복원(restore)”

많은 분들이 "리셋(reset)", "되돌림(revert)", "복원(restore)"가 비슷한 단어이며 구분하기 어렵다고 언급하셨습니다.

상황을 더욱 악화시키는 것은

- `git reset --hard`와 `git restore .`가 각자 같은 일을 수행하기 때문입니다. (물론 `git reset --hard COMMIT`과 `git restore --source COMMIT .`의 경우에는 서로 완전히 다른 것이기는 함)
- 각각의 매뉴얼 페이지가 유용한 설명을 제공하지 못함:
	- `git reset`: "현재 HEAD 상태를 특정 상태로 리셋함(reset)"
	- `git revert`: "존재하는 커밋 일부를 되돌림(revert)"
	- `git restore`: "작업중인 트리 파일들을 복원함(restore)"

위의 짧은 설명들을 통해 영향을 받는 대상이 무엇인지 파악할 수는 있지만("현재 HEAD", "일부 커밋", "작업 중인 트리 파일들") 여러분들이 이미 "reset", "revert", "restore"가 문맥상 무엇을 의미하는지 알고 있을 것으로 가정하고 있습니다.

각각의 짧은 설명입니다:
- `git revert COMMIT`: 현재 브랜치의 COMMIT의 "반대되는" 새로운 커밋을 작성함(만약 COMMIT이 3줄을 추가했다면, 새로운 커밋은 해당 3줄을 삭제함)
- `git reset --hard COMMIT`: 여러분의 현재 브랜치가 `COMMIT` 상태의 모습으로 돌아가도록 강제하며, `COMMIT`이후 생긴 모든 변화를 삭제함. 상당히 위험한 작업.
- `git restore --source=COMMIT PATH`: `PATH` 내의 모든 파일들을 `COMMIT` 당시 상태로 돌려놓음, 다른 파일들은 변화 발생하지 않으며 커밋 히스토리 역시 변화 없음.

### “트랙되지 않는 파일(untracked files)”, “원격-트래킹 브랜치(remote-tracking branch)”, “원격 브랜치 트래킹하기(track remote branch)”

Git은 "트랙(track)"이라는 단어를 3가지의 서로 다르면서도 관련있는 방식으로 사용합니다:

- `git status`의 출력에서 `Untracked files:`. 이는 해당 파일들을 Git이 관리하지 않고 있으며 커밋에 포함되지 않을 것임을 의미합니다.
- `origin/main` 따위의 "원격 트래킹 브랜치"에서 사용. 이는 로컬 레퍼런스이며, 마지막으로 여러분이 `git pull` 혹은 `git fetch`를 수행한 이후 `main`이 가리키는 원격 `origin`의 커밋 ID를 의미합니다.
- "오리진의 원격 브랜치 bar를 **트랙**하는 브랜치 foo 구성"

"트랙되지 않는 파일"이라든지 "원격 트래킹 브랜치"의 경우는 나쁘지 않습니다 - 모두 "트랙"을 사용하지만 맥락이 크게 다르지 않죠. 문제 없습니다. 그러나 다른 두 가지 "트랙"의 용례가 상당히 혼란스럽습니다:

- `main`은 원격을 트랙하는 브랜치다(branch that tracks a remote)
- `origin/main`은 원격-트래킹 브랜치다(remote-tracking branch)

"원격을 트랙하는 브랜치"와 "원격-트래킹 브랜치"는 Git에서 서로 다른 것들이며 그 구분은 매우 중요합니다! 여기 그 차이점 요약일 적어드립니다:

- `main`은 브랜치 입니다. 여기에 커밋을 만들거나, 여기로 머지할 수 있씁니다.보통 원격 `main`을 "트랙(track)"하는 것으로 `.git/config`에 설정되어 있습니다.이는 `git pull`이나 `git push`를 사용해 가져오기/밀어내기 작업을 수행할 수 있도록 하죠.
- `origin/main`은 브랜치가 아닙니다. 이는 "원격-트래킹 브랜치(remote-tracking branch)"이며, 브랜치 종류가 아닙니다(죄송).여기에는 커밋을 만들 수 **없습니다**.이를 업데이트하는 유일한 방법은 `git pull`또는 `git fetch`를 수행해 원격으로부터 `main`의 최신 상태를 가져오는 것입니다.

저는 이전에 이 부분이 모호하다는 생각을 해본 적이 없는데 가만보니 사람들이 왜 혼란스러워 하는지 알 것도 같네요.

### 체크아웃(checkout)

체크아웃(checkout)은 두개의 완전히 관련 없는 것들을 수행합니다:

- `git checkout BRANCH`는 브랜치를 변경합니다
- `git checkout file.txt`는 `file.txt`에 존재하는 스테이지되지 않은 변화들을 버립니다

이는 혼란을 유발하는 것으로 매우 잘 알려진 부분이며 따라서 git은 실제로 두 기능을 `git switch`와 `git restore`로 구분했습니다(그러나 만약 여러분이 저처럼 15년간의 습관이 `git checkout` 기반으로 형성되어 있고 배운걸 억지로 버리기 싫으신 분들은 여전히 checkout을 사용할 수 있습니다).

게다가 개인적으로 15년이나 지나고도 여전히 저는 `main` 브랜치로부터 `file.txt`의 버전을 복원하기 위해 수행할 `git checkout main file.txt`의 인자 순서를 제대로 기억할 수가 없습니다.

제 생각에 여러분들은 간혹 `--`를 인자로써 `checkout`을 수행해 어떤 인자가 브랜치고 어디가 경로인지를 확인할 수도 있다고 보지만 저는 그렇게 하지 않으며 언제 그런 방법이 필요할지 모르겠네요.

### reflog

많은 사람들이 reflog를 `ref-log`가 아닌 `re-flog`로 읽는다고 언급합니다. 이미 포스트가 상당히 길기 때문에 저는 여기서 reflog에 대해 깊이 들어가지는 않을 겁니다. 그러나:

- "레퍼런스"는 git이 브랜치, 태그, HEAD를 의미하며 사용하는 일반 용어(umbrella term)입니다
- 레퍼런스 로그("reflog")는 여러분에게 레퍼런스가 가리켰던 모든 히스토리를 제공합니다
- 만약 여러분이 실수로 주요 브랜치를 삭제했다든지 하는 등의 상당히 안좋은 git 상황으로부터 벗어날 수 있도록 도움을 제공합니다
- 저는 git UI의 최고 헷갈리는 부분 중 하나라고 생각하며 가급적 사용을 피하려고 합니다

### 머지(merge) vs 리베이스(rebase) vs 체리픽(cherry-pick)

꽤 많은 사람들이 머지와 리베이스, 그리고 리베이스에서의 "베이스"가 뭘 말하고자 하는지 구분하는게 어렵다는 점을 언급하셨습니다.
저는 여기서 상당히 간략한 요약을 제공합니다. 그러나 여러분에게 여기 나오는 1줄짜리 설명이 도움이 될 것으로 기대하지는 않습니다. 이는 많은 사람들이 각자의 작업 방식을 머지/리베이스의 각기 다른 이해 방식을 중심으로 구성했놨을 것이기 때문이며 실제로 머지/리베이스를 이해하고자 한다면 작업흐름(workflow)를 이해해야 하기 때문입니다. 또한 그림이 상당한 도움이 됩니다. 그또한 하나의 별도 포스트가 될 개연성이 크므로 여기서 자세히 파고들진 않을 겁니다..

- 머지는 2개의 브랜치를 합치는 새로운 단일 커밋을 생성함
- 리베이스는 한 번에 한 개씩 현재 브랜치에 있는 커밋을 타깃 브랜치로 복사함
- 체리-픽은 리베이스와 유사하지만 완전히 다른 문법을 지니고 있음(가장 큰 차이점 중 하나는 리베이스가 커밋을 현재 브랜치**로부터** 복사하는 반면, 체리 픽은 현재 브랜치**로** 커밋을 복사함)

### rebase --onto

`git rebase`는 `onto`라는 플래그(flag)를 가집니다. 이는 `git rebase main`의 존재 목적 자체가 현재 브랜치를 메인 **위에** 리베이스하는 것이라는 점에서 저를 혼란스럽게 했습니다.따라서 별도의 `onto` 인자는 왜 존재할까요?

찾아보니 `--onto`가 확실히 저는 거의/전혀 겪어보지 못했던 문제를 해결하기는 합니다. 그래도 일단 제가 이해한 것을 적어보도록 하겠습니다.

```bash
A - B - C (main)
	 \
	  D - E - F - G (mybranch)
		  |
		  otherbranch
```

무슨 이유에서인지 제가 갑자기 커밋 `F`와 `G`를 `main`에 리베이스해야 된다고 칩시다. 제 생각에 이런 상황이 발생하는 git 작업흐름이 꽤나 존재할 것 같은데요.

여기서 여러분은 `git rebase --onto main otherbranch mybranch`를 수행할 수 있습니다. 제가 보기에 이 문법을 기억하는 것은 불가능해 보입니다(3개의 각기 다른 브랜치 이름이 필요하며, 제가 보기엔 너무 많네요). 그러나 많은 사람들이 이야기를 나누는 것을 보니 분명 유용한가 봅니다.

### 커밋(commit)

누군가 커밋(commit)이 git에서 동사로도 쓰이고 명사로도 쓰여 혼란스럽다고 언급하셨습니다.

예를 들어:
- 동사: "자주 커밋해야 함을 기억하라"
- 명사: "`main`에 있는 가장 최근의 커밋"

제 추측상 대부분 사람들은 이 부분에 꽤 빨리 익숙해지는 것 같습니다만, 이런 방식의 "커밋" 용례는 SQL 데이터베이스에서의 사용과 상당히 다릅니다. 그 쪽에서의 "커밋"은 제 기억에 동사로써만 쓰이지(여러분은 작업을 완료한 이후 "COMMIT"합니다) 명사로는 안 쓰이니까요.

추가로 git에서는 Git 커밋을 세가지 다른 방식으로 생각할 수도 있습니다:

1. 모든 파일의 현재 상태 **스냅샷**
2. 부모 커밋과의 **차이점**(diff)
3. 모든 이전 커밋 **기록**(history)

틀린 것은 없습니다: 서로 다른 명령어들이 커밋의 이런 모든 용례들을 활용하고 있습니다. 예를 들어 `git show`는 커밋을 차이점으로 다루고, `git log`는 기록으로 다루며, `git restore`는 스냅샷으로 활용합니다.

그러나 git의 용어 사용이 주어진 명령어에서 커밋이 어떤 용례로 활용될지 이해하는데 큰 도움이 되지는 않습니다.

### 나머지 헷갈리는 용어들

여기 헷갈리는 용어들이 더 있습니다. 저도 많은 것들의 의미를 잘 모릅니다.

제가 진짜 도저히 이해할 수 없는 것들:
- "the git pickaxe"(아마 이는 `git log -S`와 `git log -G`를 의미하고, 이전 커밋들의 차이를 검색하는데 쓰일까요?)
- 서브모듈(제가 알기로 얘네는 제가 원하는 방식으로 작동하지 않습니다)
- git sparse checkout에서의 "cone mode"(이게 뭔지 감도 안오지만 누군가 언급했습니다)

사람들이 헷갈린다고 언급했으나 이미 포스트가 3000자가 넘어 생략한 것들:
- blob, tree
- "merge" 방향
- "origin", "upstream", "downstream"
- `push`와 `pull`이 반대가 아니라는 점
- `fetch`와 `pull` 사이 관계(pull=fetch+merge)
- git prcelain
- 서브트리(subtree)
- 워크트리(worktree)
- the stash
- "마스터"냐 "메인"이냐 (뭔가 특별한 의미가 있는 것 같지만 없습니다)
- `origin main`을 사용해야 하는 때(`git push origin main` 등) vs `origin/main`이 필요한 때

사람들이 언급한 Github의 용어들:

- "pull request" (vs Gitlab의 "merge request"를 사람들은 더 명확하다고 생각하는 것 같습니다)
- "squash and merge"와 "rebase and merge"가 뭘 수행하는지(저는 어제까지 `git merge --squash`를 들어본 적도 없다보니, "squash and merge"가 Github의 특이한 기능인 줄 알았습니다)

### 사실상 "모든 git 용어"입니다

저는 git의 핵심 기능의 사실상 모든 것들을 대상으로 최소한 한 명 이상은 희한한 방식으로 헷갈린다고 언급하는 것을 보고 놀랐습니다.
제가 빼먹은 것들 중 헷갈리는 git 용어들에 대한 예시를 더 들을 수 있으면 좋을 것 같습니다.

이에 대해 과거 2012년에 작성된 훌륭한 포스트 [가장 헷갈리는 git 용어](https://longair.net/blog/2012/05/07/the-most-confusing-git-terminology/)가 또 있습니다. 이 글에서는 git의 용어들이 어떻게 CVS와 Subversion의 용어들과 관련되어 있는지에 집중하고 있습니다.

제가 git 용어 중 가장 헷갈리는 3가지를 골라야 한다면, 당장 떠오르는 것들은 아래와 같습니다:
- `head`는 브랜치인데 `HEAD`는 현재 브랜치다
- "원격 트래킹 브랜치"와 "원격을 트래킹하는 브랜치"가 서로 다른 것이다
- "인덱스", "스테이지됨", "캐시됨"이 같은 것을 의미한다

### 여기까지예요!

저는 이것을 작성하며 많이 배웠습니다(TN: 저도 이걸 번역하며 많이 배웠네요!) - git에 대해 새로운 사실들도 알게 되었고, 가장 중요한 건 누군가 git의 모든게 헷갈린다고 말할 때 그게 뭔 소린지 조금은 잘 이해할 수 있게 되었다는 점입니다.

솔직히 이런 문제들에 대해 전에는 깊게 고민해본 적이 없습니다 - 즉, "트래킹"이 브랜치들 논의 중에 사용되는 이상한 방식에 대해 알지 못했죠.

추가로, 경험해보지 못했던 git의 구석구석을 다루다보니 평소처럼 제가 몇몇 실수를 했을지도 모릅니다.
