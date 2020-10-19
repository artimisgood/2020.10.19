# 2020.10.19————电话号码的字母组合
## 题目
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/original_images/17_telephone_keypad.png)

示例:
```
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```

说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

## 解题
### 语言
java
### 思路
- 1.首先使用哈希表存储每个数字对应所有可能的字母，然后进行回溯操作。
- 2.回溯过程中维护一个字符串，表示已有的字母排列（如果未遍历完电话号码的所有数字，则已有的字母排列不完整）。该字符串初始为空。每次取电话号码的一位数字，从哈希表中获得该数字对应的所有可能的字母，并将其中的一个字母插入到已有的字母排列后面，然后继续处理电话号码的后一位数字，直到处理完电话号码中的所有数字，即得到一个完整的字母排列。然后进行回退操作，遍历其余的字母排列。
![](https://assets.leetcode-cn.com/solution-static/17/14.png)
### 代码
```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> combinations = new ArrayList<String>();//新建结果数组
        if(digits.length()==0){
            return combinations;//如果输入为空，则直接返回初始值null
        }
        Map<Character,String> phoneMap = new HashMap<Character,String>(){{//创建哈希表
            put('2',"abc");
            put('3', "def");
            put('4', "ghi");
            put('5', "jkl");
            put('6', "mno");
            put('7', "pqrs");
            put('8', "tuv");
            put('9', "wxyz");
        }
        };
        backtrack(combinations,phoneMap,digits,0,new StringBuffer());
        return combinations;
    }

    public void backtrack(List<String> combinations,Map<Character,String> phoneMap,
    String digits,int index,StringBuffer combination){
        if(index==digits.length()){
            combinations.add(combination.toString());//检查是否全部选出，如果选出，就加入结果集，进行递归
        }else{
            char digit = digits.charAt(index);//创建输入数字中单个数字
            String letters = phoneMap.get(digit);//从哈希表中获取对应数字的对应字母
            int lettersCount = letters.length();
            for(int i=0;i<lettersCount;i++){
                combination.append(letters.charAt(i));//加入单个字符串
                backtrack(combinations,phoneMap,digits,index+1,combination);//递归
                combination.deleteCharAt(index);//删除这个点
            }
        }
    }
}
```
