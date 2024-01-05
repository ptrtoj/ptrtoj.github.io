---
title:  "Git 커밋 메시지에 대하여"
date:   2022-07-11
url: 'commit'
---

> This is a translated work.
>
> The original post was written by Tim Pope and you can read it [here](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).
>
> On 11, July. 2022, permission was granted via e-mail.

> 이 문서는 번역본입니다.
>
> 원본은 팀 포프(Tim Pope)에 의해 작성되었으며, 다음 [링크](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)를 통해 읽을 수 있습니다.
>
> 이메일을 통해 22/07/11 번역 허가를 받아 작성 후 업로드 합니다.

$ 깃 커밋 메시지 관련하여

19 Apr 2008

잘 구성된 커밋(Commit) 메시지가 무엇인지 좀 더 상세히 적고자 시간을 할애 합니다. 깃(Git)을 더 위대하게 만드는 사소한 디테일 중 하나가 바로 커밋 메시지 형식 관행이라고 생각합니다. rails.git의 초기 커밋 중 일부가 굉장히-긴-줄의 메시지 형태를 갖게 된 이유는 이해할만하지만, 왜 이게 안 좋은 것인지 자세히 살펴보고자 합니다.

아래는 깃 메시지 영문 예시입니다:

```
Capitalized, short (50 chars or less) summary

More detailed explanatory text, if necessary.  Wrap it to about 72
characters or so.  In some contexts, the first line is treated as the
subject of an email and the rest of the text as the body.  The blank
line separating the summary from the body is critical (unless you omit
the body entirely); tools like rebase can get confused if you run the
two together.

Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
or "Fixes bug."  This convention matches up with commit messages generated
by commands like git merge and git revert.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space, with blank lines in between, but conventions vary here

- Use a hanging indent
```

아래는 깃 메시지 한글 예시입니다:

```
첫 글자 대문자, 50글자 이내의 짧은 요약

필요한 경우, 더 자세히 설명. 줄 바꿈은 72 글자 근처에서.
몇 상황에서 첫 줄은 이메일의 제목으로도 활용되고 나머지 텍스트는
본문에 쓰임. (본문을 통째로 생략하는 경우가 아닌 이상) 제목과
본문을 구분 짓는 한 줄의 공백이 중요함; rebase 류의
도구들은 구분 짓지 않은 경우 오작동 할 가능성이 있음.

명령형으로 커밋 메시지를 작성할 것: "버그 수정"식으로.
"버그 고쳤음" 혹은 "버그 고치는 중"은 안 됨. 이런 관습을 통해
git merge나 git revert 등의 명령어로 생성된 커밋 메시지와
일치하게 됨.

추가 문단은 추가 공백 줄 이후 적음.

- 글머리 기호를 사용하는 것도 괜찮음

- 통상적으로, 빈 줄 사이의 문장에서 공백 사이에 들어 있는
  하이픈('-')이나 별표('*')가 글머리 기호로 활용되나 이 관례는
  다를 수 있음

- 행잉 인덴트(*바로 아래 설명 참조)를 사용할 것
```

*행잉 인덴트(hanging indent): 단락의 두 번째 라인 및 후속 행의 인덴트 또는 한 라인 보다 큰 표시물의 인덴트. ⇒규범 표기는 미확정이다.*

일단 왜 72 글자 제한을 두는지 몇 가지 이유를 살펴보겠습니다.

- `git log`는 커밋 메시지의 자동 줄바꿈(wrap)을 전혀 하지 않습니다. 기본 페이저인 `less -S`를 사용하는 경우, 문단이 스크린 끝까지 너무 멀리 가버려 읽기 매우 어렵게 만든다는 것을 의미하죠. 세로 80 줄 터미널에서 좌측에 들여쓰기를 위해 4 글자를 빼고 우측에 균형을 위해 똑같이 4 칸을 제외하면 72 칸이 남습니다.
- `git format-patch --stdout` 는 메시지를 본문으로 하여 일련의 커밋들을 연속된 이메일로 변환합니다. 잘 만들어진 이메일 네티켓 지시 사항들을 보면, 답장으로 생기는 지시자 들여쓰기를 감안해도 세로 80 줄의 터미널에서 화면의 가로로 내용이 넘어가지 않도록 하기 위해 일반 텍스트(plain text) 이메일을 미리 래핑하도록 요구합니다. (물론 현재 rails.git 업무 방식은 이메일을 수반하지 않습니다만 미래에 이메일을 활용할지 누가 알겠습니까.)

Vim 사용자들은 이런 요구 사항을 내 [vim-git 런타임 파일](http://github.com/tpope/vim-git)을 설치해 만족 시킬 수 있고 혹은 다음 옵션을 Git 커밋 메시지 파일에 추가해 달성할 수도 있습니다:

`:set textwidth=72`

Textmate의 경우, view 메뉴 아래 “Wrap Column(줄 바꿀 열)” 옵션을 조정할 수 있는데, 이후 `^Q`를 이용해 문단을 원하는 대로 다시 래핑할 수 있습니다 (코멘트들이 섞이는 것을 피하기 위해 빈 줄이 추가된다는 점을 명심하세요). 아래는 매번 선택을 위해 드래그 하지 않기 위해 메뉴에 72를 추가하는 쉘 명령어 입니다:

`$ defaults write com.macromates.textmate OakWrapColumns '( 40, 72, 78 )'`

본문 정렬 방식보다 훨씬 중요한 것은 제목을 다는 것입니다. 예시에서 보인 것처럼, (타협 불가능한 제한 사항까지는 아니지만 가급적) 50 글자를 목표로 삼아야 합니다. 그리고 항상, 무조건 한 줄을 비우세요. 첫 줄은 커밋으로 인해 생기는 변화들의 구체적 요약이어야만 합니다; 만약 기술적인 디테일이 이런 엄격한 글자 수 제한 안에 표현하기에 너무 많다면, 차라리 본문에 넣기 바랍니다. 제목 줄은 깃 전반에 걸쳐 활용되며, 너무 긴 메시지가 사용되면 뒷 부분이 짤려 나가는 경우가 많습니다.  다음은 그런 식으로 짤려 나간 제목들의 종착점입니다:

- `git log --pretty=oneline` 은 커밋 id와 요약으로 구성된 상세한 과거 기록을 보여줌
- `git rebase --interactive` 은 에디터에서 요청하는 항목의 커밋 요약을 보여줌
- 설정 옵션 `merge.summary` 이 켜져 있는 경우, 통합된(Merged) 커밋들의 모든 요약이 머지 커밋 메시지에 포함됨
- `git shortlog` 는 요약 부분의 내용을 명령어가 출력하는 체인지로그-스타일의 아웃풋에 가져다 씀
- `git format-patch`, `git send-email`, 과 관련 도구들은 짤린 제목으로 가져다 이메일 제목으로 씀
- reflogs라는 `git reflog` 명령을 통해 접근 가능한 로컬 기록은 멍청한 실수로부터 복구하고자 존재하는데, 요약에서부터 복사본을 가져옴
- `gitk` 는 요약을 위한 탭이 존재함
- 깃허브(GitHub)는 사용자 인터페이스 곳곳에 요약을 활용함

제목/본문 구분은 사소해 보일 수 있으나 서브버젼(Subversion)에 비해 깃 히스토리가 훨씬 더 작업하기 좋다고 느끼게 만드는 다양한 세부 요인 중 하나입니다.
