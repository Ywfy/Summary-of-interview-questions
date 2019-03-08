# 八大排序算法
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/SortMethod/%E5%A4%8D%E6%9D%82%E5%BA%A6.png)<br>

## 1、插入排序
将第一个元素视为已经排好序的，然后后面一个一个比较插入到前面的序列中<br>
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/SortMethod/%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F.gif)<br>
```
public class insert_sort {
    public static void sort(int[] array){
        if(array == null)
            throw new IllegalArgumentException("Array is Empty!");

        int temp;
        for(int i = 1 ; i < array.length ; i++){
            temp = array[i];//待插入元素
            int j;
            for(j = i - 1 ; j >= 0 ; j--){
                //升序排序,判断是否大于temp，是则后移一位
                if(array[j] > temp){
                    array[j + 1] = array[j];
                }else{
                    break;
                }
            }
            array[j + 1] = temp;
        }
    }
}
```

## 2、希尔排序
希尔排序(Shell's Sort)是插入排序的一种又称“缩小增量排序”（Diminishing Increment Sort），是直接插入排序算法的一种更高效的改进版本。<br>
希尔排序是非稳定排序算法。该方法因D.L.Shell于1959年提出而得名。<br>
希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，<br>
整个文件恰被分成一组，算法便终止。<br>
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/SortMethod/%E5%B8%8C%E5%B0%94%E6%8E%92%E5%BA%8F.jpg)<br>
```
public class Shell_Sort {
    public static void sort(int[] array){

        for(int dk = array.length / 2 ; dk >= 1 ; dk /= 2){
            int temp;
            for(int x = 0 ; x < dk ; x++)
            for(int i = dk ; i < array.length ; i += dk){
                temp = array[i];//待插入元素
                int j = i - dk;
                for(j = i - dk ; j >= 0 ; j -= dk ) {
                    //升序排序,判断是否大于temp，是则后移一位
                    if (array[j] > temp) {
                        array[j + dk] = array[j];
                    } else {
                        break;
                    }
                }
                array[j + dk] = temp;
            }
        }

    }
}
```

## 3、选择排序
选择排序（Selection sort）是一种简单直观的排序算法。它的工作原理是每一次从待排序的数据元素中选出最小（或最大）的一个元素，<br>
存放在序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。<br>
以此类推，直到全部待排序的数据元素排完。 选择排序是不稳定的排序方法。<br>
```
public class Selection_Sort {
    public static void sort(int[] array){
        for(int i = 0 ; i < array.length ; i++){
            int minIndex = i;//保存最小元素的下标
            for(int j = i ; j < array.length ; j++){
                if(array[minIndex] > array[j]){
                    minIndex = j;
                }
            }
            int temp = array[i];
            array[i] = array[minIndex];
            array[minIndex] = temp;
        }
    }
}
```

## 4、冒泡排序
冒泡排序算法的原理如下：<br>
比较相邻的元素。如果第一个比第二个大，就交换他们两个。<br>
对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。<br>
针对所有的元素重复以上的步骤，除了最后一个。<br>
持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。<br>
```
public class Bubble_Sort {
    public static void sort(int[] array){
        for(int i = 0 ; i < array.length - 1; i++){
            for(int j = 0 ; j < array.length - i - 1 ; j++){
                //升序
                if(array[j] > array[j + 1]){
                    int temp = array[j];
                    array[j] = array[j + 1];
                    array[j + 1] = temp;
                }
            }
        }
    }
}
```

## 5、归并排序
归并操作的工作原理如下：<br>
第一步：申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列<br>
第二步：设定两个指针，最初位置分别为两个已经排序序列的起始位置<br>
第三步：比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置<br>
重复步骤3直到某一指针超出序列尾<br>
将另一序列剩下的所有元素直接复制到合并序列尾<br>
https://www.cnblogs.com/skywang12345/p/3602369.html<br>
```
public class MERGE_SORT {

    /*
    * 将一个数组中的两个相邻有序区间合并成一个
    *
    * 参数说明：
    *     a -- 包含两个有序区间的数组
    *     start -- 第1个有序区间的起始地址。
    *     mid   -- 第1个有序区间的结束地址。也是第2个有序区间的起始地址。
    *     end   -- 第2个有序区间的结束地址。
    */
    private static void merge(int[] arr, int start, int mid, int end){
        int[] tmp = new int[end - start + 1];
        int i = start;
        int j = mid + 1;
        int k = 0;

        while(i <= mid && j <= end){
            if(arr[i] <= arr[j]){
                tmp[k++] = arr[i];
                i++;
            }else{
                tmp[k++] = arr[j];
                j++;
            }
        }

        while(i <= mid){
            tmp[k++] = arr[i];
            i++;
        }

        while(j <= end){
            tmp[k++] = arr[j];
            j++;
        }

        for(int a = 0 ; a < k ; a++){
            arr[a + start] = tmp[a];
        }
        tmp = null;
    }
    private static void merge_sort(int[] arr, int start, int end){
        if(arr == null || start >= end)
            return;

        int mid = (end + start) / 2;
        merge_sort(arr, start, mid);
        merge_sort(arr, mid + 1, end);

        merge(arr, start, mid, end);
    }
    public static void sort(int[] arr){
        merge_sort(arr, 0, arr.length - 1);
    }
}
```

## 6、快速排序
[快速排序(过程图解)](https://blog.csdn.net/adusts/article/details/80882649)<br>
[快速排序法为什么一定要先从右边开始的原因](https://blog.csdn.net/lkp1603645756/article/details/85008715)<br>
```
public class Quick_Sort {

    private static void quickSort(int[] arr, int left, int right){

        if(left > right)
            return;

        int i = left;
        int j = right;
        int pivot = arr[left];

        while(i != j){
            while(arr[j] >= pivot && i < j) {
                j--;
            }
            while(arr[i] <= pivot && i < j)
                i++;
            if(i < j){
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        //基准值归位
        arr[left] = arr[i];
        arr[i] = pivot;

        quickSort(arr, left, i - 1);
        quickSort(arr, i + 1, right);
    }

    public static void sort(int[] arr){
        quickSort(arr, 0, arr.length - 1);
    }
}
```

## 7、堆排序
堆排序其实就是利用大顶堆(小顶堆)的根节点就是最大值(最小值),不断地进行取出元素，最后的结果就是顺序的

## 8、桶排序
![无法加载图片](https://github.com/Ywfy/Summary-of-interview-questions/blob/master/SortMethod/t.jpg)<br>
详情可以查看[视频](https://www.bilibili.com/video/av17940595?from=search&seid=13722490270188887524)<br>

以下是基于桶排序思想实现的基数排序
```
public class BucketSort {
    public static void basket(int data[])//data为待排序数组
    {
        int n=data.length;
        int bask[][]=new int[10][n];
        int index[]=new int[10];
        int max=Integer.MIN_VALUE;
        //为了确定数的最大长度
        for(int i=0;i<n;i++)
        {
            max=max>(Integer.toString(data[i]).length())?max:(Integer.toString(data[i]).length());
        }
        String str;
        //这里是从低位到高位不断比较的过程
        //先排序个位，再排序十位，再排序百位...
        for(int i=max-1;i>=0;i--)
        {
            for(int j=0;j<n;j++)
            {
                //以下过程将小于Max位数的整型前面补0，使得所有数的位数相同
                str="";
                if(Integer.toString(data[j]).length()<max)
                {
                    for(int k=0;k<max-Integer.toString(data[j]).length();k++)
                        str+="0";
                }
                str+=Integer.toString(data[j]);
                bask[str.charAt(i)-'0'][index[str.charAt(i)-'0']++]=data[j];
            }
            //此处j代表桶，一共有10个桶
            //index[j]代表第j个桶有多少个元素在里面
            int pos=0;
            for(int j=0;j<10;j++)
            {
                for(int k=0;k<index[j];k++)
                {
                    data[pos++]=bask[j][k];
                }
            }
            for(int x=0;x<10;x++)index[x]=0;
        }
    }

    public static void main(String[] args){
        int[] a = {5,800,4,60,8,1,0,20,3,6};
        basket(a);
        for(int i = 0 ; i < a.length ; i++)
            System.out.print(a[i] + " ");
    }
}
```

## 桶排序、计数排序和基数排序
```
桶排序：
桶排序 (Bucket sort)或所谓的箱排序，是一个排序算法，工作的原理是将数组分到有限数量的桶子里。
每个桶子再个别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。

计数排序：
算法导论的描述是：计数排序的基本思想是对于给定的输入序列中的每一个元素x，确定该序列中值小于x的元素的个数
（此处并非比较各元素的大小，而是通过对元素值的计数和计数值的累加来确定）。一旦有了这个信息，
就可以将x直接存放到最终的输出序列的正确位置上。例如，如果输入序列中只有17个元素的值小于x的值，
则x可以直接存放在输出序列的第18个位置上。当然，如果有多个元素具有相同的值时，
我们不能将这些元素放在输出序列的同一个位置上，因此，上述方案还要作适当的修改。

不过我的理解是这样的，
将数扔到值对应的桶子里，最后将桶顺序倒出，此时就排好序了

基数排序：
基数排序很简单，利用桶排序的思想，先排序个位数，再排序十位数，再排序百位数...
推荐看以下博客：
https://www.cnblogs.com/skywang12345/p/3603669.html
```
