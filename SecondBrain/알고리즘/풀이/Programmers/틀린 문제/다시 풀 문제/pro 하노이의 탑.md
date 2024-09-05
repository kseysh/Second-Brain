```java
class Solution {
    public int[][] solution(int n) {

        return hanoi(1, 3, 2, n);
    }
    
    public int[][] hanoi(int start, int target, int temp, int n){
        if(n == 1){
            int[][] arr = {{start, target}};
            return arr;
        }
        
        int[][] firstHanoi = hanoi(start, temp, target, n-1);
        int firstHanoiLength = firstHanoi.length;
        int[][] secondHanoi = {{start, target}};
        int secondHanoiLength = 1;
        int[][] thirdHanoi = hanoi(temp, target, start, n-1);
        int thirdHanoiLength = thirdHanoi.length;
        int resultArrLength = firstHanoiLength + secondHanoiLength + thirdHanoiLength;
        int[][] result = new int[resultArrLength][2];
        for (int i = 0; i < resultArrLength; i++){
            if(i < firstHanoiLength){
                result[i][0] = firstHanoi[i][0];
                result[i][1] = firstHanoi[i][1];
            }else if (i < firstHanoiLength + secondHanoiLength){
                result[i][0] = secondHanoi[0][0];
                result[i][1] = secondHanoi[0][1];
            }else{
                result[i][0] = thirdHanoi[i - firstHanoiLength - 1][0];
                result[i][1] = thirdHanoi[i - firstHanoiLength - 1][1];
            }
        }
        return result;
    }
}
```