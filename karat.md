```java
public class actionList {
    public static List<String> lcs(String[] str1,String[] str2) {
     int m = str1.length;
  int n = str2.length;
  List<String> res=new ArrayList<>();
  int max = 0;
  int[][] dp = new int[m][n];
  int end=-1;
  for(int i=0; i<m; i++){
   for(int j=0; j<n; j++){
    if(str1.equals(str2[j])){
     if(i==0 || j==0)dp[j]=1;
     else dp[j] = dp[i-1][j-1]+1;                                
     if(max < dp[j]){
      max = dp[j];
      end=i;
     }
    }
   }
  }
  for(int i=end-max+1;i<end+1;i++)
   res.add(str1);

  return res;
    }

    public static void main(String[] args) {
     String[] user0 = { "/nine.html", "/four.html", "/six.html", "/seven.html", "/one.html" };
     String[] user2 = { "/nine.html", "/two.html", "/three.html", "/four.html", "/six.html", "/seven.html" };
     String[] user1 = { "/one.html", "/two.html", "/three.html", "/four.html", "/six.html" };
     String[] user3 = { "/three.html", "/eight.html" };
     List<String>  res1=lcs(user0, user2);
     List<String>  res2=lcs(user1, user2);
     List<String>  res3=lcs(user0, user3);
     List<String>  res4=lcs(user1, user3);
        System.out.println(res1);
        System.out.println(res2);
        System.out.println(res3);
        System.out.println(res4);
    }


class TrieNode {
        String word;
        TrieNode[] next = new TrieNode[26];
    }
    TrieNode root;

    public autoComplete(String[] words) {
        root = new TrieNode();
        for (String word : words)
            add(root, word);
    }

    public void add(TrieNode root, String word) {
        TrieNode cur = root;
        for (char c : word.toCharArray()) {
            if (cur.next[c-'a'] == null)
                cur.next[c-'a'] = new TrieNode();
            cur = cur.next[c-'a'];
        }
        cur.word = word;
    }

    public List<String> findAllWords(String prefix) {
        List<String> res = new ArrayList<>();
        if (prefix == null || prefix.length() == 0)
            return res;

        TrieNode cur = root;
        for (char c : prefix.toCharArray()) {
            if (cur.next[c-'a'] == null)
                return null;
            cur = cur.next[c-'a'];
        }
        DFS(res, cur);
        return res;
    }

    private void DFS(List<String> res, TrieNode root) {
        if (root.word != null) {
            res.add(root.word);
        }

        // continue explore
        for (int i = 0; i < 26; i++) {
            if (root.next != null) {
                DFS(res, root.next);
            }
        }

    }

    public static void main(String[] args) {
        String[] words = {
                "marketing",
                "make",
                "stop",
                "development",
                "develop",
                "dev"
        };
        autoComplete sol = new autoComplete(words);
        System.out.println(sol.findAllWords("de"));
    }


public static int calculate(String s) {
        int res = 0;
        int sign = 1;
        int cur = 0;
        char[] sc = s.toCharArray();
        for(int i = 0; i < sc.length; i++) {
            if(sc >= '0' && sc <= '9') {
                cur = 0;
                while(i < sc.length && sc >= '0' && sc <= '9') {
                    cur = cur * 10 + (sc - '0');
                    i++;
                }
                i--;
            } else if(sc == '+' || sc == '-'){               
                res += cur * sign;
                sign = sc == '+'? 1: -1;
                cur = 0;
            }
        }
        res += cur * sign;
        return res;  
}


static int calculatorPathesis(String s) {
      if (s == null || s.length() == 0) {
             return 0;
         }
         Stack<Integer> stack = new Stack<>();
         char[] chars = s.toCharArray();
         int res = 0;
         int num = 0;
         int sign = 1;
         stack.push(sign);
         for (int i = 0; i < chars.length; i++) {
             if (chars >= '0' && chars <= '9') {
                 num = num * 10 + (chars - '0');
             } else if (chars == '+' || chars == '-') {
                 res += sign * num;
                 sign = stack.peek() * (chars == '+' ? 1 : -1);
                 num = 0;
             } else if (chars == '(') {
                 stack.push(sign);
             } else if (chars == ')') {
                 stack.pop();
             }
         }
         res += sign * num;
         return res;
}

public static String calculatorWords(String s, Map<String, Integer> map) {
     if (s == null || s.isEmpty()) {
         return "";
     }
     String res = "";
     Deque<Integer> signStack = new LinkedList<>();
     int sign = 1;
     signStack.push(1);
     List<String> notMappedVariables = new ArrayList<>();
     for (int i = 0; i < s.length(); i++) {
         char ch = s.charAt(i);
         if(ch == '(') {
             signStack.push(sign * signStack.peek());
             sign = 1;
         } else if (ch == ')') {
             signStack.pop();
         } else if (ch == '+') {
             sign = 1;
         } else if (ch == '-') {
             sign = -1;
         } else if (Character.isDigit(ch)) {
             int num = ch - '0';
             while (i + 1 < s.length() && Character.isDigit(s.charAt(i + 1))) {
                 num = num * 10 + s.charAt(++i) - '0';
             }
             String prefix = sign * signStack.peek() == 1 ? "+":"-";
             res += prefix + num;
         } else if (Character.isLowerCase(ch)) {
             String v = "" + ch;
             while (i + 1 < s.length() && Character.isLowerCase(s.charAt(i + 1))) {
                 v += s.charAt(++i);
             }
             if (map.containsKey(v)) {
                 v = "" + map.get(v);
                 String prefix = sign * signStack.peek() == 1 ? "+":"-";
                 res += prefix + v;
             } else {
                 String variableString = (signStack.peek() * sign == 1 ? "+" : "-") + v;
                 notMappedVariables.add(variableString);
             }
         }
     }
     System.out.println(notMappedVariables);
     res = "" + calculate(res);
     for (String each : notMappedVariables) {
         res += each;
     }
     return res;
}

public static void main(String[] args) {
  // TODO Auto-generated method stub
        int res=calculate("2+6-1+111-100");
        System.out.println(res);
        int res2=calculatorPathesis("2+((8+2)+(3-999))");
        System.out.println(res2);
        Map<String, Integer> map = new HashMap<>();
        map.put("temperature", 5);
        map.put("e", 1);
        map.put("y", 2);
        String resWords=calculatorWords("(e+8) - (pressure + 3 - (temperature + 8) + y) +yy",map);
        System.out.println(resWords);
}

public class commonAncestor {
static Map<Integer,List<Integer>> map;
static List<Integer> findparent(int[][] edges){
  if(edges==null || edges.length==0) {
      return new ArrayList<>();
     }
  List<Integer> res1=new ArrayList<>();
  List<Integer> res2=new ArrayList<>();
     map = new HashMap<>();
     for(int[] edge : edges) {
         List<Integer> list = map.get(edge[1]);
      if(list == null) {
         list = new ArrayList<>();
         }
         list.add(edge[0]);
         map.put(edge[1],list);
         list = map.get(edge[0]);
         if(list == null) {
          list = new ArrayList<>();
         }
         map.put(edge[0],list);
     }
     for(Map.Entry<Integer,List<Integer>> entry: map.entrySet()){
      if(entry.getValue().size()==0) {
       res1.add(entry.getKey());
         }else if (entry.getValue().size()==1) {
          res2.add(entry.getKey());
         }
     }
     return res2;
}

static  boolean hasCommonAncestor(int[][] edges, int a, int b) {
  Set<Integer> a_parent= new HashSet<>();
     getall(map,a,a_parent);
  Set<Integer> b_parent = new HashSet<>();
     getall(map,b,b_parent);        
     for(int n: b_parent) {
      if(a_parent.contains(n)) {
          return true;
         }
     }
     return false;
}

    static void getall(Map<Integer,List<Integer>> map, int a, Set<Integer> parent) {
     List<Integer> list = map.get(a);
     if(list == null || list.size()==0) {
      return;
     }
     for(int p : list) {
      parent.add(p);
      getall(map,p,parent);
     }
}

    static  int furthestAncestor(int a, Map<Integer, List<Integer>> map) {
        Queue<Integer> q = new LinkedList<>();
        q.offer(a);
        int size = q.size();
        int ances = a;
        while(!q.isEmpty()){
            ances = q.peek();
            while(size-- > 0) {
                int cur = q.poll();
                for(Integer p: map.get(cur)) {
                    q.offer(p);
                }
            }
            size = q.size();
        }
        return ances;
    }

     static int lowestCommon(int i1, int i2, Map<Integer, List<Integer>> parent) {
         Set<Integer> parent1 = new HashSet<>();
         getall(parent,i1, parent1);
         if(parent1.contains(i2)){
             return i2;
         }
         Queue<Integer> q = new LinkedList<>();
         q.offer(i2);
         while(!q.isEmpty()){
             int cur = q.poll();
             if(parent1.contains(cur)){
                 return cur;
             }
             for(Integer p: parent.get(cur)) {
                 q.offer(p);
             }
         }
         return -1;
     }


public static void main(String[] args) {
  // TODO Auto-generated method stub

/*        
      14   13
       \    \
  2   1    4   12
   \ /   / | \ /
    3   5  8  9
     \ / \     \
      6   7    11
*/     

  int[][] inputs= {
    {1, 3},
          {2, 3},
          {3, 6},
          {5, 6},
          {5, 7},
          {4, 5},
             {4, 8},
             {4, 9},
             {9, 11},
             {14, 4},
             {13, 12},
             {12, 9}
           };

  List<Integer> res=findparent(inputs);
  System.out.println(res);
     boolean res1=hasCommonAncestor(inputs,3,8);
     boolean res2=hasCommonAncestor(inputs,5,8);
     boolean res3=hasCommonAncestor(inputs,6,1);
     boolean res4=hasCommonAncestor(inputs,6,9);
     boolean res5=hasCommonAncestor(inputs,1,3);
     boolean res6=hasCommonAncestor(inputs,7,11);
     boolean res7=hasCommonAncestor(inputs,6,5);
        int farestAncestor=furthestAncestor(6, map);
        int lowestCommonAncestor=lowestCommon(6,1,map);
     System.out.println();
     System.out.println();
     System.out.println(res1);
     System.out.println(res2);
     System.out.println(res3);
     System.out.println(res4);           
     System.out.println(res5);        
     System.out.println(res6);
     System.out.println(res7);
     System.out.println(farestAncestor);
     System.out.println(lowestCommonAncestor);
}


static List<List<String>> shareClass(String[][] student_course_pairs_1){
  Map<String,HashSet<String>> map=new HashMap<>();
  List<String> stu=new ArrayList<>();
  List<List<String>> res=new ArrayList<>();
  for(String[] str:student_course_pairs_1) {
   if(!stu.contains(str[0]))stu.add(str[0]);
   if(map.get(str[0])==null) {
    HashSet<String> set=new HashSet<>();
       map.put(str[0],set);
   }
   map.get(str[0]).add(str[1]);
  }
        for(int i=0;i<stu.size();i++) {
         for(int j=i+1;j<stu.size();j++) {
          HashSet<String> in1=map.get(stu.get(i));
          HashSet<String> in2=map.get(stu.get(j));
          List<String> l=new ArrayList<>();
          l.add(stu.get(i));
          l.add(stu.get(j));
          for(String s:in1) {
           if(in2.contains(s)){
            l.add(s);
           }
          }
          if(l.size()==2)l.add("[]");
          res.add(l);
         }
        }

  return res;
}

static Map<String,String> map;
static String findTopNodeWithNoParent(String[][] edges){
  String res="zero";
  Set<String> ele=new HashSet<String>();
  for(String[] s:edges) {
   ele.add(s[0]);
   ele.add(s[1]);
  }
  if(edges==null || edges.length==0) {
      return res;
     }

     map = new HashMap<>();
     for(String[] edge : edges) {
            map.put(edge[0], edge[1]);
     }
     Set<String> valSet=new HashSet<String>();
     for(Map.Entry<String,String> entry: map.entrySet()){
            valSet.add(entry.getValue());
     }
     for(String s:ele) {
      if(!valSet.contains(s))return s;
     }
     return res;
}

static String getMiddleCourseOfChain(String[][] pre,Map<String,String> map) {
  String topOlestCourse=findTopNodeWithNoParent(pre);
  List<String> chain=new ArrayList<>();
        String son=map.get(topOlestCourse);
        chain.add(topOlestCourse);
        int index=0;
        while(son!=null) {
         chain.add(son);
         son=map.get(son);
        }
        System.out.print(chain);
        if(chain.size()%2==0) {
         index=chain.size()/2-1;
        }else {
         index=chain.size()/2;
        }
        return chain.get(index);
}



public static void main(String[] args) {
  // TODO Auto-generated method stub
  String[][] student_course_pairs_1 = {
                         {"58", "Software Design"},
                         {"58", "Linear Algebra"},
                         {"94", "Art History"},
                         {"94", "Operating Systems"},
                         {"17", "Software Design"},
                         {"58", "Mechanics"},
                         {"58", "Economics"},
                         {"17", "Linear Algebra"},
                         {"17", "Political Science"},
                         {"94", "Economics"},
                         {"25", "Economics"},
  };
  String[] s1={"58", "17"};
  String[] s2={"58", "94"};
  String[] s3={"58", "25"};
  String[] s4={"94", "25"};
  String[] s5={"17", "25"};
  String[] s6={"17", "94"};
  List<List<String>> res1=shareClass(student_course_pairs_1);
  //List<String> res2=shareClass(student_course_pairs_1,s2);
  //List<String> res3=shareClass(student_course_pairs_1,s3);
  //List<String> res4=shareClass(student_course_pairs_1,s4);
//        List<String> res5=shareClass(student_course_pairs_1,s5);
//        List<String> res6=shareClass(student_course_pairs_1,s6);        
  for(List<String> l:res1) {
   System.out.println(l);
  }
//        System.out.println(res2);
//        System.out.println(res3);
//        System.out.println(res4);
//        System.out.println(res5);
//        System.out.println(res6);
  String[][] pre={{"A","B"},{"B","C"},{"C","D"},{"D","E"},{"E","F"}};
  String res=findTopNodeWithNoParent(pre);
  System.out.println(res);
  String resMiddle=getMiddleCourseOfChain(pre,map);
  System.out.println(resMiddle);
}

static void getGroup(String[][] records) {
        Map<String, Integer> map = new HashMap<>();
        Set<String> exitWithoutBadge = new HashSet<>();
        Set<String> enterWithoutBadge = new HashSet<>();  
        for(String[] record: records) {
            Integer prev = map.get(record[0]);
            if(record[1].equals("enter")) {
                if(prev != null && prev == 1) {
                    exitWithoutBadge.add(record[0]);
                }
                prev = 1;
            } else {
                if(prev == null || prev == 0) {
                    enterWithoutBadge.add(record[0]);
                }
                prev = 0;
            }
            map.put(record[0], prev);
        }

        for(String person: map.keySet()){
            if(map.get(person) > 0) {
                exitWithoutBadge.add(person);
            }
        }
        printSet("enter without badge " , enterWithoutBadge);
        printSet("exit without badge " , exitWithoutBadge);
    }

    static Map<String, List<Integer>> security(String[][] records) {
        Map<String, List<Integer>> map = new HashMap<>();
        for(String[] record: records) {
            if(!map.containsKey(record[0])) {
                map.put(record[0], new ArrayList<>());
            }
            map.get(record[0]).add(Integer.parseInt(record[1]));

        }
        Map<String, List<Integer>> res = new HashMap<>();
        for(String person: map.keySet()) {
            if(map.get(person).size() >= 3) {
                List<Integer> cur = map.get(person);
                Collections.sort(cur);
                for(int i = 0; i < cur.size(); i++) {
                    int index = oneHour(cur, i);
                    if(index - i >= 3) {
                        List<Integer> temp = new ArrayList<>();
                        while( i < index) {
                            temp.add(cur.get(i));
                            i++;
                        }
                        res.put(person, temp);
                        break;
                    }
                }
            }
        }
        printMap("secure ", res);
        return res;
    }

    static int oneHour(List<Integer> list, int startIndex) {
        int endVal = list.get(startIndex) + 100;
        int endPos = startIndex;
        while(endPos < list.size()) {
            if(list.get(endPos) <= endVal) {
                endPos++;
            } else {
                break;
            }
        }
        return endPos;
    }

    static void printSet(String s, Set<String> set) {
        System.out.println(s);
        for(String i: set){
            System.out.println(i + " ");
        }
        System.out.println();
    }

    static void printList(String s, List<String> list) {
        System.out.println(s);
        for(String i: list){
            System.out.println(i + " --> ");
        }
        System.out.println();
    }

    static void printListInt(String s, List<Integer> list) {
        System.out.println(s);
        for(Integer i: list){
            System.out.println(i + " --> ");
        }
        System.out.println();
    }

    static void printMap(String s, Map<String, List<Integer>> map) {
        System.out.println(s);
        for(String ss: map.keySet()){
            printListInt(ss, map.get(ss));
        }
        System.out.println();
    }

    // Driver Code
    public static void main(String args[]) {
        String[][] records = new String[][]{
            {"Martha","exit"},
            {"Paul","enter"},
            {"Martha","enter"},
            {"Martha","exit"},
            {"Jennifer","enter"},
            {"Paul","enter"},
            {"Curtis","enter"},
            {"Paul","exit"},
            {"Martha","enter"},
            {"Martha","exit"},
             {"Jennifer", "exit"},
        };
        String[][] records2 = new String[][]{
            {"Paul","1355"},
            {"Jennifer","1910"},
            {"John","830"},
            {"Paul","1315"},
            {"John","835"},
            {"Paul","1405"},
            {"Paul","1630"},
            {"John","855"},
            {"John","915"},
            {"John","930"},
            {"Jennifer","1335"},
            {"Jennifer","730"},
            {"John","1630"},
        };
        getGroup(records);
        System.out.println(security(records2));
    }

public static  Map<Integer, Set<Integer>> generateMap(String[] employees, String[] friendships) {
  Map<Integer, Set<Integer>> friendMap = new HashMap<>();
     for (String employee : employees) {
      String[] parts = employee.split(",");
            int employeeId = Integer.parseInt(parts[0]);
            friendMap.put(employeeId, new HashSet<>());
     }
     for (String friendship : friendships) {
      String[] parts = friendship.split(",");
      int id1 = Integer.parseInt(parts[0]);
      int id2 = Integer.parseInt(parts[1]);
      friendMap.get(id1).add(id2);
      friendMap.get(id2).add(id1);
  }
     return friendMap;
}

public static List<int[]> count(String[] employees, String[] friendships) {
  List<int[]> resList=new ArrayList<>();
  Map<String,Set<Integer>> departMap=new HashMap<>();
  Map<Integer, Set<Integer>> friendMap=generateMap(employees,friendships);
  for(String employee:employees) {
      String[] parts = employee.split(",");
            int employeeId = Integer.parseInt(parts[0]);
            String depart= parts[2];
   departMap.computeIfAbsent(depart, x->new HashSet<Integer>()).add(employeeId);               
  }
  for(Entry<String,Set<Integer>> entry:departMap.entrySet()) {
   Set<Integer> emps=entry.getValue();
   int[] res=new int[2];
   res[0]=emps.size();
   int friendNotInDepart=0;
   for(int id:emps) {
       Set<Integer> friends=friendMap.get(id);
       for(int friend:friends) {
        if(!emps.contains(friend))friendNotInDepart++;
       }
   }
   res[1]=friendNotInDepart;
   resList.add(res);
   System.out.println("depart"+entry.getKey());
   System.out.println("depart employee number "+res[0]);
   System.out.println("depart friend not in the same "+res[1]);
  }
  return resList;
}

public static void main(String[] args) {
  // TODO Auto-generated method stub

  String[] employeesInput = {
    "1,Richard,Engineering",
    "2,Erlich,HR",
    "3,Monica,Business",
    "4,Dinesh,Engineering",  
    "6,Carla,Engineering",  
    "9,Laurie,Directors"
    };        


  String[] friend= {"1,2","1,3","1,6","2,4"};
  Map<Integer, Set<Integer> > res= generateMap( employeesInput, friend);
  System.out.println(res);
  List<int[]> resList=count(employeesInput, friend);
  for(int[] a:resList)
   System.out.println(a[0]+" "+a[1]);
}


    public static List<int[]> findLegalMoves(int[][] board, int startI, int startJ) {
      int[][] dir = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
      int m = board.length, n = board[0].length;
      List<int[]> res = new ArrayList<>();
      for(int[] d: dir) {
          int i = startI + d[0];
          int j = startJ + d[1];
          if(i >= 0 && i < m && j >= 0 && j < n && board[j] == 0) {
              res.add(new int[]{i,j});
          }
      }
      return res;
    }


    public static boolean isAllPositionsPassed(int[][] board, int endI, int endJ) {
        int m = board.length, n = board[0].length;
        int[][] boardCopy = new int[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                boardCopy[j] = board[j];
            }
        }
        boolean connected = false;
        for(int i = 0;i < m; i++){
            for(int j = 0; j < n; j++){
                if(boardCopy[j] == 0) {
                    if(!connected) {
                        connected = true;
                        DFS(boardCopy, i, j);
                    } else {
                        return false;
                    }                        
                }
            }  
        }
        return true;                    
    }




    public static void DFS(int[][] board, int startI, int startJ) {
        int[][] dir = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};
        int m = board.length, n = board[0].length;            
        board[startI][startJ]= 1;

        for(int[] d: dir) {
            int i = startI + d[0];
            int j = startJ + d[1];
            if(i >= 0 && i < m && j >= 0 && j < n && board[j] == 0) {
                DFS(board, i, j);
            }
        }
    }


    public static boolean isReachable(int[][] board,int[] start,int[] end){
    int m=board.length;
    int n=board[0].length;
    int[][]dirs = new int[][]{{1,0}, {-1,0}, {0,1}, {0,-1}};
    boolean visited[][]=new boolean[m][n];
    Queue<int[]> queue = new LinkedList<>();
          if( start[0] < 0 || start[1] < 0 || start[0] >=m || start[1] >=n  || board[start[0]][start[1]]!=0){
              return false;
          }
          if( end[0] < 0 || end[1] < 0 || end[0] >=m || end[1] >=n  || board[end[0]][end[1]]!=0){
              return false;
          }
    queue.offer(new int[]{start[0],start[1]});

    while(!queue.isEmpty()){
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            int[] curNode = queue.poll();
            int x = curNode[0];
            int y = curNode[1];

            if( x < 0 || y < 0 || x >=m || y >=n || visited[x][y]==true || board[x][y]!=0){
                continue;
            }
            visited[x][y]=true;
            if(x==end[0] && y==end[1]){
                return true;
            }
            for(int[] dir : dirs){
                int new_x = dir[0] + x;
                int new_y = dir[1] + y;
                queue.offer(new int[]{new_x, new_y});
            }
        }
    }
    return false;
      }


    public static void dfs2(int[][] board, int i, int j, int endI, int endJ, int count, int totalTreasure, List<int[]> cur, List<List<int[]>> res) {
        int m = board.length, n = board[0].length;
        int[][] dir = new int[][]{{1,0},{-1,0},{0,1},{0,-1}};

        if(i >= 0 && i < m && j >= 0 && j < n) {
            if(board[j] == 1 || board[j] == 0) {
             cur.add(new int[]{i,j});
                if(board[j] == 1){
                    count++;
                }
                int temp = board[j];
                board[j] = 2;
                if( i == endI && j == endJ && count == totalTreasure) {
                    res.add(new ArrayList<>(cur));
                    cur.remove(cur.size()-1);
                    board[j] = temp;
                    return;
                }
                for(int[] d:dir) {
                    int newi = i + d[0];
                    int newj = j + d[1];
                    dfs2(board, newi, newj, endI, endJ, count, totalTreasure, cur, res);
                }
                board[j] = temp;
                cur.remove(cur.size() -1);
            }
        }
    }

    public static List<int[]> treasure(int[][] board, int startI, int startJ, int endI, int endJ) {
        int m = board.length, n = board[0].length;
        int totalTreasure = 0;
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(board[j] == 1) {
                    totalTreasure++;
                }
            }
        }
        int[][] boardCopy = new int[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                boardCopy[j] = board[j];
            }
        }
        List<List<int[]>> paths = new ArrayList<>();
        dfs2(boardCopy, startI, startJ, endI, endJ, 0, totalTreasure, new ArrayList<>(), paths);
        List<int[]> shortest = paths.get(0);
        for(int i = 1; i < paths.size(); i++) {
            if(paths.get(i).size() < shortest.size()) {
                shortest = paths.get(i);
            }
        }
        return shortest;
    }




    public static void printList(List<int[]> res1) {
      for(int[] array: res1) {
        System.out.println(array[0] +" " + array[1]);
      }
    }

    public static void main(String[] args) {
      int[][] board = new int[][] {
        { 0,  0,  0, 0, -1 },
        { 0, -1, -1, 0,  0 },
        { 0,  0,  0, 0,  0 },
        { 0, -1,  0, 0,  0 },
        { 0,  0,  0, 0,  0 },
        { 0,  0,  0, 0,  0 },
      };
      int[][] board2 = new int[][] {
        {  0,  0,  0, 0, -1 },
        {  0, -1, -1, 0, -1 },
        { -1, -1,  0, 0,  0 },
        { -1,  0, -1, 0,  0 },
        { -1,  0, -1, 0,  0 },
        {  0,  0,  0, 0,  0 },
      };
      int[][] board3 = new int[][] {
        {  0,  0,  0,  0,  0 },
        {  0, -1, -1,  0,  0 },
        {  0, -1,  0,  1,  0 },
        { -1,  0,  0,  0,  0 },
        {  0,  1, -1,  0,  0 },
        {  0,  0,  0,  0,  0 },
      };


      int[] start = new int[] { 5, 0 };
      int[] end = new int[] { 0, 4 };


      List<int[]> res1 = findLegalMoves(board, start[0], start[1]);

      //List<int[]> res2 = position(board, 5, 4);
     // for(int[]a:res1) {
     //         for(int i:a)System.out.print(i+" ");
    //      System.out.println();
    //  }

      //for(int[]b:res1)System.out.print(b);
      //printList(res2);

     //  System.out.println(isAllPositionsPassed(board3, 0, 0));
     //  System.out.println(isAllPositionsPassed(board, 0, 0));


       int[] start1 = new int[] { 0, 0 };
       int[] end1 = new int[] { 1, 0 };
       int[] end2 = new int[] { 2, 2 };
       int[] end3 = new int[] { 3, 1 };
       System.out.println(isReachable(board,start1,end1));
       System.out.println(isReachable(board2,start1,end1));
       System.out.println(isReachable(board2,start1,end2));
       System.out.println(isReachable(board2,start1,end3));
      List<int[]> res3 = treasure(board3, start[0], start[1], end[0], end[1]);

      printList(res3);

    }

  public static  boolean isNoneOverlap(int[][] intervals,int[] interval) {
     int start=interval[0],end=interval[1];
     for(int[] cur:intervals) {
      if( (cur[0]<start&& start<cur[1])||(cur[0]<end && end<cur[1])  )return false;
     }
     return true;
    }

    public static List<int[]> merge(int[][] intervals){

      Arrays.sort(intervals,(i1,i2) -> Integer.compare(i1[0], i2[0]));
      List<int[]> res=new ArrayList<>();
      if(intervals.length<=1)return res;
      int[] newInterval=intervals[0];
      res.add(newInterval);
      for(int[] interval:intervals) {
       if(interval[0]<=newInterval[1]) {
        newInterval[1]=Math.max(newInterval[1], interval[1]);
       }else {
        newInterval=interval;
        res.add(newInterval);
       }
      }

     for(int[] a:res)
     System.out.println(a[0]+" "+a[1]);
     return res;
    }

    public static List<int[]> freeTime(int[][] intervals){
     List<int[]> res=new ArrayList<>();
     List<int[]> busyTime=merge(intervals);
     res.add(new int[] {0,busyTime.get(0)[0]});
     for(int i=0;i<busyTime.size()-1;i++) {
      res.add(new int[] {busyTime.get(i)[1],busyTime.get(i+1)[0]});
     }
     res.add(new int[] {busyTime.get(busyTime.size()-1)[1],2400});
     return res;
    }





    public static void main(String[] args) {
     int[][] intervals1 = { {1300, 1500}, {930, 1200},{830, 845},{820,830} };
     int[] interval1= {820,830};
     int[] interval2= {1450,1500};
     System.out.println(isNoneOverlap(intervals1,interval1));
     System.out.println(isNoneOverlap(intervals1,interval2));
     List<int[]> res=freeTime(intervals1);
     for(int[] a:res) {
         System.out.print(a[0]+" "+a[1]);
         System.out.print(",");
     }
    }


public class oneAndMultipleRectangle {
    static int[] rectangle1 (int[][] matrix) {
        int[] res = new int[4];
        int row = matrix.length, col = matrix[0].length;
        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                if(matrix[j] == 0) {
                    int iRight = i, jDown = j;
                    while(iRight < row) {
                        if(matrix[iRight][j] == 0) {
                            iRight++;
                        } else {
                            break;
                        }
                    }
                    while(jDown < col) {
                        if(matrix[jDown] == 0) {
                            jDown++;
                        } else {
                            break;
                        }
                    }
                    return new int[]{i, j, iRight-1, jDown-1};
                }
            }
        }
        return res;
    }


    static List<int[]> rectangleMulti(int[][] matrix) {
        List<int[]> res = new ArrayList<>();
        int row = matrix.length, col = matrix[0].length;
        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                if(matrix[j] == 0) {
                    int iRight = i, jDown = j;
                    while(iRight < row) {
                        if(matrix[iRight][j] == 0) {
                            iRight++;
                        } else {
                            break;
                        }
                    }
                    while(jDown < col) {
                        if(matrix[jDown] == 0) {
                            jDown++;
                        } else {
                            break;
                        }
                    }
                    res.add(new int[]{i, j, iRight-1, jDown-1});
                    fill(i,j, iRight-1, jDown-1, matrix);
                }
            }
        }
        printListArray("", res);
        return res;
    }


    static List<List<int[]>> irregular(int[][] matrix) {
        List<List<int[]>> res = new ArrayList<>();
        int row = matrix.length, col = matrix[0].length;
        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                if(matrix[j] == 0) {
                    List<int[]> cur = new ArrayList<>();
                    DFS(matrix, i, j, row, col, cur);
                    res.add(cur);
                }
            }
        }
        for(List<int[]> r: res) {
            printListArray("", r);
        }
        return res;
    }



    static void DFS(int[][] matrix, int i, int j, int row, int col, List<int[]> cur) {
        int[][] dir = new int[][] {{-1,0}, {1,0}, {0,-1}, {0,1}};
        if(i<0 || i>= row || j<0 || j>=col || matrix[j] == 1) {
            return;
        }
        cur.add(new int[]{i,j});
        matrix[j] = 1;
        for(int[] k: dir) {
            int i2 = i+k[0], j2 = j+k[1];
            DFS(matrix, i2, j2, row, col, cur);
        }
    }

    static void fill(int i1, int j1, int i2, int j2, int[][] matrix) {
        for(int i = i1; i <= i2; i++) {
            for(int j = j1; j <= j2; j++) {
                matrix[j] = 1;
            }
        }
    }

    static void printArray(String s, int[] array) {
        System.out.println(s);
        for(int i: array){
            System.out.print(i + " ");
        }
        System.out.println();
    }

    static void printSet(String s, Set<String> set) {
        System.out.println(s);
        for(String i: set){
            System.out.println(i + " ");
        }
        System.out.println();
    }

    static void printList(String s, List<String> list) {
        System.out.println(s);
        for(String i: list){
            System.out.println(i + " --> ");
        }
        System.out.println();
    }

    static void printListArray(String s, List<int[]> list) {
        System.out.println(s);
        for(int[] i: list){
            printArray("",i);
        }
        System.out.println();
    }

    static void printListInt(String s, List<Integer> list) {
        System.out.println(s);
        for(Integer i: list){
            System.out.println(i + " --> ");
        }
        System.out.println();
    }

    static void printMap(String s, Map<String, List<Integer>> map) {
        System.out.println(s);
        for(String ss: map.keySet()){
            printListInt(ss, map.get(ss));
        }
        System.out.println();
    }

    // Driver Code
    public static void main(String args[]) {
        int[][] matrix = new int[][]{
            {1,1,1,1,1,1},
            {0,0,1,0,1,1},
            {0,0,1,0,1,0},
            {1,1,1,0,1,0},
            {1,0,0,1,1,1}
        };

        int[][] matrix2 = new int[][]{
            {1,1,1,1,1,1},
            {0,0,1,0,0,1},
            {0,0,1,0,1,0},
            {0,1,0,0,1,0},
            {1,0,0,1,0,1}
        };

        printArray("only one " ,rectangle1(matrix));
        rectangleMulti(matrix);
        irregular(matrix2);
    }

public class subdomainVisitCount {
     public List<String> subdomainVisits(String[] inputs) {
      Map<String, Integer> map = new HashMap<>();               
         for (String s : inputs) {
             String[] strs = s.split("\\s+");
             int count = Integer.parseInt(strs[0]);
             String[] domains = strs[1].split("\\.");
             String temp = domains[domains.length-1];
             int domainLen = domains.length-1;                    
             while(domainLen >= 0) {
                 map.put(temp, map.getOrDefault(temp, 0) + count);
                 if (domainLen > 0) temp = domains[domainLen-1] + "." + temp;                        
                 domainLen--;
             }                    
         }               
         List<String> res = new ArrayList<>();
         for (Map.Entry<String, Integer> entry : map.entrySet()) {
             String str = String.valueOf(entry.getValue()) + " " + entry.getKey();
             res.add(str);
         }               
         return res;  
     }

   public static void main(String[] args) {
    subdomainVisitCount test=new subdomainVisitCount();
    String[] counts  ={"900 go‍‌‌‌‌‍‍‍‍‌‌‌‍‍‍‍‌‌ogle.com", "60 mail.yahoo.com","10 mobile.sports.yahoo.com","40 sports.yahoo.com","300 yahoo.com","10 stackoverflow.com","2 en.wikipedia.org", "1 es.wikipedia.org","1 mobile.sports"};
    List<String> res=test.subdomainVisits(counts);
    for(String s:res)
        System.out.println(s);
   }


public static List<String> wrap(String[] words,int maxLen){
  List<String> res=new ArrayList<>();
  res.add(words[0]);
  for(int i=1;i<words.length;i++) {
   String lastWord=res.get(res.size()-1);
   if(lastWord.length()+words.length()+1<=maxLen) {
    lastWord+=("-"+words);
    res.set(res.size()-1, lastWord);
   }else {
    res.add(words);
   }
  }
  return res;
}

public static void main(String[] args) {
  // TODO Auto-generated method stub
  String[] words1 = { "The", "day", "began", "as", "still", "as", "the", "night", "abruptly", "lighted", "with", "brilliant", "flame" };
  int maxLen1=13;
  System.out.println(wrap(words1,maxLen1));
  String[] words2= {"Hello","world"};
  int maxLen2=5;
  System.out.println(wrap(words2,maxLen2));
}
```