# 递归与迭代循环

# 编程题：有n步台阶，一次只能走1步或2步，共有多少种走法？

## 递归
从递归的角度分析，非常简单<br>

|n|步骤|f(n)|
|:---|:---|:---|
|n=1|      一步   |                f(1)=1|
|n=2|   一步一步或直接两步 |        f(2)=2|
|n=3|   (1)先到达f(1),然后跨两步<br>(2)先到达f(2),然后跨一步 |  f(3)=f(1)+f(2)
|n=4|   (1)先到达f(2),然后跨两步<br> (2)先到达f(3),然后跨一步 |  f(4)=f(2)+f(3)   
|...|
|n=n|   (1)先到达f(n-2),然后跨两步<br> (2)先到达f(n-1),然后跨一步 | f(n)=f(n-2)+f(n-1)

```
public int f(int n){
    if(n < 1)
			throw new IllegalArgumentException("n不能小于1");
		if(n == 1 || n == 2)
			return n;
		else
			return f(n-1) + f(n-2);
}
```

## 循环
递归的运算思想其实是逆着来的，要求f(n)就求f(n-1)和f(n-2),然后不断缩减下去。直到触底反弹<br>
其实我们同样利用上面的分析表格，完全能够做出顺着来的循环运算版本<br>
* 要上到第三步台阶，要知道第一步和第二步的走法数
* 要上到第四步台阶，要知道第三步和第二步的走法数
* 要上到第五步台阶，要知道第四步和第三步的走法数
* 只需要不断走下去，直到我们想要知道的第n步台阶就行了

```
import org.junit.jupiter.api.Test;

public class TestStep2 {
	@Test
	public void test() {
		System.out.println(loop(4));
	}
	
	public int loop(int n) {
		if(n<1) {
			throw new IllegalArgumentException("n不能小于1");
		}
		if(n==1 || n==2) {
			return n;
		}
		
		int one = 1;//初始化为走到第一级台阶的走法数
		int two = 2;//初始化为走到第二级台阶的走法数
		int sum = 0;
		
		for(int i=3; i<=n; i++) {
			sum = one + two;
			one = two;
			two = sum;
		}
		return sum;
	}
}

```
