## Oh shit, git!

#### git reflog

* 내가 깃으로 작업했던 모든 내역을 볼 수 있다.(모든 브랜치)
* `HEAD@{index}` 이런식으로 index 번호를 가지고 있다.

``` git
  git reset HEAD@{index} // 이런식으로 사용가능


  git rest HEAD@{2} // first commit 상태로 돌아간다.
```

1. `git init`
2. `git1.html` 생성
3. `git add git1.html`
4. `git commit -m 'first commit'`
5. `git checkout -b other` // other 브랜치 생성 및 이동
6. `git2.html` 생성
7. `git add git2.html`
8. `git commit -m "added git2.html"`
9. `git reflog`
10. `git reset 'HEAD@{2}'` // first commit 완료 후 상태로 돌아간다.

![reflog](./reflog.png)

#### git commit --amend

* 커밋 직후 커밋 메시지를 수정을 해야될 때

``` git
git add git2.html
git commit -m "git2.html"

git commit --amend
//added git2.html 으로 커밋메세지 수정
```

![ammend](./ammend.png)

#### 현재 브랜치에서 따온 새로운 브랜치에 커밋해야되는데 현재 브랜치에 커밋한 경우

_ master branch에서 생성 된 some branch에 커밋해야되는데 master branch 커밋한 경우_
``` js
// master
git add git3.html
git commit -m "added git3.html"

git branch some

git reset HEAD~ --hard
git checkout some

// some branch에는 "added git3.html" 커밋이 살아있꼬 master branch에는 없다.
```

* commit log가 최소 2개이상일때 동작된다.

#### 다른 브랜치에 커밋했을때

``` git
// matser
git add git4.html
git commit -m "added git4.html"
git reset HEAD~ --soft // commit 취소하지만 add한것, 즉 변경사항은 그대로
git stash // 현재 변경사항을 저장(잘라내기)

// move to new branch
git checkout -b new
git stash pop // 아까 잘라낸거 복구
git commit -m "write message"
```

*다른 방법 cherry-pick*
* 공부하기

``` git
git checkout name-of-the-correct-branch
# grab the last commit to master
git cherry-pick master
# delete it from master
git checkout master
git reset HEAD~ --hard
```
