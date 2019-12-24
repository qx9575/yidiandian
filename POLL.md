# poll  #
题目列表中类似题目会放在一起，每个题目先总结自己的思路想法以及代码，然后分析与标准答案和最优代码与自己代码差距（在能看到官方silution的条件下），同时有看到更好的办法或者发现我代码有问题（基本不可能，所有代码都跑过的）欢迎给我留言：792869003@qq.com 
## 使用的函数 ##
- [socket](#code1)
- [fcntl](#code2)
- [bind](#code3)
- [listen](#code3)
- [setsockopt](#code4)
- [poll](#code5)
- [accept](#code6)
- [recv](#code7)
## 使用的结构体 ##
- [pollfd](#code8)
## 使用的常量 ##
- [EWOULDBLOCK](#code9)
- [EINTR](#code10)
- [POLLIN](#code11)
- [F_GETFL](#code12)
- [O_NONBLOCK](#code13)
- [SO_REUSEADDR](#code14)
- [SOL_SOCKET](#code15)
- [SO_REUSEPORT](#code16)
## 函数详解 ##
    #include<sys/types.h>:数据类型定义
    #include<sys/socket.h>：scoket相关函数及数据结构
	#include<arpa/inet.h>：IP地址转换函数
	#include<errno.h>：提供错误号errno的定义
	#include<fcntl.h>：提供对文件控制的函数



socket(int domain, int type, int protocol) 
	

给出一串数字和一个目标数字，找到数字串中相加能得到目标数字的组合。我们可以获得以下信息

- 数字串不排序（排序后的数字串是否更快）
- 一定有解（可以考虑无解的判断，求出所有解）
- 不能重复使用
- 两个相加（可以考虑多个相加，只要能不重复就行）
### 思路 ###
- 先看简单一点的，这就是Two Sum（一定有解，两个数字相加)
	- 我的第一反应就是两次循环暴力无脑求解（能解决问题就行），可以看结果很慢(*>﹏<*) ，分析一波后发现没救了$O\left( n^{2}\right)$，换个思路---------[代码链接以及结果](#code1)      
	- 用map直接可以找到对应的元素不用遍历，节约的时间就是暴力中第二次循环，时间复杂度：$O\left( n\right)$，然后发现贼快，还可以把map换成unordered_map，这是因为底层实现不同（请参考MAP的实现原理），不过LeetCode测试结果一样  ---------[代码链接以及结果](#code2)  
	- 在官方的solutions还有个更快的，在初始化map的时候就检查是否满足要求,然后代码也是最简单---------[代码链接以及结果](#code3)   
- 如果输入数据排序后就变成了Two Sum II - Input array is sorted
	- 我们可以直接用上面的任何一种，只是需要把下标换一下
- Two Sum III - Data structure design找到了题目，可以直接用map、set这些做，就是add(n)添加数字,find(m)判断是否存在
- Two Sum IV - Input is a BST高级了点，输入是一个二叉搜索树，然后判断是否存在两个数字相加等于目前值
	- 想的是把二叉树遍历一次，所有值存数组就是之前的算法---------[代码链接以及结果](#code6)
- 考虑3个数字相加,这就是3Sum
	- 第一反应暴力上次循环无脑，结果不用想肯定垃圾，结果超出想象居然超时了---------[代码链接以及结果](#code4)  
	- 想了一下，这是3个数字求和为0，我们可以先排序（从小到大），依次选一个负数然后找出两个数与他的和为零，我们只需要遍历所有的负数，然后在通过双下标找到另外两个数。发现速度还行，暂时想不出更好的---------[代码链接以及结果](#code5)  
- 3Sum可能出现给出的和找不到，那就输出最接近他的三个数字和，这就是变成了3Sum Closest---------[代码链接以及结果](#code7) 
	- 还是先排序，然后找个变量记录差值，最终要差值最小  
- 3Sum我们现在找出所有比目标值小的，这就是3Sum Smaller，时间要求是O(n*n)，我们可以参考3Sum Closest和3Sum，先排序，在遍历，不过有个技巧我们在利用双指针遍历的时候，如果三者的和小于目标值，这两个指针之间的都满足要求不再加一而是加他们的差值。
- 解决了三个数字求和后，我们尝试4个数字求和（4Sum），理论上我们四次循环一定能找到，但是时间上不满足我们的要求。我们可以参考3Sum，在外面添加一层循环---------[代码链接以及结果](#code8) 
- 如果不限定相加元素的个数，就是类似Subarray Sum Equals K，由于是连续子串求和，和图像里面积分图很类似，所以先计算从0-n的所有元素和放在n位置，任意两个区间n-m之间子串的和就是sum(m)-sum(n-1),这里实现分别实现了最简单的两次遍历和参考2sum的遍历，**要考虑等于相减为零的情况a[0]++**---------[代码链接以及结果](#code9) 
### 代码 ###
<span id="code1">2Sum 暴力法</span>

    class Solution {
            public:
            std::vector<int> twoSum(std::vector<int>& nums, int target) {
                std::vector < int > resu;
                for (int i = 0; i < nums.size() - 1; i++)
                {
                    for (int j = i + 1; j < nums.size(); j++)
                    {
                        if ((nums[i] + nums[j]) == target) {
                            resu.emplace_back(i);
                            resu.emplace_back(j);
                            return resu;
                        }
                    }
                }
            }
        };
![](https://i.imgur.com/Pn6a497.png)
