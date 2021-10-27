### 思路

- 拿到题的直觉解法
    - 直觉可以用回溯解决
    - 写起来满啰嗦，但应该是能力有限，即便backtrack应该也不至于这么繁琐

`AC`

- 标答提示了可以用枚举，笛卡尔积，其实也确实如此，逻辑上更直观
- 给你两个桶，给你target个球，那么就分配(枚举)一下所有的可能分桶就好了
- 因为java没有enumeration util..偷懒用scala了。。

`AC`

### 代码
```java
//backtrack
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<String> readBinaryWatch(int turnedOn) {
        int[] HOUR = new int[]{8,4,2,1};
        int[] MIN = new int[]{32, 16, 8, 4, 2, 1};
        List<String> finalResult = new ArrayList<>();
        for(int h = 0;h <= Math.min(3, turnedOn);h++){
            res.clear();
            List<Integer> combination = new ArrayList<>();
            backtrack(HOUR, 0, combination, h, true);
            int hour = 0;
            List<String> hours = new ArrayList<>();
            for(List<Integer> lst: res){
                for(Integer i: lst){
                    hour += i;
                }
                hours.add(Integer.toString(hour));
                hour = 0;
            }
            if (hours.isEmpty()) hours.add("0");
            res.clear();
            combination.clear();
            int m = turnedOn - h;
            backtrack(MIN, 0, combination, m, false);
            int min = 0;
            List<String> mins = new ArrayList<>();
            for(List<Integer> lst: res){
                for(Integer i: lst){
                    min += i;
                }
                String toAdd = "";
                if(min < 10){
                    toAdd = "0"+Integer.toString(min);
                } else {
                    toAdd = Integer.toString(min);
                }
                mins.add(toAdd);
                min = 0;
            }
            for(String hh: hours){
                for(String mm: mins){
                    finalResult.add(hh+":"+mm);
                }
            }
     
        
            
        }
        return finalResult;
    }
    
    public boolean backtrack(int[] time, int start, List<Integer> combination, int size, boolean isHour){
        if(combination.size() == size){
            int total = 0;
            for(Integer i: combination){
                total += i;
            }
            if((isHour && total <= 11) || (!isHour && total <= 59)) res.add(new ArrayList(combination));
        }
        
        for(int i = start;i < time.length;i++){
            combination.add(time[i]);
            backtrack(time, i+1, combination, size, isHour);
            combination.remove(combination.size() - 1);
        }
        return false;
    }
}

//分桶
import scala.collection.mutable.ArrayBuffer
object Solution {
    def readBinaryWatch(turnedOn: Int): List[String] = {
        var res = ArrayBuffer[String]();
        val hs = Seq[Int](1,2,4,8);
        val ms = Seq[Int](1,2,4,8,16,32);
        
        (0 to turnedOn).foreach{(h: Int) => {
            val combH = hs.combinations(h).toList.map(_.sum).filter(_ < 12).map(_.toString);
            val combM = ms.combinations(turnedOn - h).toList.map(_.sum).filter(_ < 60).map{
                case o if o /10 == 0 => "0"+o.toString
                case d => d.toString
            };
            for{
                ch <- combH
                cm <- combM
            } yield {
                res.append(ch + ":"+ cm);
            }
        }}
        res.toList;
    }
    
}
```


### 复杂度

//permutation
time: 常数级;具体取决于符合条件的排列组合的个数。C(4, 0)*C(6, k)+C(4, 1)*C(6, k-1)+...+C(4,k)*C(6,0)
space: O(1)