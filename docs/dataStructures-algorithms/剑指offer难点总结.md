
<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [重构二叉树](#重构二叉树httpswwwnowcodercompractice8a19cbe657394eeaac2f6ea9b0f6fcf6tpid13tqid11157rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [旋转数组的最小数字](#旋转数组的最小数字httpswwwnowcodercompractice9f3231a991af4f55b95579b44b7a01batpid13rp1ru2fta2fcoding-interviewsqru2fta2fcoding-interviews2fquestion-ranking)
- [跳N级台阶](#跳n级台阶httpswwwnowcodercompractice22243d016f6b47f2a6928b4313c85387tpid13rp1ru2fta2fcoding-interviewsqru2fta2fcoding-interviews2fquestion-ranking)
- [栈的压入、弹出序列](#栈的压入-弹出序列httpswwwnowcodercompracticed77d11405cc7470d82554cb392585106tpid13rp1ru2fta2fcoding-interviewsqru2fta2fcoding-interviews2fquestion-ranking)
- [调整数组顺序使奇数位于偶数前面](#调整数组顺序使奇数位于偶数前面httpswwwnowcodercompracticebeb5aa231adc45b2a5dcc5b62c93f593tpid13rp1ru2fta2fcoding-interviewsqru2fta2fcoding-interviews2fquestion-ranking)
- [树的子结构](#树的子结构httpswwwnowcodercompractice6e196c44c7004d15b1610b9afca8bd88tpid13rp1ru2fta2fcoding-interviewsqru2fta2fcoding-interviews2fquestion-ranking)
- [二叉搜索树的后序遍历序列](#二叉搜索树的后序遍历序列httpswwwnowcodercompracticea861533d45854474ac791d90e447bafdtpid13rp1ru2fta2fcoding-interviewsqru2fta2fcoding-interviews2fquestion-ranking)
- [二叉树中和为某一值的路径](#二叉树中和为某一值的路径httpswwwnowcodercompracticeb736e784e3e34731af99065031301bcatpid13rp1ru2fta2fcoding-interviewsqru2fta2fcoding-interviews2fquestion-ranking)
- [二叉搜索树与双向链表](#二叉搜索树与双向链表httpswwwnowcodercompractice947f6eb80d944a84850b0538bf0ec3a5tpid13tqid11179rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [最小的k个数（partation）](#最小的k个数partationhttpswwwnowcodercompractice6a296eb82cf844ca8539b57c23e6e9bftpid13tqid11182rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [连续子数组的最大和（sum < 0置为0）](#连续子数组的最大和sum-0置为0httpswwwnowcodercompractice459bd355da1549fa8a49e350bf3df484tpid13tqid11183rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [整数中1出现的次数（从1到n整数中1出现的次数）](#整数中1出现的次数从1到n整数中1出现的次数httpswwwnowcodercompracticebd7f978302044eee894445e244c7eee6tpid13tqid11184rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [数组中的逆序对](#数组中的逆序对httpswwwnowcodercompractice96bd6684e04a44eb80e6a68efc0ec6c5tpid13tqid11188rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [两个链表的第一个公共结点](#两个链表的第一个公共结点httpswwwnowcodercompractice6ab1d9a29e88450685099d45c9e31e46tpid13tqid11189rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [数字在排序数组中出现的次数](#数字在排序数组中出现的次数httpswwwnowcodercompractice70610bf967994b22bb1c26f9ae901fa2tpid13tqid11190rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [和为S的连续正数序列](#和为s的连续正数序列httpswwwnowcodercompracticec451a3fd84b64cb19485dad758a55ebetpid13tqid11194rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [孩子们的游戏（圆圈中剩下的数）](#孩子们的游戏圆圈中剩下的数httpswwwnowcodercompracticef78a359491e64a50bce2d89cff857eb6tpid13tqid11199rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [不用加减乘除做加法](#不用加减乘除做加法httpswwwnowcodercompractice59ac416b4b944300b617d4f7f111b215tpid13tqid11201rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [数组中重复的数字](#数组中重复的数字httpswwwnowcodercompractice623a5ac0ea5b4e5f95552655361ae0a8tpid13tqid11203rp1rutacoding-interviewsqrutacoding-interviewsquestion-ranking)
- [正则表达式匹配](#正则表达式匹配)
- [表示数值的字符串](#表示数值的字符串)
- [删除链表中重复的结点](#删除链表中重复的结点)
- [二叉树的下一个结点](#二叉树的下一个结点)
- [对称的二叉树（序列化）](#对称的二叉树序列化)

<!-- /code_chunk_output -->


### [重构二叉树](https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=13&&tqId=11157&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
import java.util.*;
public class Solution {
    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
       if(pre.length == 0||in.length == 0){
            return null;
        }

        // 前序遍历的第一个节点是根结点
        TreeNode node = new TreeNode(pre[0]);
        for(int i = 0; i < in.length; i++){
            // 如果找到中序遍历和根结点一样，
            // 那么中序遍历的左边是左子树，右边是右子树
            if(pre[0] == in[i]){
                node.left = reConstructBinaryTree(Arrays.copyOfRange(pre, 1, i+1),
                 Arrays.copyOfRange(in, 0, i));
                node.right = reConstructBinaryTree(Arrays.copyOfRange(pre,
                         i+1, pre.length), Arrays.copyOfRange(in, i+1,in.length));
            }
        }
        return node;
    }
}
```

### [旋转数组的最小数字](https://www.nowcoder.com/practice/9f3231a991af4f55b95579b44b7a01ba?tpId=13&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```java
import java.util.ArrayList;

public class Solution {
    public int minNumberInRotateArray(int [] array) {
        int low = 0 ; int high = array.length - 1;   
        while(low < high){
            int mid = low + (high - low) / 2; 
            // 出现这种情况的array类似[3,4,5,6,0,1,2]，
            // 此时最小数字一定在mid的右边。       
            if(array[mid] > array[high]){
                low = mid + 1;
            // 出现这种情况的array类似 [1,0,1,1,1] 或者[1,1,1,0,1]，
            // 此时最小数字不好判断在mid左边还是右边,这时只好一个一个试 
            }else if(array[mid] == array[high]){
                high = high - 1;
            // 出现这种情况的array类似[2,2,3,4,5,6,6],
            // 此时最小数字一定就是array[mid]或者在mid的左边
            }else{
                high = mid;
            }   
        }
        return array[low];
    }
}
```

### [跳N级台阶](https://www.nowcoder.com/practice/22243d016f6b47f2a6928b4313c85387?tpId=13&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```java
// 每个台阶都有跳与不跳两种情况（除了最后一个台阶），
// 最后一个台阶必须跳。所以共用2^(n-1)中情况
public int JumpFloorII(int target) {
    return 1 << (target - 1);
}

public int JumpFloorII(int target) {
    if(target <= 0){
        return 0;
    }

    int arr[] = new int[target + 1];
    arr[0] = 1;
    int preSum = arr[0];
    for(int i = 1 ; i < arr.length ; i++){
        arr[i] = preSum;
        preSum += arr[i];
    }

    return arr[target];
}
```


### [栈的压入、弹出序列](https://www.nowcoder.com/practice/d77d11405cc7470d82554cb392585106?tpId=13&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```java
public boolean IsPopOrder(int [] arr1,int [] arr2) {
    //input check
    if(arr1 == null || arr2 == null 
        || arr1.length != arr2.length 
        || arr1.length == 0){
        return false;
    }
    Stack<Integer> stack = new Stack();
    int length = arr1.length;
    int i = 0, j = 0;
    while(i < length && j < length){
        if(arr1[i] != arr2[j]){
            // 不相等，压入栈中
            stack.push(arr1[i++]);
        }else{
            // 相等，同时弹出
            i++;
            j++;
        }
    }

    // 栈中为空时，才满足条件
    while(j < length){
        if(arr2[j] != stack.peek()){
            return false;
        }else{
            stack.pop();
            j++;
        }
    }

    return stack.empty() && j == length;
}

```

### [调整数组顺序使奇数位于偶数前面](https://www.nowcoder.com/practice/beb5aa231adc45b2a5dcc5b62c93f593?tpId=13&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```java
public void reOrderArray(int [] array) {
    //相对位置不变，稳定性
    //插入排序的思想
    int m = array.length;
    int k = 0;//记录已经摆好位置的奇数的个数
    for (int i = 0; i < m; i++) {
        if (array[i] % 2 == 1) {
            int j = i;
            while (j > k) {//j >= k+1
                int tmp = array[j];
                array[j] = array[j-1];
                array[j-1] = tmp;
                j--;
            }
            k++;
        }
    }
}

// 空间换时间
public class Solution {
    public void reOrderArray(int [] array) {
        if (array != null) {
            int[] even = new int[array.length];
            int indexOdd = 0;
            int indexEven = 0;
            for (int num : array) {
                if ((num & 1) == 1) {
                    array[indexOdd++] = num;
                } else {
                    even[indexEven++] = num;
                }
            }

            for (int i = 0; i < indexEven; i++) {
                array[indexOdd + i] = even[i];
            }
        }
    }
}

```

### [树的子结构](https://www.nowcoder.com/practice/6e196c44c7004d15b1610b9afca8bd88?tpId=13&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```java

// 解法一
public boolean HasSubtree(TreeNode root1,TreeNode root2) {
    if(root1==null || root2==null)  return false;
    return doesTree1HasTree2(root1, root2)|| HasSubtree(root1.left, root2)
            ||HasSubtree(root1.right, root2);
}

private boolean doesTree1HasTree2(TreeNode root1,TreeNode root2) {
    if(root2==null)  return true;
    if(root1==null)  return false;
    return root1.val==root2.val && doesTree1HasTree2(root1.left, root2.left)
            && doesTree1HasTree2(root1.right, root2.right);
}

// 解法二：先序遍历判断是否是子串
public boolean HasSubtree(TreeNode root1,TreeNode root2) {
    if(root2 == null){
        return false;
    }
    String str1 = pre(root1);
    String str2 = pre(root2);
    return str1.indexOf(str2) != -1;
}
    
public String pre(TreeNode root){
    if(root == null){
        return "";
    }
    String str = root.val + "";
    str += pre(root.left);
    str += pre(root.right);
    return str;
}
```

### [二叉搜索树的后序遍历序列](https://www.nowcoder.com/practice/a861533d45854474ac791d90e447bafd?tpId=13&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

BST的后序序列的合法序列是，对于一个序列S，最后一个元素是x （也就是根），如果去掉最后一个元素的序列为T，那么T满足：T可以分成两段，前一段（左子树）小于x，后一段（右子树）大于x，且这两段（子树）都是合法的后序序列。
```java
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) {
        if(sequence.length==0)
            return false;
        if(sequence.length==1)
            return true;
        return ju(sequence, 0, sequence.length-1);
        
    }
    
    public boolean ju(int[] a,int star,int root){
    	if(star>=root)
    		return true;
    	int i = root;
        //从后面开始找
    	while(i>star&&a[i-1]>a[root])
    		i--;//找到比根小的坐标
        //从前面开始找 star到i-1应该比根小
    	for(int j = star;j<i-1;j++)
    		if(a[j]>a[root])
    			return false;;
        return ju(a,star,i-1)&&ju(a, i, root-1);
    }
}
```

### [二叉树中和为某一值的路径](https://www.nowcoder.com/practice/b736e784e3e34731af99065031301bca?tpId=13&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)

```java
public class Solution {
    private ArrayList<ArrayList<Integer>> listAll = 
                        new ArrayList<ArrayList<Integer>>();
    private ArrayList<Integer> list = new ArrayList<Integer>();
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        if(root == null) return listAll;
        list.add(root.val);
        target -= root.val;
        if(target == 0 && root.left == null 
                    && root.right == null) 
            listAll.add(new ArrayList<Integer>(list));
        FindPath(root.left, target);
        FindPath(root.right, target);
        list.remove(list.size()-1);
        return listAll;
    }
}
```

### [二叉搜索树与双向链表](https://www.nowcoder.com/practice/947f6eb80d944a84850b0538bf0ec3a5?tpId=13&&tqId=11179&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
// 直接使用中序遍历
public class Solution {
    public TreeNode head = null;
    public TreeNode dummy = null;
    public TreeNode Convert(TreeNode pRootOfTree) {
        ConvertSub(pRootOfTree);
        return dummy;
    }
    
    public void ConvertSub(TreeNode root){
        if(root == null) return;
        ConvertSub(root.left);
        
        if(head == null){
            head = root;
            dummy = root;
        } else {
            head.right = root;
            root.left = head;
            head = root;
        }
 
        ConvertSub(root.right);
    }
}
```

### [最小的k个数（partation）](https://www.nowcoder.com/practice/6a296eb82cf844ca8539b57c23e6e9bf?tpId=13&&tqId=11182&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
// 方法一：最大堆
public class Solution {
   public ArrayList<Integer> GetLeastNumbers_Solution(int[] input, int k) {
       ArrayList<Integer> result = new ArrayList<Integer>();
       int length = input.length;
       if(k > length || k == 0){
           return result;
       }
        PriorityQueue<Integer> maxHeap = 
            new PriorityQueue<Integer>(k, new Comparator<Integer>() {

            @Override
            public int compare(Integer o1, Integer o2) {
                return o2.compareTo(o1);
            }
        });
        for (int i = 0; i < length; i++) {
            if (maxHeap.size() != k) {
                maxHeap.offer(input[i]);
            } else if (maxHeap.peek() > input[i]) {
                Integer temp = maxHeap.poll();
                temp = null;
                maxHeap.offer(input[i]);
            }
        }
        for (Integer integer : maxHeap) {
            result.add(integer);
        }
        return result;
    }
}

// 全排序
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> list = new ArrayList<>();
        if(k > input.length){
            return list;
        }
        Arrays.sort(input);
        for(int i = 0; i < k; i++){
            list.add(input[i]);
        }
        return list;
    }
}
```

### [连续子数组的最大和（sum < 0置为0）](https://www.nowcoder.com/practice/459bd355da1549fa8a49e350bf3df484?tpId=13&&tqId=11183&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java

// dp
public int FindGreatestSumOfSubArray(int[] array) {
    if(array.length == 0){
        return 0;
    }
    
    int max = Integer.MIN_VALUE;
    int[] dp = new int[array.length];
    for(int i = 0; i < array.length; i++){
        dp[i] = array[i];
    }
    for(int i = 1; i < array.length; i++){
        dp[i] = Math.max(dp[i-1] + array[i],dp[i]);
    }
    
    for(int i = 0; i < dp.length; i++){
        if(dp[i] > max){
            max = dp[i];
        }
    }
    return max;
}

// sum < 0置为0
public int FindGreatestSumOfSubArray(int[] array) {
    if(array.length == 0){
        return 0;
    }
    
    int max = Integer.MIN_VALUE;
    int cur = 0; 
    for(int i = 0; i < array.length; i++){
        cur += array[i];
        max = Math.max(max,cur);
        cur = cur < 0 ? 0 : cur;
    }
    return max;
}

```

### [整数中1出现的次数（从1到n整数中1出现的次数）](https://www.nowcoder.com/practice/bd7f978302044eee894445e244c7eee6?tpId=13&&tqId=11184&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
public int NumberOf1Between1AndN_Solution(int n) {
    int count=0;
    StringBuffer s=new StringBuffer();
    for(int i=1;i<n+1;i++){
        s.append(i);
    }

    for(int i=0;i<s.length();i++){
        if(s.charAt(i)=='1')
            count++;
    }
    return count;
}
```

### [数组中的逆序对](https://www.nowcoder.com/practice/96bd6684e04a44eb80e6a68efc0ec6c5?tpId=13&&tqId=11188&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
public class Solution {
    //统计逆序对的个数
    int cnt;
    public int InversePairs(int [] array) {
        if(array.length != 0){
            divide(array,0,array.length-1);
        }
        return cnt;
    }

    //归并排序的分治---分
    private void divide(int[] arr,int start,int end){
        //递归的终止条件
        if(start >= end)
            return;
        //计算中间值，注意溢出
        int mid = start + (end - start)/2;

        //递归分
        divide(arr,start,mid);
        divide(arr,mid+1,end);

        //治
        merge(arr,start,mid,end);
    }

    private void merge(int[] arr,int start,int mid,int end){
        int[] temp = new int[end-start+1];

        //存一下变量
        int i=start,j=mid+1,k=0;
        //下面就开始两两进行比较，若前面的数大于后面的数，就构成逆序对
        while(i<=mid && j<=end){
            //若前面小于后面，直接存进去，并且移动前面数所在的数组的指针即可
            if(arr[i] <= arr[j]){
                temp[k++] = arr[i++];
            }else{
                temp[k++] = arr[j++];
                //a[i]>a[j]了，那么这一次，从a[i]开始到a[mid]必定都是大于这个a[j]的，因为此时分治的两边已经是各自有序了
                cnt = (cnt+mid-i+1)%1000000007;
            }
        }
        //各自还有剩余的没比完，直接赋值即可
        while(i<=mid)
            temp[k++] = arr[i++];
        while(j<=end)
            temp[k++] = arr[j++];
        //覆盖原数组
        for (k = 0; k < temp.length; k++)
            arr[start + k] = temp[k];
    }
}
```

### [两个链表的第一个公共结点](https://www.nowcoder.com/practice/6ab1d9a29e88450685099d45c9e31e46?tpId=13&&tqId=11189&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
    if(pHead1 == null || pHead2 == null){
        return null;
    }
        
    ListNode p1 = pHead1;
    ListNode p2 = pHead2;
        
    while(p1 != p2){
        p1 = p1.next;
        p2 = p2.next;
        if(p1 != p2){
            if(p1 == null) p1 = pHead2;
            if(p2 == null) p2 = pHead1;
        }
    }
        
    return p1;
        
}
```

### [数字在排序数组中出现的次数](https://www.nowcoder.com/practice/70610bf967994b22bb1c26f9ae901fa2?tpId=13&&tqId=11190&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
public class Solution {
    public static int GetNumberOfK(int [] array , int k) {
        int length = array.length;
        if(length == 0){
            return 0;
        }
        int lastK = getLastK(array,k);
        int firstK = getFirstK(array,k);
        if(firstK != -1 && lastK != -1){
            return lastK - firstK + 1;
        }
        return 0;
    }

    public static int getLastK(int [] array, int k){
        int start = 0, end = array.length - 1;
        int mid = start + (end - start) / 2;
        while(start <= end){
            if(array[mid] > k){
                end = mid - 1;
            } else if(array[mid] < k){
                start = mid + 1;
            } else if(mid + 1 < array.length && array[mid+1] == k){
                start = mid + 1;
            }else {
                return mid;
            }
            mid = start + (end - start) / 2;
        }
        return -1;
    }

    public static int getFirstK(int [] array, int k){
        int start = 0, end = array.length - 1;
        int mid = start + (end - start) / 2;
        while(start <= end){
            if(array[mid] > k){
                end = mid - 1;
            } else if(array[mid] < k){
                start = mid + 1;
            } else if(mid - 1 >= 0 && array[mid-1] == k){
                end = mid - 1;
            }else {
                return mid;
            }
            mid = start + (end - start) / 2;
        }

        return -1;
    }
}
```

### [和为S的连续正数序列](https://www.nowcoder.com/practice/c451a3fd84b64cb19485dad758a55ebe?tpId=13&&tqId=11194&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
        ArrayList<ArrayList<Integer>> list = new ArrayList<>();
        int left = 1;
        int right = 2;
        int res = 0;
        while(left < right){
            ArrayList<Integer> temp = new ArrayList<>();
            res = 0;
            int index = left;
            while(index <= right){
                //收集临时值
                temp.add(index);
                res+=index;
                index++;
            }
            
            if(res < sum){
                right++;
            } else if(res > sum){
                left++;
            } else {
                //收集结果
                list.add(temp);
                left++;//如果找到一个结果，left右移，继续找其他结果
            }
           
        }
        return list;
    }
}
```

### [孩子们的游戏（圆圈中剩下的数）](https://www.nowcoder.com/practice/f78a359491e64a50bce2d89cff857eb6?tpId=13&&tqId=11199&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
import java.util.*;
public class Solution {
    public int LastRemaining_Solution(int n, int m) {
        LinkedList<Integer> list = new LinkedList<>();
        for(int i = 0; i < n; i++){
            list.add(i);
        }
        int bt = 0;
        while(list.size() > 1){
            bt = (bt + m - 1) % list.size();
            list.remove(bt);
        }
        
        return list.size() == 1 ? list.get(0) : -1;
    }
}
```

### [不用加减乘除做加法](https://www.nowcoder.com/practice/59ac416b4b944300b617d4f7f111b215?tpId=13&&tqId=11201&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
public class Solution {
    public int Add(int num1,int num2) {
        while(num2 != 0){
            int sum = num1 ^ num2;//不进位求值
            int carry = (num1 & num2) << 1;//求进位值
            num1 = sum;
            num2 = carry;
        }
        return num1;
    }
}
```

### [数组中重复的数字](https://www.nowcoder.com/practice/623a5ac0ea5b4e5f95552655361ae0a8?tpId=13&&tqId=11203&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

```java
//boolean只占一位，所以还是比较省的
public boolean duplicate(int numbers[], int length, int[] duplication) {
    boolean[] k = new boolean[length];
    for (int i = 0; i < k.length; i++) {
        if (k[numbers[i]] == true) {
            duplication[0] = numbers[i];
            return true;
        }
        k[numbers[i]] = true;
    }
    return false;
}
```

### 正则表达式匹配

```java

```

### 表示数值的字符串

```java
// 统一回复 .2 在 Java、Python 中都是数字
public class Solution {
    public boolean isNumeric(char[] str) {
        String string = String.valueOf(str);
        return string.matches("[\\+-]?[0-9]*(\\.[0-9]*)?([eE][\\+-]?[0-9]+)?");
    }
}
```

### 删除链表中重复的结点

```java
public ListNode deleteDuplication(ListNode pHead){
    if(pHead == null){
        return null;
    }
    ListNode node = new ListNode(Integer.MIN_VALUE);
    node.next = pHead;
    ListNode pre = node, p = pHead;
    boolean deletedMode = false;
    while(p != null){
        if(p.next != null && p.next.val == p.val){
            p.next = p.next.next;
            deletedMode = true;
        }else if(deletedMode){
            pre.next = p.next;
            p = pre.next;
            deletedMode = false;
        }else{
            pre = p;
            p = p.next;
        }
    }

    return node.next;
}

```

### 二叉树的下一个结点

```java
public TreeLinkNode GetNext(TreeLinkNode pNode){
    if(pNode == null){
        return null;
    }
    //如果有右子树，后继结点是右子树上最左的结点
    if(pNode.right != null){
        TreeLinkNode p = pNode.right;
        while(p.left != null){
            p = p.left;
        }
        return p;
    }else{
        //如果没有右子树，向上查找第一个当前结点是父结点的左孩子的结点
        TreeLinkNode p = pNode.next;
        while(p != null && pNode != p.left){
            pNode = p;
            p = p.next;
        }

        if(p != null && pNode == p.left){
            return p;
        }
        return null;
    }
}

```

### 对称的二叉树（序列化）

```java
boolean isSymmetrical(TreeNode pRoot){
    if(pRoot == null){
        return true;
    }
    StringBuffer str1 = new StringBuffer("");
    StringBuffer str2 = new StringBuffer("");
    preOrder(pRoot, str1);
    preOrder2(pRoot, str2);
    return str1.toString().equals(str2.toString());
}

public void preOrder(TreeNode root, StringBuffer str){
    if(root == null){
        str.append("#");
        return;
    }
    str.append(String.valueOf(root.val));
    preOrder(root.left, str);
    preOrder(root.right, str);
}

public void preOrder2(TreeNode root, StringBuffer str){
    if(root == null){
        str.append("#");
        return;
    }
    str.append(String.valueOf(root.val));
    preOrder2(root.right, str);
    preOrder2(root.left, str);
}

```