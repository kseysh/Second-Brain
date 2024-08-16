```java
class Solution {
    public int solution(int[][] sizes) {
        
        int maxBiggerLength = -1;
        int maxSmallerLength = -1;
        int biggerLength = -1;
        int smallerLength = -1;
        int width, height;
        for(int i = 0; i < sizes.length; i++){
            width = sizes[i][0];
            height = sizes[i][1];
            if(width > height){
                biggerLength = width;
                smallerLength = height;
            }else{
                biggerLength = height;
                smallerLength = width;
            }
            
            if(maxBiggerLength < biggerLength){
                maxBiggerLength = biggerLength;
            }
            
            if(maxSmallerLength < smallerLength){
                maxSmallerLength = smallerLength;
            }
            
        }
        
        return maxBiggerLength * maxSmallerLength;
    }
}
```