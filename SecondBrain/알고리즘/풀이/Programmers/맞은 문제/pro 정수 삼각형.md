```java
class Solution {
    public int solution(int[][] triangle) {
        int triangleLength = triangle.length;
        int[][] sumTriangle = new int[triangleLength][triangleLength];
        
        sumTriangle[0][0] = triangle[0][0];
        for(int i = 1; i < triangleLength; i++){
            for(int j = 0; j < i + 1; j++){
                if(j == 0){
                    sumTriangle[i][j] = triangle[i][j] + sumTriangle[i - 1][j];
                }else if(j == i){
                    sumTriangle[i][j] = triangle[i][j] + sumTriangle[i - 1][i - 1];
                }else{
                    sumTriangle[i][j] = triangle[i][j] + Math.max(sumTriangle[i - 1][j - 1], sumTriangle[i - 1][j]);
                }
            }
        }
        
        int answer = -1;
        for(int i = 0; i < triangleLength; i++){
            answer = Math.max(answer, sumTriangle[triangleLength - 1][i]);
        }
        
        return answer;
    }
}
```