```java
class Solution {
    public int solution(int[][] board) {
        final int isNotSafe = -1;
        final int boardWidth = board.length;
        
        for(int i = 0; i < boardWidth; i++){
            for(int j = 0; j < boardWidth; j++){
                int boardValue = board[i][j];
                if(boardValue == 1){
                    for(int k = - 1; k <= 1; k ++){
                       for(int l = -1; l <= 1; l++){
                           int isNotSafeIndexX = j + k;
                           int isNotSafeIndexY = i + l;
                           if(isNotSafeIndexX >= 0 && isNotSafeIndexX < boardWidth){
                               if(isNotSafeIndexY >= 0 && isNotSafeIndexY < boardWidth){
                                    if(board[isNotSafeIndexY][isNotSafeIndexX] == 0){
                                        board[isNotSafeIndexY][isNotSafeIndexX] = isNotSafe;
                                    }       
                               }
                           } 
                        }
                    }    
                }
            }
        }
        
        int answer = 0;
        
        for(int i = 0; i < boardWidth; i++){
            for(int j = 0; j < boardWidth; j++){
                if(board[i][j] == 0){
                    answer++;
                }
            }
        }
        return answer;
    }
}
```

## 배울 점 
인덱스를 잘 보자! board\[y]\[x]여야 하는데 board\[x]\[y]로 넣어서 틀렸다.