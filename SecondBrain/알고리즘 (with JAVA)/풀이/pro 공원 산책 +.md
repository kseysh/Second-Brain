```java
class Solution {
        int x = 0;
        int y = 0;
        public int[] solution(String[] park, String[] routes) {

            int parkWidth = park[0].length();
            int parkHeight = park.length;
            this.findS(park);

            for(String opN: routes){
                char op = opN.charAt(0);
                int n = Integer.parseInt(opN.split(" ")[1])     ;
                if(op == 'S'){
                    boolean isMove = true;

                    if(y + n >= parkHeight){ // 공원을 벗어납니다.
                        isMove = false;
                    } else{
                        for(int i = 1; i < n + 1 ; i++){ // 장애물이 있는지 확인합니다.
                            if(park[y + i].charAt(x) == 'X') {
                                isMove = false;
                                break;
                            }
                        }
                    }

                    if(isMove){ // 공원을 벗어나지도, 장애물이 있지도 않으면 움직입니다.
                        y += n;
                    }
                }
                if(op == 'N'){
                    boolean isMove = true;

                    if(y - n < 0){ // 공원을 벗어납니다.
                        isMove = false;
                    } else{
                        for(int i = 1; i < n + 1 ; i++){ // 장애물이 있는지 확인합니다.
                            if(park[y - i].charAt(x) == 'X') {
                                isMove = false;
                                break;
                            }
                        }
                    }

                    if(isMove){ // 공원을 벗어나지도, 장애물이 있지도 않으면 움직입니다.
                        y -= n;
                    }

                }
                if(op == 'W'){
                    boolean isMove = true;

                    if(x - n < 0){ // 공원을 벗어납니다.
                        isMove = false;
                    } else{
                        for(int i = 1; i < n  + 1; i++){ // 장애물이 있는지 확인합니다.
                            if(park[y].charAt(x - i) == 'X') {
                                isMove = false;
                                break;
                            }
                        }
                    }

                    if(isMove){ // 공원을 벗어나지도, 장애물이 있지도 않으면 움직입니다.
                        x -= n;
                    }
                }
                if(op == 'E'){
                    boolean isMove = true;

                    if(x + n >= parkWidth){ // 공원을 벗어납니다.
                        isMove = false;
                    }else{
                        for(int i = 1; i < n  + 1; i++){ // 장애물이 있는지 확인합니다.
                            if(park[y].charAt(x + i) == 'X') {
                                isMove = false;
                                break;
                            }
                        }
                    }

                    if(isMove){ // 공원을 벗어나지도, 장애물이 있지도 않으면 움직입니다.
                        x += n;
                    }

                }
            }

            int[] answer = {y, x};
            return answer;
        }

        public void findS(String[] park){
            for(int i =0; i < park.length; i++){
                String s = park[i];
                for(int j=0; j < s.length(); j++){
                    if(s.charAt(j) == 'S'){
                        this.x = j;
                        this.y = i;
                    }
                }
            }
        }
    }
```

## 배운 점
조건을 다시 잘 봐야할 것 같아요.
