---
layout: single
title:  "VS Code 이용하여 깃허브 블로그에 포스트하기"
date:   2021-07-28 19:27:00 +0530
categories: Github
tags : Github
toc: true
toc_sticky: true
toc_label: "Contents"
---

# Clone 하기 (or pull)
VS-Code 실행 후 `F1` 키를 누른 후 `Git:clone`을 선택하여 내 github주소(http://github.com/블로그주소)를 가져온다.<br>
그 후 ``Ctrl+` ``을 이용해 터미널 창을 열고 자신의 github repository branch 이름에 따라 
```
git pull origin main
```
```
git pull origin master
```
를 입력하여 최신 repository 정보를 가져온다.

<br>

# Staging & Commit
pull하여 가져온 내용을 수정했을 땐 **push**하여 수정한 내용을 다시 github 서버로 보내야한다.

1. Staging
    
    내용을 수정한 후 변경된 파일을 `staged` 상태로 만들어야 commit 할 수 있는데, 이 과정을 staging이라고 한다. VS-Code 에선 수정한 파일을 저장한 후 왼쪽의 'Source control' 탭에서 'Changes'옆의 `+`버튼을 눌러주면 파일을 `staged`상태로 만들어준다.

2. Commit

Staged된 파일을 `commit`한 후 `push`하면 깃허브 블로그에 해당 내용이 반영된다. 마찬가지로 왼쪽의 'Source control' 탭에서 상단의 체크버튼을 눌러주면 `staged`됐던 내용들이 `commit`되어 로컬 저장소에 저장된다. 다음 단계로, 터미널에
```
git push origin main
```
을 입력하면 `commit`한 내용이 github로 `push`되어 원격 저장소에 저장된다.

# Push

다음 단계로, 터미널에

```
git push origin main(or master)
```

을 입력하면 `commit`한 내용이 github로 `push`되어 원격 저장소에 저장된다.
    
<br><br>

그럼 업로드가 완료되고, 잠깐의 시간이 지난 후 블로그에 반영된 것을 확인할 수 있다.
