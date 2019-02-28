# 多益笔试编程题

## 1、“    aa bb   cc de”去除多余的空格，变成“aa bb cc de”
```
public class Prog1 {
    public static String trims(String str){
        StringBuilder res = new StringBuilder();
        for(String str2 : str.split("\\s+")){
            res.append(str2 + " ");
        }
        res.deleteCharAt(res.length() - 1);
        return res.toString();
    }

    public static void main(String[] args){
        String str = "    aa bb   cc  de ";
        System.out.println(Prog1.trims(str));
    }
}
```

## 2、不用中间变量交换两个整形变量
```
public class Prog2 {
    public static void trans(int a, int b){
        System.out.format("a: %d, b: %d\n", a, b);
        a = a + b;
        b = a - b;
        a = a - b;
        System.out.format("a: %d, b: %d", a, b);
    }

    public static void main(String[] args){
        trans(3, 6);
    }
}
```

## 3、查找数组的主元素(出现次数大于length/2)
```
public class majorityElement {
    public static Integer searchMajorityEle(int[] arr){
        int length = arr.length;
        if(arr == null)
            throw new IllegalArgumentException("Array is Empty!");
        if(arr.length == 1)
            return arr[1];

        int x = arr[0];
        int mark = 1;
        for(int i = 1 ; i < arr.length ; i++){
            if(arr[i] == arr[i - 1])
                mark++;
            else{
                mark--;
                if(mark == 0){
                    x = arr[i];
                    mark = 1;
                }
            }
        }

        int count = 0;
        for(int i = 0 ; i < arr.length ; i++){
            if(arr[i] == x)
                count++;
        }
        if(count > arr.length / 2)
            return x;

        return null;
    }

    public static void main(String[] args){
        int[] arr = {1,5,8,9,2,2,2,2,2,2,2,6};
        int[] arr2 = {1,2,3,4,5,6,6,6,6,6,8,8,9};
        System.out.println("The majorityElement of arr is " + searchMajorityEle(arr));
        System.out.println("The majorityElement of arr2 is " + searchMajorityEle(arr2));
    }
}

```

## 4、查找字符串中的最大数字连续串
```
public class Prog4 {
    public static Integer MaxLengthInt(String str){
        if(str == null)
            throw new IllegalArgumentException("The input str is empty");

        StringBuilder res = new StringBuilder();
        int max = 0;
        boolean HasInt = false;
        for(int i = 0 ; i < str.length() ; i++){
            if(str.charAt(i) >= '0' && str.charAt(i) <= '9'){
                HasInt = true;
                res.append(str.charAt(i));
            }
            else if(!res.toString().isEmpty()){
                int tmp = Integer.parseInt(res.toString());
                if(max < tmp)
                    max = tmp;
                res.setLength(0);
            }
        }
        if(HasInt == true)
            return max;
        return null;
    }

    public static void main(String[] args){
        String str = "lao256897ma666liu589hq123456ty";
        System.out.println(MaxLengthInt(str));
    }
}
```

## 5、用两个栈实现队列
```
import java.util.Stack;

public class Stack2Queue {
    private Stack<Integer> stackPush; //压入栈
    private Stack<Integer> stackPop; //弹出栈

    public Stack2Queue(){
        stackPush = new Stack<>();
        stackPop = new Stack<>();
    }

    public void add(Integer e){
        stackPush.push(e);
    }

    public Integer remove(){
        if(stackPop.isEmpty()){
            while(!stackPush.isEmpty()){
                Integer tmp = stackPush.pop();
                stackPop.push(tmp);
            }
        }
        if(stackPop.isEmpty()){
            return null;
        }else{
            return stackPop.pop();
        }
    }

    public Integer peek(){
        if(stackPop.isEmpty()){
            while(!stackPush.isEmpty()){
                Integer tmp = stackPush.pop();
                stackPop.push(tmp);
            }
        }
        if(stackPop.isEmpty()){
            return null;
        }else{
            return stackPop.peek();
        }
    }

    public static void main(String[] args){
        Stack2Queue queue = new Stack2Queue();
        for(int i = 0 ; i < 10 ; i++){
            queue.add(i);
        }
        for(int i = 0 ; i < 10 ; i++){
            System.out.print(queue.peek() + ", ");
            queue.remove();
        }
    }

}
```

## 6、快乐数
