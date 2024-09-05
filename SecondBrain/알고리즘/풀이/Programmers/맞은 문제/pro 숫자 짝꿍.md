```java
class Solution {
    public String solution(String X, String Y) {
        
        int[] xNumberCounts = new int[10];
        for(int i = 0; i < X.length(); i++){
            xNumberCounts[X.charAt(i) - '0']++;
        }
        
        int[] yNumberCounts = new int[10];
        for(int i = 0; i < Y.length(); i++){
            yNumberCounts[Y.charAt(i) - '0']++;
        }
        
        int[] resultNumberCounts = new int[11];
        for(int i = 0; i < 10; i++){
            resultNumberCounts[i] = Math.min(xNumberCounts[i],yNumberCounts[i]);
        }
        
        StringBuilder sb = new StringBuilder();
        for(int i = 9; i > 0; i--){
            for(int j = 0; j < resultNumberCounts[i]; j++){
                sb.append(i);
            }
        }
        
        if(sb.toString().isEmpty()){
            if(resultNumberCounts[0] == 0){
                return "-1";
            } else{
               return "0"; 
            }
        } else{
            for(int j = 0; j < resultNumberCounts[0]; j++){
                sb.append(0);
            }    
        }
        
        return sb.toString();
    }
}
```