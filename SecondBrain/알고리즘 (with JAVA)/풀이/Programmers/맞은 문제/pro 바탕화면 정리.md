```java
class Solution {
    public int[] solution(String[] wallpaper) {
        int minX = 100;
        int maxX = -1;
        int minY = 100;
        int maxY = -1;
        
        int wallpaperWidth = wallpaper[0].length();
        int wallpaperHeight = wallpaper.length;
        
        for(int y = 0; y < wallpaperHeight; y++){
            for(int x = 0; x < wallpaperWidth; x++){
                if(wallpaper[y].charAt(x) == '#'){
                    if(minX > x){
                        minX = x;
                    }
                    if(maxX < x){
                        maxX = x;
                    }
                    if(minY > y){
                        minY = y;
                    }
                    if(maxY < y){
                        maxY = y;
                    }
                }
            }
        }
        
        
        int[] answer = {minY, minX, maxY + 1,maxX + 1};
        return answer;
    }
}
```