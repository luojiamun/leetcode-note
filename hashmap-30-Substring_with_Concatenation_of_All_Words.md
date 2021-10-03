### 思路

- 这类题目比较弱，主要是bf的考虑就不够；
- bf来看，可以把所有word的可能组合都写出来直接找，但是n!的时间是不可能完成的；
- 换思路，不从words看，从s来看；
- 把words当key存入hashmap，value存频率；
- 从s的i=0出发，每个位置都
    - 按照单词的长度(题目已经告诉你是固定长度的单词了，否则这里还需要一次遍历)来搜索map，找到了就记录在一个新的map，即found
    - 每次比对一下，found记录的频次如果超过原map，那说明出问题了；break；
    - 如果s的比对没找到，break
    - 如果完美匹配了所有的words，把这个i记录下来


`AC`

### 代码
```java
//hashmap
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        HashMap<String, Integer> map = new HashMap<>();
        List<Integer> res = new ArrayList<>();
        int wordLength = words[0].length();
        for(String word: words){
            map.put(word, map.getOrDefault(word, 0)+1);     
        }
        
        HashMap<String, Integer> found = new HashMap<>();
        for(int i = 0;i <= s.length() - words.length * wordLength;i++){
            found.clear();
            int cur = i;
            int size = 0;
            for(int j = 0;j < words.length;j++){
                String str = s.substring(cur, cur + wordLength);
                if(map.containsKey(str)){
                    found.put(str, found.getOrDefault(str, 0)+1);
                    if(found.get(str) > map.get(str)) break;
                    size++;
                } else {
                    break;
                }
                cur = cur + wordLength;
            }
            if(size == words.length){
                res.add(i);
            }
        }
        return res;      
    }
}

//不可能的words排列组合，还是记录一下backtrack
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<String> res = backtrack(Arrays.asList(words), new ArrayList<>(), "", words.length*words[0].length());
        System.out.println(res);
        
        return new ArrayList<>();
    }
    
    public List<String> backtrack(List<String> words, List<String> res, String combination, int size){
        if(size == combination.length()) {
            res.add(combination);
            return res;
        }
        for(int i = 0;i < words.size();i++){
            if(words.get(i) == "") continue;
            String temp = combination;
            String word = words.get(i);
            combination += word;
            words.set(i, "");
            backtrack(words, res, combination, size);
            combination = temp;
            words.set(i, word);
        }
        return res;
    }
}


```

### 复杂度

time:
- 假设s的长度为N，words的长度为M，每个单词长度为L
    - 需要O(M)把words放入hashmap
    - 需要N - M * L次来检查每个位置
        - 需要M次来看是否找到单词
总共需要M + (N - M * L) * M = M + N*M - M^2 * L

space：map所需的空间O(N)