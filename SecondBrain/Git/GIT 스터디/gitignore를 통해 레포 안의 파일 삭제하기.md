레포에 이미 커밋을 하고 gitignore에 파일을 추가해도 제외가 되지 않는데, 이 때는 강제로 캐시를 날려줘야 한다.

```
git rm -r --cached .
git add .
git commit -m "fixed untracked files"
```
