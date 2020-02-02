## 二维数组中的查找
### 方法一:
通过遍历array数组，去查找array数组中有没有target的值。它的时间复杂度是(O(n * m))
~~~ java 
    public boolean Find(int target, int [][] array) {
        for (int i = 0; i < array.length; i++)
        {
            for (int j = 0; j < array[0].length; j++)
            {
                if (array[i][j] == target)
                    return true;
            }
        }
        return false;
    }
~~~ 

### 方法二(从矩阵的右上角开始找):
设置一个i，j表示所找当前位置，如果说array[i][j] > target大的话，接着就往左找,反之往下找，直到找到array[i][j] == target为止。它的时间复杂度是：O(n + m)
~~~ java
public boolean Find(int target, int [][] array) {
        int i = 0;
        int j = array[0].length - 1;
        while(i >= 0 && i < array.length && j >= 0 && j < array[0].length)
        {
            // array[i][j]
            if (array[i][j] == target)
                return true;
            else if (array[i][j] > target)
                j--;
            else
                i++;
        }
        return false;
    }
~~~
### 方法三(从矩阵的左下角开始找):
设置一个i，j表示所找当前位置，如果说array[i][j] > target大的话，接着就往上找,反之往右找，直到找到array[i][j] == target为止。它的时间复杂度是：O(n + m)
~~~ java
public boolean Find(int target, int [][] array) {
        int i = array.length - 1;
        int j = 0;
        while(i >= 0 && i < array.length && j >= 0 && j < array[0].length)
        {
            // array[i][j]
            if (array[i][j] == target)
                return true;
            else if (array[i][j] > target)
                i--;
            else
                j++;
        }
        return false;
    }
~~~

## 替换空格
### 方法一:去遍历字符串，然后判断当前位置的字符是否为空格，如果为空格的话，就追加"%20",如果不为空格的话，那么就追加当前位置的字符
~~~ java
public String replaceSpace(StringBuffer str) {
        int len = str.length();
        String res = "%20";
        StringBuffer ans = new StringBuffer();
        for (int i = 0; i < len; i++) {
            ans.append(str.charAt(i) == ' ' ? res : str.charAt(i));
        }
        return ans.toString();
    }
~~~

## 从尾到头打印链表
### 方法一：通过Java中的Stack类去模拟栈的过程
~~~ java
public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<>();
        Stack<Integer> stack = new Stack<>();
        while(listNode != null) {
            stack.add(listNode.val); /// 取当前节点的值放入栈中
            listNode = listNode.next; /// 更新当前节点为下一个节点
        }
        while (!stack.isEmpty()) {
            list.add(stack.pop()); /// 取出当前栈顶元素然后放入list中
        }
        return list;
    }
~~~
### 方法二:采用递归的方式去模拟链表反置的作用
~~~ java
private static ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        ArrayList<Integer> list = new ArrayList<>();
        if (listNode == null) {
            return list;
        }
        return solve(list, listNode);
    }
    // 1->2->3->4
    private static ArrayList<Integer> solve(ArrayList<Integer> list, ListNode listNode) {
        if (listNode.next != null) {   /// 当前节点的下一个节点不为空
            list = solve(list, listNode.next); /// 往下递归
        }
        list.add(listNode.val);
//        System.out.println(list);
        return list;
    }
~~~

## 重建二叉树
### 方法一：通过依次遍历前序序列，然后在中序序列中确定当前遍历的前序序列中的数字所在的位置，然后在去划分出当前节点的左右子树，最后在去传入递归程序即可。
~~~ java
import org.omg.PortableInterceptor.SYSTEM_EXCEPTION;

class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

public class Main4 {
    private static int index = 0;
    private static TreeNode solve(int[] pre, int[] tempIn) {
        int len1 = 0; /// 当前节点的左子树的节点的个数
        int len2 = 0; /// 当前节点的右子树的节点的个数
        for (int i = 0; i < tempIn.length; i++) {
            if (pre[index] == tempIn[i]) {
                break;
            }
            len1 ++; /// 左子树节点的个数++
        }
        len2 = tempIn.length - len1 - 1;

        int index1 = 0;
        int index2 = 0;
        int[] temp1 = new int[len1]; /// 当前节点的左子树
        int[] temp2 = new int[len2]; /// 当前节点的右子树
        boolean flag = false;
        for (int i = 0; i < tempIn.length; i++) {
            if (pre[index] == tempIn[i]) {
                flag = true;
            } else if (!flag) {
                temp1[index1++] = tempIn[i];
            } else {
                temp2[index2++] = tempIn[i];
            }
        }
        TreeNode node = new TreeNode(pre[index]);
        node.right = null;
        node.left = null;
//        System.out.printf("%d左子树:", pre[index]);
//        for (int i = 0; i < temp1.length; i++) {
//            System.out.printf("%d ", temp1[i]);
//        }
//        System.out.printf(",");
//        System.out.printf("%d右子树:", pre[index]);
//        for (int i = 0; i < temp2.length; i++) {
//            System.out.printf("%d ", temp2[i]);
//        }
//        System.out.println();
        if (index < pre.length && temp1.length > 0) {
            index++; /// 遍历前序序列的下标加1
            node.left = solve(pre, temp1); /// 创建当前节点的左子树
        }
        if (index < pre.length && temp2.length > 0) {
            index++; /// 遍历前序序列的下标加1
            node.right = solve(pre, temp2); /// 创建当前节点的右子树
        }
        return node;
    }
    private static TreeNode reConstructBinaryTree(int[] pre, int[] in) {
        index = 0; /// 遍历前序序列的下标
        return solve(pre, in);
    }

    public static void main(String[] args) {
        int[] pre = {1, 2, 4, 7, 3, 5, 6, 8}; /// 前序遍历
        int[] in = {4, 7, 2, 1, 5, 3, 8, 6}; /// 中序遍历
        TreeNode root = reConstructBinaryTree(pre, in);

        dfs1(root);
        System.out.println();
        dfs2(root);
        System.out.println();
        dfs3(root);
        System.out.println();

    }
    private static void dfs1(TreeNode node) {
        System.out.printf("%d ", node.val);
        if (node.left != null) {
            dfs1(node.left);
        }
        if (node.right != null) {
            dfs1(node.right);
        }
    }
    private static void dfs3(TreeNode node) {
        if (node.left != null) {
            dfs3(node.left);
        }
        if (node.right != null) {
            dfs3(node.right);
        }
        System.out.printf("%d ", node.val);
    }

    private static void dfs2(TreeNode node) {
        if (node.left != null) {
            dfs2(node.left);
        }
        System.out.printf("%d ", node.val);
        if (node.right != null) {
            dfs2(node.right);
        }
    }

}
~~~ 
## 用两个栈去实现队列
### 方法一：通过两个栈中元素之间的的复制交换去实现了队列的功能。
~~~ java
import java.util.Stack;

public class Main5 {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();

    public void push(int node) {
        while (!stack2.isEmpty()) {
            stack1.push(stack2.pop());  /// 将栈2中的元素放入到栈1中
        }
        stack1.push(node);
    }

    public int pop() {
        while (!stack1.isEmpty()) {
            stack2.push(stack1.pop()); /// 将栈1中的元素放入到栈2中
        }
        return stack2.pop();
    }
}
~~~

## 旋转数组的最小数字
### 方法一：遍历数组，不断去更新保存最小值的变量。时间复杂度是O(n)
~~~ java
    public int minNumberInRotateArray(int [] array) {
        if (array.length == 0) {
            return 0;
        }
        int ans = array[0];
        for (int i = 1; i < array.length; i++) {
            ans = Math.min(ans, array[i]);
        }
        return ans;
    }
~~~ 
### 方法二：通过二分的方法，不断去更新存在于两个子数组(两个非递减排序子数组)中的下标。时间复杂度是O(log(n))
~~~ java
 public int minNumberInRotateArray(int[] array) {
        if (array.length == 0) {
            return 0;
        }
        int l = 0;
        int r = array.length - 1;
        while (l < r - 1) {
            int mid = (l + r) >> 1;
            if (array[mid] >= array[l]) {
                l = mid; /// 说明mid所在的位置是在第一个非递减子数组中
            } else if (array[mid] <= array[r]) {
                r = mid; /// 说明mid所在的位置是在第二个非递减子数组中
            }
        }
        return array[r];
    }
~~~

## 斐波那契额数列
### 方法一：采用递推的方式去求出a[n]的值
~~~ java
    public int Fibonacci(int n) {
        if (n == 0) {
            return 0;
        }
        int[] a = new int[n + 1];
        if (n == 1 || n == 2) {
            return 1;
        }
        a[1] = 1;
        a[2] = 1;
        for (int i = 3; i <= n; i++) {
            a[i] = a[i - 1] + a[i - 2];
        }
        return a[n];
    }
~~~

### 方法二：采用递归的方式去求出a[n]的值
~~~ java
public int Fibonacci(int n) {
        if (n == 0) {
            return 0; /// 终止递归的条件
        }
        if (n == 1 || n == 2) {
            return 1; /// 终止递归的条件
        }
        return Fibonacci(n - 1) + Fibonacci(n -2);
    }
~~~

## 跳台阶
### 方法一：采用递归的方式去模拟出每次跳台阶的时候所作出的选择:要么跳1阶，要么跳2阶。
~~~ java
 public int JumpFloor(int target) {
        if (target == 1) {
            return 1; /// 目前递归到第1阶台阶，就没有必要往下去递归了
        }
        if (target == 2) {
            return 1 + JumpFloor(target - 1); /// 如果target == 2，其实就是等于从起点位置直接跳2阶 + 递归到第一阶的情况的总的跳阶的次数
//            return 2;
        }
        return JumpFloor(target - 1) + JumpFloor(target - 2); /// 当前target台阶的次数等于往前跳1阶加上往前跳2阶
    }
~~~

### 方法二：采用递推的方式去计算出从起点到第i阶的总的情况数与从起点到第i - 1阶的总的情况数和从起点到第i - 2阶的总的情况数之间的关系等式。
~~~ java
public int JumpFloor(int target) {
        if (target == 1) {
            return 1;
        }
        if (target == 2) {
            return 2;
        }
        int[] a = new int[target + 1]; /// a[i] 代表从起点到第i阶的总的情况数
        a[1] = 1; /// 第一阶的总情况数是1
        a[2] = 2; /// 第二阶的总情况数是2
        for (int i = 3; i <= target; i++) {
            a[i] = a[i - 1] + a[i - 2]; /// 对于第i阶的总情况数就等于从起点到第i-1阶的情况数(从0 -> i-1 -> +1 = i)加上从起点到第i-2阶的情况数(0 -> i-2 -> +2 ->i)
        }
        return a[target];
    }
~~~

### 变态跳台阶
### 方法一：采用递推的方式，对于第i阶台阶的跳法的总次数，他是等于从第一阶到第i-1阶的情况数总和然后再加上从起点到终点的这一种情况数。
~~~ java
public int JumpFloorII(int target) {
        if (target == 1) {
            return 1;
        }
        int[] a = new int[target + 1];
        int sum = 1; /// 设置一个sum变量去记录1到n-1阶的总的情况数
        for (int i = 2; i <= target; i++) {
            a[i] = sum + 1; /// 对于第i阶台阶他是等于从第1阶到第i-1阶台阶的情况数之和然后再加上1(从起点到i阶的情况)
            sum = sum + a[i]; /// 需要去更新1到i阶的情况数
        }
        return a[target];
    }
~~~

### 矩阵覆盖
### 方法一：对于2*i的矩形，他的情况数就是等于2*(i-1)的基础上在右边放置一个竖着的2*1的小矩阵，然后再加上2*(i-2)的矩形的基础上在右边横着放置两个2*1的小矩阵。
~~~ java
public int RectCover(int target) {
        if (target == 0) {
            return 0;
        }
        if (target == 1) {
            return 1;
        }
        if (target == 2) {
            return 2;
        }
        int[] a = new int[target + 1];
        a[1] = 1;
        a[2] = 2;
        for (int i = 3; i <= target; i++) {
            a[i] = a[i - 1] + a[i - 2];  /// 对于2*i的矩形，他的情况数就是等于2*(i-1)的基础上在右边放置一个竖着的2*1的小矩阵，然后再加上2*(i-2)的矩形的基础上在右边横着放置两个2*1的小矩阵。
        }
        return a[target];
    }
~~~

## 二进制中1的个数
### 方法一：本质上就是对n的二进制表示中的每一位进行判断。 
eg:
5 -》 101 & 1 —》 10 & 1  -》 1 & 1 -》 0 & 1这种方法是有问题的。
1 -> 0000000...01 -> (-1) -> 11....11111 -> 右移1位，数字-1的二进制的左边是补1的，也就是说，无论你右移多少次，结果都是-1.

 改进：对n&运算的后面的那个数字进行操作：
 5-》 101 & 1 -》 101 & 10 -》 101 & 100
 
~~~ java
    public int NumberOf1(int n) {
        int sum = 0; /// 记录1的个数
        int temp = 1; /// 本质上是用temp变量去判断n的每一位数字是否为1
        while (temp != 0) { /// 当temp为0的时候，说明已经移动了32次，然后就说明已经遍历完了n的每一位
            sum = (n & temp) != 0 ? sum + 1 : sum;
            temp = temp << 1;
        }
        return sum;
    }
~~~
 
 ### 方法二：本质上对n的二进制表示中的1的位置的判断。
 eg:
 5 -》 101 & 100(101 - 1) = 100 -》 100 & 011(100 - 1) = 000 -》 000
~~~ java
public int NumberOf1(int n) {
        int sum = 0; /// 记录1的个数
        while (n != 0) {  /// 说明当前n的二进制表示中肯定有1
            sum++;
            n = n & (n - 1); /// 本质上就是消除从右往左数的第一个位置的1。
        }
        return sum;
    }
~~~

## 数值的整数次方
### 方法一：对exponent进行分类讨论，主要是当exponent小于0的时候，我们需要求出base的-exponent次方的值，然后拿1除以这个结果即可。
~~~ java
public double Power(double base, int exponent) {
        double ans = 1.0;
        if (exponent >= 0) {
            for (int i = 1; i<= exponent; i++) {
                ans = ans * base;
            }
        } else {
            for (int i = 1; i<= -exponent; i++) {   /// 注意一下exponent是一个负数
                ans = ans * base;
            }
            ans = 1 / ans;
        }
        return ans;
    }
~~~

## 调整数组顺序使奇数位于偶数前面
### 方法一：类似于插入排序的思想，遇见奇数就将当前的奇数往前移动，知道往前移动的过程中，遇到奇数时停止移动。时间复杂度是O(n^2)
~~~ java
        public void reOrderArray(int [] array) {
        int len = array.length;

        for (int i = 0; i < len; i++) {
            if (array[i] % 2 != 0) {
                for (int j = i - 1; j >= 0; j--) {
                    if (array[j] % 2 == 0) {
                        int temp = array[j];
                        array[j] = array[j + 1];
                        array[j + 1] = temp;
                    } else {
                        break;
                    }
                }
            }
        }
    }
~~~
### 方法二： 本质上就是开辟两个空间去存储奇数和偶数，最终将这两个空间中的值合并即可。时间复杂度是O(n)，但是空间复杂度比方法一要大。
~~~ java
public void reOrderArray(int[] array) {
        int len = array.length;
        ArrayList<Integer> list1 = new ArrayList<>();  /// 保存奇数
        ArrayList<Integer> list2 = new ArrayList<>();  /// 保存偶数

        for (int i = 0; i < len; i++) {
            if (array[i] % 2 != 0) {
                list1.add(array[i]);
            } else {
                list2.add(array[i]);
            }
        }
        int index = 0;
        for (int x : list1) {
            array[index++] = x;
        }
        for (int x : list2) {
            array[index++] = x;
        }
    }
~~~
## 链表中倒数第k个节点
### 方法一：采用递归的方式去模拟链表从尾到头的这样一个方向，然后在从尾到头的过程中，去判断当前节点的位置，是否为倒数第k个即可。
~~~ java
private ListNode ans; /// 最终返回的结果
    private int sum; /// 用来记录当前节点是倒数第几个节点

    private void dfs(ListNode node, int k) {
        if (node.next != null) {
            dfs(node.next, k); /// 继续递归到下一节点。
        }
        // 下面这部分其实就是判断当前层的节点是倒数第几个节点。
        sum++;
        if (sum == k) {
            ans = node;
        }
    }

    public ListNode FindKthToTail(ListNode head, int k) {
        ans = null;
        sum = 0;
        if (head == null) {  /// 说明链表为null，就没有必要去递归的需要了
            return null;
        }
        dfs(head, k); /// 递归遍历链表
        return ans;
    }
~~~
### 方法二：通过初始化两个移动节点的位置距离为k，然后同时移动两个节点，知道第二个节点移动到链表的末尾时，移动节点1的位置就是链表倒数第k个节点。
~~~ java
        public ListNode FindKthToTail(ListNode head,int k) {
        ListNode removeNode = head;
        while (k != 0) {
            if (removeNode == null) { /// k 大于链表的长度，直接返回null
                return null;
            }
            removeNode = removeNode.next;
            k--;
        }
        while (removeNode != null) {  /// 这个循环其实就是同时移动head和removeNode两个节点。
            removeNode = removeNode.next;
            head = head.next;
        }
        return head;
    }
~~~
## 反转链表
### 方法一：就是通过两个距离为1的移动节点，去不断的去反转原链表相邻的节点之间的指向。
~~~ java
    public ListNode ReverseList(ListNode head) {
        if (head == null) {
            return null;
        }

        ListNode frontNode = head;
        ListNode removeNode = head.next;

        while (removeNode != null) {
            ListNode tempNode = removeNode.next; /// 用来保存移动节点的下一个节点，不然的话，就会造成节点最终无法往右移动的情况。
            removeNode.next = frontNode; /// 实现链表的反置
            // 下面两行代码就是实现两个节点的向右平移操作。
            frontNode = removeNode;
            removeNode = tempNode;
        }
        head.next = null;
        return frontNode;
    }
~~~
### 方法二：通过栈去模拟反置的过程（不推荐）
~~~ java
public ListNode ReverseList(ListNode head) {
        if (head == null) {
            return null;
        }
        Stack<ListNode> stack = new Stack<>();
        while (head != null) {
            stack.push(head);
            head = head.next;
        }

        ListNode removeNode = stack.pop(); /// 创建新的链表，需要创建一个新的引用
        ListNode ans = removeNode;
        removeNode.next = null; /// 初始化
        while (!stack.isEmpty()) {
            ListNode x = stack.pop(); /// 取出栈顶节点元素，然后初始化节点元素的next值
            x.next = null;
            /// 可以用链表的尾接法去理解
            removeNode.next = x;
            removeNode = x;
        }
        return ans;

    }
~~~

## 合并两个排序的链表
### 方法一：类似于归并排序中子序列合并过程，不断去比较两个链表中节点的val值，然后去判断那个节点优先需要添加到合成链表的尾部。
~~~ java
public ListNode Merge(ListNode list1,ListNode list2) {
        if (list1 == null) {
            return list2;
        }
        if (list2 == null) {
            return list1;
        }
        ListNode headNode; /// 最终合成链表得头节点。
        if (list1.val > list2.val) {
            headNode = list2;
            list2 = list2.next;
        } else {
            headNode = list1;
            list1 = list1.next;
        }
        ListNode removeNode = headNode; /// 其实在当前位置就是合成链表得长度为1，头节点和尾节点是一样的。

        while (list1 != null && list2 != null) {
            if (list1.val > list2.val) {
                removeNode.next = list2; /// 将合成链表的尾部节点添加链表2中当前所指向的节点
                removeNode = list2; /// 去更新合成链表的尾部节点
                list2 = list2.next;
            } else {
                removeNode.next = list1; /// 将合成链表的尾部节点添加链表2中当前所指向的节点
                removeNode = list1; /// 去更新合成链表的尾部节点
                list1 = list1.next;
            }
        }

        /// 其实就是将剩余的链表1中的节点放入到合成链表中
        while (list1 != null) {
            removeNode.next = list1;
            removeNode = list1;
            list1 = list1.next;
        }
        /// 其实就是将剩余的链表2中的节点放入到合成链表中
        while (list2 != null) {
            removeNode.next = list2;
            removeNode = list2;
            list2 = list2.next;
        }
        return headNode;
    }
~~~

## 树的子结构
### 方法一：主要是分为两个过程，过程一就是查找过程，只有在查找过程中当前A中节点的val等于B中root节点val值一样时，才会进入到匹配过程，匹配过程的话对两个二叉树就是采用同样的比例方式去比较每次递归的节点的val是否一样。然后去判断两个二叉树结构是否一样。
~~~ java
private boolean judge(TreeNode node1, TreeNode node2) { /// 第二部分，匹配
        if (node2 == null) {
            return true; /// 说明二叉树B的某一个方向的节点已经完全的匹配成功。
        }
        if (node1 == null) {
            return false; /// 说明在某一方向上，二叉树A中的节点不缺少的，相对于二叉树B
        }
        if (node1.val == node2.val) {
            boolean flag1 = true; /// 默认左子树是匹配的，假如说不匹配，它就会返回false
            boolean flag2 = true; /// 默认右子树是匹配的，假如说不匹配，它就会返回false
            if (node1.left != null || node2.left != null) {
                flag1 = judge(node1.left, node2.left); /// 比较子树A和二叉树B的左子树
            }
            if (flag1 && (node1.right != null || node2.right != null)) { /// flag1 -> 剪枝
                flag2 = judge(node1.right, node2.right); /// 比较子树A和二叉树B的右子树
            }
            return flag1 && flag2; /// && -> 不光某一个节点的左子树要完全匹配，右子树也是要完全匹配的
        } else {
            return false;
        }
    }

    /// 二叉树的先序遍历
    private boolean dfs(TreeNode node, TreeNode root2) {  /// 第一部分，查找
        boolean flag = false;
        if (node.val == root2.val) {
            flag = judge(node, root2); /// 进入第二部分的匹配过程
        }
        if (flag) {
            return true; /// 通过当前节点已经找到二叉树B的完全匹配结果了，就没有必要再往下去遍历二叉树A了。也可以说是剪枝欸但一个过程
        }
        boolean flag1 = false; /// 用来记录当前节点的左子树中的查找结果(其实也是包含了匹配的过程)，如果查找成功（包含了匹配过程）返回true
        boolean flag2 = false; /// 用来记录当前节点的右子树中的查找结果(其实也是包含了匹配的过程)，如果查找成功（包含了匹配过程）返回true
        if (node.left != null) {
            flag1 = dfs(node.left, root2); /// 当前节点的val不等于二叉树B的root值，那么就去遍历当前节点的左子树，看否找到二叉树B
        }
        if ((!flag1) && node.right != null) { /// !flag1-》剪枝
            flag2 = dfs(node.right, root2); /// 当前节点的val不等于二叉树B的root值，那么就去遍历当前节点的右子树，看否找到二叉树B
        }
        return flag1 || flag2; /// || -》只需要找到节点的某一个方向的子树进行匹配成功就行了
    }

    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        if (root1 == null || root2 == null) { /// root1 == null -> 就是二叉树A就是一颗空树， root2 -> 约定空树不是任意一个树的子结构
            return false;
        }
        return dfs(root1, root2);
    }
~~~