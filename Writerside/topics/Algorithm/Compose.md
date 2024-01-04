# 组合


## JAVA用1、2、2、3、4、5排列组合，最多能排列多少组合并打印出来。要求：4不能放在第三位，4和5不能相连

```
/**
 * 1,2,2,3,4,5  4不能在第三位,3和5不能相链
 * @author xiwenlu
 */
public class Demo1 {
    public static void main(String[] args) {
        char[] s = {'1','2','2','3','4','5'};
          permutation(s, 0, 5);
    }
    private static void printInfo(char[] a) {
        String s = "";
        for (int i = 0; i < a.length; i++) {
            s+=a[i]+"";
        }
        String regex = "[1-5]{2}[1235][1-5]{3}";
        boolean result = s.matches(regex);
        if (result) {
            if (!(s.contains("35")||s.contains("53"))) {
                System.out.println(s);
            }
        }
    }
    public static void permutation(char[] s,int from,int to) {
        if(to <= 1)
            return;
        if(from == to) {
            printInfo(s);
        } else {
            for(int i=from; i<=to; i++) {
                swap(s,i,from); //交换前缀，使其产生下一个前缀
                permutation(s, from+1, to);
                swap(s,from,i); //将前缀换回，继续做上一个前缀的排列
            }
        }
    }
    public static void swap(char[] s,int i,int j) {
        char tmp = s[i];
            s[i] = s[j];
            s[j] = tmp;
    }
}
```
