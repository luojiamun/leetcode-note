### 思路

- 拿到题的直觉解法
    - 主要是处理分针；
    - 开始和结束，如果不在15分钟整数倍位置，就要相应左右缩进
    - 同时注意极端情况
    - 感觉标答里的处理方式可能更好，更清晰一些；

`AC`

- 虽然过了，但是提交了四次。要处理的edge cases太多的话，不如多写几行；


### 代码
```java
class Solution {
    public int numberOfRounds(String startTime, String finishTime) {
        String[] start = startTime.split(":");
        int sMin = Integer.valueOf(start[1]);
        int rawSTime = Integer.valueOf(start[0])*60 + sMin;
        sMin = sMin % 15 == 0?sMin:(sMin / 15 + 1) * 15;
        int sTime = Integer.valueOf(start[0])*60 + sMin;
        
        String[] finish = finishTime.split(":");
        int fMin = Integer.valueOf(finish[1]);
        int rawFTime = Integer.valueOf(finish[0])*60 + fMin;
        fMin = fMin % 15 == 0?fMin:(fMin / 15) * 15;
        int fTime = Integer.valueOf(finish[0])*60 + fMin;
        int time = 0;
        if(rawFTime < rawSTime){
            time =  (24 * 60 - sTime) + fTime;
        } else {
            time =   fTime - sTime;
        }
        System.out.println((time));
        int count = (time / 15);
        
        return count < 0?0:count;
    }
}
```


### 复杂度

time: O(1)
space: O(1)
