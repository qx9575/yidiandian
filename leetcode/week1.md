# 第一周LeetCode  #
题目列表中类似题目会放在一起，每个题目先总结自己的思路想法以及代码，然后分析与标准答案和最优代码与自己代码差距
## 题目列表 ##
题目带有leetcode链接（少打字，带点颜色比较好看）

1. Two Sum
	1. [Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
	2. Two Sum III - Data structure design
	3. Two Sum IV - Input is a BST
	4. 3Sum
	5. 4Sum
	6. Subarray Sum Equals K
2. 
## 回顾思路 ##
### 分析 ###
cess to  Markdown syntax and convenient keyboard [shortcut](#mycode)
### 代码 ###
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

### See your changes instantly with LivePreview ###

Don't guess if your [hyperlink syntax](http://markdownpad.com) is correct; LivePreview will show you exactly what your document looks like every time you press a key.
哥特 

### Make it your own  ###

Fonts, color schemes, layouts and stylesheets are all 100% customizable so you can turn MarkdownPad into your perfect editor 

### A robust editor for advanced Markdown users ###

MarkdownPad supports multiple Markdown processing engines, including standard Markdown, Markdown Extra (with Table support) and GitHub Flavored Markdown.

With a tabbed document interface, PDF export, a built-in image uploader, session management, spell check, auto-save, syntax highlighting and a built-in CSS management interface, there's no limit to what you can do with MarkdownPad.
<span id="mycode"></span>