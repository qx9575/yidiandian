<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
# 第一周LeetCode  #
题目列表中类似题目会放在一起，每个题目先总结自己的思路想法以及代码，然后分析与标准答案和最优代码与自己代码差距
## 题目列表 ##
题目带有leetcode链接（少打字，带点颜色比较好看），如果没有链接的就是需要付费的，后面集中处理

1. Two Sum
	1. [Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
	2. Two Sum III - Data structure design
	3. [Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)
	4. [3Sum](https://leetcode.com/problems/3sum/)
	5. [4Sum](https://leetcode.com/problems/4sum/)
	6. [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
2. 
## 回顾思路 ##
### 题目目的 ###
给出一串数字和一个目标数字，找到数字串中相加能得到目标数字的组合。我们可以获得以下信息

- 数字串不排序（排序后的数字串是否更快）
- 一定有解（可以考虑无解的判断，求出所有解）
- 不能重复使用
- 两个相加（可以考虑多个相加，只要能不重复就行）
### 思路 ###
- 考虑最简单的，只要能accept就行，这就是Two Sum（一定有解，两个数字相加)
	- 我的第一反应就是两次循环暴力无脑求解（能解决问题就行），可以看结果很慢(*>﹏<*) ，分析一波后发现没救了$O\left( n^{2}\right)$，换个思路---------[代码链接以及结果](#code1)      
	- 用map直接可以找到对应的元素不用遍历，节约的时间就是暴力中第二次循环，时间复杂度：$O\left( n\right)$，然后发现贼快，还可以把map换成unordered_map，底层实现不同，不过LeetCode测试结果一样  ---------[代码链接以及结果](#code2)  
	- 在官方的solutions还有个更快的，在初始化map的时候就检查是否满足要求,然后代码也是最简单---------[代码链接以及结果](#code3)   
- 考虑3个数字相加
### 代码------------------------------同样的代码有时候时间是不一样的 ###
####code1####
<span id="code1">暴力法</span>

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
####code2####
<span id="code2">hashmap-twopass</span>

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
####code3####
<span id="code2">hashmap-onepass</span>

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
### 跑的最快的代码 ###
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
### 分析 ###
感觉没区别，又拿去跑了一次时间是4ms，可能和他的测试用例有关。
