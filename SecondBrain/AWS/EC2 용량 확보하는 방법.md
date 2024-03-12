
# 오래된 커널삭제하기
보통 커널들을 저장하는 경로 => `/user/src`
오래된 커널 삭제하는 명령어 
```shell
sudo apt autoremove --purge
```

###### `> You might want to run ‘apt-get -f install’ to correct these…` 문제 발생시
```shell
apt-get -f install
```


# apt 캐시 삭제
```shell
sudo apt clean
```
