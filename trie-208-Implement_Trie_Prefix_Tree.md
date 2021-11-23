### 思路

- 拿到题的直觉解法
    - 手动实现trie，模版背诵题

`AC`


### 代码
```java
class Trie {

    TrieNode root;
    
    public Trie() {
        root = new TrieNode();    
    }
    
    public void insert(String word) {
        TrieNode node = root;
        for(int i = 0;i < word.length();i++){
            if(node.children[word.charAt(i) - 'a'] == null){
                node.children[word.charAt(i) - 'a'] = new TrieNode();
            }
            node = node.children[word.charAt(i) - 'a'];
            node.preCount++;
        }
        node.count++;
    }
    
    public boolean search(String word) {
        TrieNode node = root;
        for(int i = 0;i < word.length();i++){
            if(node.children[word.charAt(i) - 'a'] == null){
                return false;
            }
            node = node.children[word.charAt(i) - 'a'];
        }
        return node.count > 0;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode node = root;
        for(int i = 0;i < prefix.length();i++){
            if(node.children[prefix.charAt(i) - 'a'] == null){
                return false;
            }
            node = node.children[prefix.charAt(i) - 'a'];
        }
        return node.preCount > 0;
    }
    
    private class TrieNode{
        int count;
        int preCount;
        TrieNode[] children;
        
        TrieNode(){
            count = 0;
            preCount = 0;
            children = new TrieNode[26];
        }
    }
}
```


### 复杂度

time: O(1)建树，其他O(N)
space: 同前缀的单词越多越好，否则没意义。