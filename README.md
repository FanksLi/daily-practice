# daily-practice

### 字符串转换整数
1. 读入字符串并丢弃无用的前导空格
2. 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
3. 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
4. 将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
5. 如果整数数超过 32 位有符号整数范围 [−231,  231 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −231 的整数应该被固定为 −231 ，大于 231 − 1 的整数应该被固定为 231 − 1 。
6. 返回整数作为最终结果。

~~~javascript
/**
 * @param {string} s
 * @return {number}
 */
var myAtoi = function (s) {
    const len = s.length;
     let index = 0;

     while(s[index] === ' ' && index < len ) {
         index++;
     }

     if(index >= len) return 0;

     let sign = 1;
     if(s[index] === '-' || s[index] === '+') {
         if(s[index] === '-') {
             sign = -1;
         }
         index++;
     }

     let result = 0;
     while(index < len) {
         let num = ~~s[index];
        if(s[index].charCodeAt(0) < '0'.charCodeAt(0) || s[index].charCodeAt(0) > '9'.charCodeAt(0)) break;
         result = result * 10 + num;
         index++;
     }

     const max = 2 ** 31 -1;
     const min = -(2 ** 31);
     if((sign * result) > max) {
         return max;
     } else if((sign * result) < min) {
        return min;
     }
    return result * sign;
};
~~~

### 二维数组中的查找
在一个 n * m 的二维数组中，每一行都按照从左到右 非递减 的顺序排序，每一列都按照从上到下 非递减 的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。
~~~javascript
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var findNumberIn2DArray = function(matrix, target) {
    
    for(let arr of matrix) {
        const index = search(arr, target);
        if(index !== -1) return true;
    }
        return false;
};

function search(data, target) {
    let low = 0, high = data.length - 1;
    while(low <= high) {
        const mid = low + ((high - low) >> 1);
        const num = data[mid];
        console.log(num);
        if(num === target) {
            return mid;
        } else if(num > target) {
            high = mid - 1;
        } else if(num < target) {
            low = mid + 1;
        }
    }
    return -1;
}
~~~

### 重建二叉树
题目：

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输
入前序遍历序列 {1,2,4,7,3,5,6,8} 和中序遍历序列 {4,7,2,1,5,3,8,6}，则重建二叉树并返回。

思路：
利用递归的思想来求解，首先先序序列中的第一个元素一定是根元素。然后我们去中序遍历中寻找到该元素的位置，找到后该元素的左
边部分就是根节点的左子树，右边部分就是根节点的右子树。因此我们可以分别截取对应的部分进行子树的递归构建。使用这种方式的
时间复杂度为 O(n)，空间复杂度为 O(logn)。
~~~javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    if(preorder === null || preorder.length === 0) return null;

    const root = new TreeNode(preorder[0]);
    const stack = [];

    stack.push(root);

    let index = 0;

    for(let i = 1; i < preorder.length; i++){
        const val = preorder[i];

        let node = stack[stack.length - 1];

        if(node.val !== inorder[index]) {
            node.left = new TreeNode(val);
            stack.push(node.left);
        } else {
            while(stack.length > 0 && stack[stack.length - 1].val === inorder[index]) {
                node = stack.pop();
                index++;
            }

            node.right = new TreeNode(val);
            stack.push(node.right);
        }
    }

    return root;
};
~~~
