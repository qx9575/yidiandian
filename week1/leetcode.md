<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>

# 第一周LeetCode  #
题目列表中类似题目会放在一起，每个题目先总结自己的思路想法以及代码，然后分析与标准答案和最优代码与自己代码差距（在能看到官方silution的条件下），同时有看到更好的办法或者发现我代码有问题（基本不可能，所有代码都跑过的）欢迎给我留言：792869003@qq.com 
## 题目列表 ##
题目带有leetcode链接（少打字，带点颜色比较好看），如果没有链接的就是需要付费的，就只能自己找题目

1. [Two Sum](https://leetcode.com/problems/two-sum/)
	1. [Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
	2. Two Sum III - Data structure design
	3. [Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)
2. [3Sum](https://leetcode.com/problems/3sum/)
	1. [3Sum Closest](https://leetcode.com/problems/3sum-closest/)
	2. [3Sum Smaller](https://leetcode.com/problems/3sum-smaller/)
3. [4Sum](https://leetcode.com/problems/4sum/)
4. [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
## 回顾思路 ##
### 题目目的 ###
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

<span id="code2">2Sum hashmap-twopass</span>

     class Solution {
            public:
            std::vector<int> twoSum(std::vector<int>& nums, int target) {
                std::vector < int > resu;
                std::map < int, int > data;
                int index = 0;
                for (auto & num : nums)
                {
                    data[num] = index++;
                }
                for (int i = 0; i < nums.size() ; i++)
                {
                    if (data.find(target - nums[i]) != data.end() && data[target - nums[i]] != i) {
                        resu.emplace_back(i);
                        resu.emplace_back(data[target - nums[i]]);
                        return resu;
                    }
                }
            }
        };  
![](https://i.imgur.com/3t2DdOD.png)

<span id="code3">2Sum hashmap-onepass</span>  

      class Solution {
            public:
            std::vector<int> twoSum(std::vector<int>& nums, int target) {
                std::unordered_map < int, int > data;
                for (int i = 0; i < nums.size() ; i++)
                {
                    if (data.find(target - nums[i]) != data.end()) {
                        return { i, data[target - nums[i]] };
                    }
                    data[nums[i]] = i;
                }
            }
        };
![](https://i.imgur.com/3t2DdOD.png)

<span id="code6">Two Sum IV - Input is a BST</span> 
		
	  class Solution {
            public:
     		bool findTarget(TreeNode* root, int k) {
        		if(!root)
           	 		return false;
        		unordered_set<int> numset;
        		return visitNode(root,k,numset);
    		}
    		bool visitNode(TreeNode* node,int target,unordered_set<int>& numset)
    		{
        		if(!node)
           			return false;
        		if(numset.find(target-node->val)!=numset.end())
            		return true;
        		numset.insert(node->val);
        		if(visitNode(node->left,target,numset))
           			return true;
        		if(visitNode(node->right,target,numset))
           			return true;
        		return false;
    		}
		};
![](https://i.imgur.com/ECVdL6l.png)
### 2Sum跑的最快的代码 ###
    class Solution {
		public:
    	vector<int> twoSum(vector<int>& nums, int target) {
        	unordered_map<int, int> map;
        	vector<int> answer;
        	for(int i = 0; i < nums.size(); i++) {
            	if(map.find(target-nums[i]) != map.end()) {
                	answer.push_back(map[target-nums[i]]);
                	answer.push_back(i);
                	return answer;
            	} else {
                	map[nums[i]] = i;
            	}
        	}
    	}
	};
<span id="code4">3Sum 暴力法</span>  

      class Solution {
		public:
		std::vector<std::vector<int>> threeSum(std::vector<int>& nums) {
			std::set<std::vector<int >> resu1;
			for (int i = 0; i < nums.size() - 2; i++)
			{
				for (int j = i + 1; j < nums.size() - 1; j++)
				{
					for (int k = j + 1; k < nums.size(); k++)
					{
						if ((nums[i] + nums[j] + nums[k]) == 0) {
							std::vector<int > temp = { nums[i], nums[j], nums[k] };
							std::sort(temp.begin(), temp.end());
							resu1.insert(temp);
						}
					}
				}
			}
			return {resu1.begin(),resu1.end()};
		}
	};
![](https://i.imgur.com/DxnPxPh.png)

<span id="code5">3Sum 只遍历负数</span>  
    
    std::vector<std::vector<int>> threeSum1(std::vector<int>& nums) {
		std::set<std::vector<int >> resu;
		std::sort(nums.begin(), nums.end());
		if (nums.size() < 3 || nums[0] >= 0 || nums[nums.size() - 1] <= 0)
			return {  };
		for (int i = 0; i < nums.size(); i++)
		{
			if (nums[i] > 0)
				break;
			int start = i + 1;
			int end = nums.size() - 1;
			while (start < end) {
				if ((nums[i] + nums[start] + nums[end]) == 0)
				{
					resu.insert({ nums[i] , nums[start] , nums[end] });
					while (start < end&&nums[start] == nums[start + 1])
						start++;
					while (start < end&&nums[end] == nums[end - 1])
						end--;
					start++;
					end--;
				}
				else if ((nums[i] + nums[start] + nums[end]) > 0)
					end--;
				else start++;
			}
		}
		return std::vector<std::vector<int >>(resu.begin(), resu.end());
	}
![](https://i.imgur.com/vv1iwzR.png)

<span id="code7">3Sum Closest</span>

    class Solution {
	public:
    	int threeSumClosest(vector<int>& nums, int target) {
        	std::sort(nums.begin(), nums.end());
			int result = nums[0] + nums[1] + nums[2];
			int diff = target-result;
			for (int i = 0; i < nums.size(); i++)
			{
				int start = i + 1;
				int end = nums.size() - 1;
				while (start < end) {
					int sum = nums[i] + nums[start] + nums[end];
					if (std::abs(sum - target) < diff)
					{
						diff = std::abs(sum - target);
						result = sum;
					}
					if (sum < target)
						start++;
					else
						end--;
				}
			}
		return result;
    	}
	};		
![](https://i.imgur.com/ZIbrKil.png)

<span id="code8">4Sum</span>

     vector<vector<int>> fourSum(vector<int> &nums, int target)
    {
        std::set<std::vector<int>> resu;
        std::sort(nums.begin(), nums.end());
        if (nums.size() < 4 || nums[0] + nums[1] + nums[2] + nums[3] > target)
            return {};
        for (int ii = 0; ii < nums.size() - 3; ii++)
        {
            for (int i = ii + 1; i < nums.size() - 2; i++)
            {
                int start = i + 1;
                int end = nums.size() - 1;
                while (start < end)
                {
                    if ((nums[i] + nums[start] + nums[end] + nums[ii]) == target)
                    {
                        resu.insert({nums[ii], nums[i], nums[start], nums[end]});
                        while (start < end && nums[start] == nums[start + 1])
                            start++;
                        while (i < nums.size() - 2 && nums[i] == nums[i + 1])
                            i++;
                        while (start < end && nums[end] == nums[end - 1])
                            end--;
                        start++;
                        end--;
                    }
                    else if ((nums[i] + nums[ii] + nums[start] + nums[end]) > target)
                        end--;
                    else
                        start++;
                }
            }
        }
        return std::vector<std::vector<int>>(resu.begin(), resu.end());
    }
![](https://i.imgur.com/NzUvyLJ.png)

<span id="code9">Subarray Sum Equals K</span>

     int subarraySum(vector<int>& nums, int k) {
        int sum = 0;
	    int count = 0;
	    for (int i = 0; i < nums.size(); i++)
	    {
		    sum += nums[i];
		    nums[i] = sum;
		    if (nums[i] == k)
			    count++;
	    }
	    for (int i = 1; i < nums.size(); i++)
	    {
		    for (int j = i; j < nums.size(); j++)
		    {
			    if (nums[j] - nums[i-1] == k)
				    count++;
		    }
	    }
	    return count;
    }
![](https://i.imgur.com/cR2xoNG.png)

    int subarraySum1(std::vector<int>& nums, int k) {
		int sum = 0;
		int count = 0;
		std::map<int, int> sumMap;
		sumMap[0]++;
		for (int i = 0; i < nums.size(); i++)
		{
			sum += nums[i];
			if(sumMap.find(sum - k)!=sumMap.end())
				count += sumMap[sum - k];
			sumMap[sum]++;
		}
		return count;
	}
![](https://i.imgur.com/3QVTBsh.png)
### 分析Two Sum ###
除了bst以外，都是通过hash表来获得最快的处理，而bst中我采用递归遍历，算法过程和普通的类似，但是最终的出来的时间会比先遍历所有节点，在来查找满足要求的慢，原因个人觉得是由于递归的栈处理时间导致。同时在多个数字相加求和的都是采用先排序后两个指针同时移动来加快遍历。
