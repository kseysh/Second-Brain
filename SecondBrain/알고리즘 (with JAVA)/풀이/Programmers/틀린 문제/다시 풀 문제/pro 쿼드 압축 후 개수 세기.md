```java
class Solution {
    int zeroCount = 0;
    int oneCount = 0;
    int[] answer = new int[2];
    
    public int[] solution(int[][] arr) {
        int arrLength = arr.length;
        dq(arr, 0, 0, arrLength);
        return answer;
    }
    
    public void dq(int[][] arr, int startX, int startY, int size){
        if(check(arr, startX, startY, size)){
            answer[arr[startY][startX]]++;
        }else{
            dq(arr, startX, startY, size/2);
            dq(arr, startX + size/2, startY, size/2);
            dq(arr, startX, startY+ size/2, size/2);
            dq(arr, startX+ size/2, startY+ size/2, size/2);
        }
        
    }
    
    public boolean check(int[][] arr, int startX, int startY, int size){
        int value = arr[startY][startX];
    
        for(int i = startX; i < startX + size; i++){
            for(int j = startY; j < startY + size; j++){
                if(arr[j][i] != value){
                    return false;
                }
            }
        }
        return true;
    }
}
```
이게 분할 정복이라고 하는 듯 합니다.