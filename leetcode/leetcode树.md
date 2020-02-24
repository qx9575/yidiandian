<script type="text/javascript" async src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML"> </script>


题目列表中类似题目会放在一起，每个题目先总结自己的思路想法以及代码，然后分析与标准答案和最优代码与自己代码差距（在能看到官方silution的条件下），同时有看到更好的办法或者发现我代码有问题（基本不可能，所有代码都跑过的）欢迎给我留言：792869003@qq.com 
## 题目列表 ##
题目带有leetcode链接（少打字，带点颜色比较好看），如果没有链接的就是需要付费的，就只能自己找题目

1. [100-相同的树](https://leetcode-cn.com/problems/same-tree/)
2. [111-二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)
3. [104. 二叉树的最大深度](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
4. [101.对称二叉树](https://leetcode-cn.com/problems/symmetric-tree)
## 思路与总结 ##
### 题目目的 ###
1、给出两棵树，判断他们是否相同(节点值相同、树的结构相同)

2、计算根节点到叶子节点的最小深度 

3、计算根节点到叶子节点的最大深度
### 思路 ###
- 判断相同需要遍历树，第一反应就是递归---------[代码链接以及结果](#code1) 
	- 效果还行，常规解法，但是太多判断语句，整理了下思路优化代码---------[优化代码](#code2) 

- 二叉树的最小深度只要遍历到叶子节点就停止，也是用递归做----------[代码链接以及结果](#code3)
    - 看了效果，发现内存使用过多，怀疑递归深度过多导致，写了bfs非递归方式----------[代码链接以及结果](#code4) 
    - 在看了讨论区后，换了一种更简单的写法，上面使用两个变量记录当前层和下一层的节点数，可以直接记录一个队列长度代替，可读性个人觉得更强----------[代码链接以及结果](#code5) 
- 二叉树的最大深度刚好和最小深度相反，一个左右取最小一个左右取最大，同时非递归算法也和最小深度类似----------[代码链接以及结果](#code6)
- 对称二叉树的递归就判断每个节点的子节点是否对称，非递归算法最开始手动计算每层节点开始比较的位置，后面改用一个队列和堆栈存----------[代码链接以及结果](#code7)

### 大佬优秀代码 ###
- 最大深度，不好理解，没事常看看

		class Solution {
			public:
    		int max;
    		void maxDph(TreeNode* root, int depth)
    		{
        		if(root == NULL)
        		{
            		if((depth - 1) > max)
                		max = depth - 1;
            		return;
        		}else
        		{
            		if(depth > max) max = depth;
        		}
				if(root->left != NULL) maxDph(root->left, depth + 1);
        		if(root->right != NULL) maxDph(root->right, depth + 1);
    		}
    		int maxDepth(TreeNode* root) {
        		max = 0;
        		maxDph(root, 1);
        		return max;
    		}
		};
### 代码 ###
<span id="code1">相同的树 递归</span>

    /**
 		* Definition for a binary tree node.
 		* struct TreeNode {
 		*     int val;
 		*     TreeNode *left;
 		*     TreeNode *right;
 		*     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 		* };
 	*/
	class Solution {
	public:
    	bool isSameTree(TreeNode* p, TreeNode* q) {
        bool l_same = false;
        bool r_same = false;
        bool val_same=false;
        if (p != NULL && q != NULL)
            val_same=p->val==q->val;
        else if(p==NULL&&q==NULL)
            return true;
        if(!val_same)
            return false;
        if (p->left == NULL && q->left == NULL)
            l_same = true;
        else if (p->left != NULL && q->left != NULL)
            l_same = isSameTree(p->left, q->left);
        else
            l_same = false;
        if(!l_same)
            return false;
        if (p->right == NULL && q->right == NULL)
            r_same = true;
        else if (p->right != NULL && q->right != NULL)
            r_same = isSameTree(p->right, q->right) ;
        else
            r_same = false;
        if(!r_same)
            return false;
        return true ;
    	}
    
	};
![](https://i.imgur.com/sh57iyF.png)

<span id="code2">优化后的递归</span>

    /**
 		* Definition for a binary tree node.
 		* struct TreeNode {
 		*     int val;
 		*     TreeNode *left;
 		*     TreeNode *right;
 		*     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
		* };
 	*/
	class Solution {
	public:
    	bool isSameTree(TreeNode* p, TreeNode* q) {
        	if(p==NULL&&q==NULL)
            	return true;
       	 	else if(p==NULL||q==NULL)
            	return false;
        	else {
            	if(p->val==q->val)
                	return isSameTree(p->left, q->left)&&isSameTree(p->right, q->right);
            	return false;
        	}
    	}  
	};
![](https://i.imgur.com/1jIcIPl.png)

<span id="code3">二叉树的最小深度</span>  

    /**
 		* Definition for a binary tree node.
 		* struct TreeNode {
 		*     int val;
 		*     TreeNode *left;
 		*     TreeNode *right;
 		*     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 		* };
 	*/
		class Solution {
			public:
    			int minDepth(TreeNode* root) {
        		if(root==NULL){
            		return 0;
        		}
        		if (root->left == NULL && root->right != NULL) {
            		return 1 + minDepth(root->right);
        		}
        		if (root->right == NULL && root->left != NULL) {
            		return 1 + minDepth(root->left);
        		}
        		return 1+min( minDepth(root->left), minDepth(root->right));
    	}
	};

	执行用时 :20 ms, 在所有 cpp 提交中击败了35.64%的用户
	内存消耗 :20.1 MB, 在所有 cpp 提交中击败了10.86%的用户



<span id="code4">二叉树的最小深度 BFS（双变量记录）</span> 
		
		int minDepthNotRecurisive(TreeNode* root) {
			if (!root)
				return 0;
			int minDepth = 1;
			int curNodeCount = 0;
			int nextLayerNodeCount = 0;
			int endFlag = false;
			queue<TreeNode*> nodeList;
			nodeList.push(root);
			curNodeCount++;
			while (!nodeList.empty())
			{
				auto cur = nodeList.front();
				nodeList.pop();
				curNodeCount--;
				//终止条件
				if (cur->left == NULL && cur->right == NULL)
					return minDepth;
				cout << "val:" << cur->val << endl;
				if (cur->left != NULL) {
					nodeList.push(cur->left);
					nextLayerNodeCount++;
				}if (cur->right != NULL) {
					nodeList.push(cur->right);
					nextLayerNodeCount++;
				}
				/*if (cur->left == NULL && cur->right == NULL)
					endFlag = true;*/
				if (curNodeCount == 0) {
					minDepth++;
					//逻辑有点问题
					/*if (endFlag)
						break;
					endFlag = false;*/
					curNodeCount = nextLayerNodeCount;
					nextLayerNodeCount = 0;
				}
			}
			return minDepth;
		}


		执行用时 :32 ms, 在所有 cpp 提交中击败了5.43%的用户
		内存消耗 :19.4 MB, 在所有 cpp 提交中击败了94.01%的用户


<span id="code5">二叉树的最小深度 BFS（队列长度）</span> 
		
		int minDepthNotRecurisive2(TreeNode* root) {
			if (!root)
				return 0;
			int minDepth = 1;
			queue<TreeNode*> nodeList;
			nodeList.push(root);
			while (!nodeList.empty())
			{
				int curLayerNodeSize = nodeList.size();
				for (int i = 0; i < curLayerNodeSize; i++) {
					TreeNode* cur= nodeList.front();
					nodeList.pop();
					if (cur->left == NULL && cur->right == NULL)
						return minDepth;
					if (cur->left != NULL) {
					nodeList.push(cur->left);
					}if (cur->right != NULL) {
					nodeList.push(cur->right);
					}
				}
				minDepth++;
			}
			return minDepth;
		}


		执行用时 :4 ms, 在所有 cpp 提交中击败了99.82%的用户
		内存消耗 :19.5 MB, 在所有 cpp 提交中击败了80.71%的用户

<span id="code6">最大深度</span>  

		int maxDepthFirst(TreeNode* root) {
        	int ld=0;
        	int rd=0;
        	if(root==NULL)
           		return 0;
        	if(root->left)
            	ld= maxDepth(root->left);
        	if(root->right)
            	rd= maxDepth(root->right);
        	return max(ld,rd)+1;
    	}
	
		执行用时 :8 ms, 在所有 cpp 提交中击败了92.98%的用户
		内存消耗 :19.7 MB, 在所有 cpp 提交中击败了5.08%的用户

		int maxDepthOptimizied(TreeNode* root) {
        	if(root==NULL)
            	return 0;
        	return max(maxDepth(root->left),maxDepth(root->right))+1;
    	}


		int maxDepthNotRecursive(TreeNode* root){
        	if(root == NULL)
            	return 0;
        	int num = 0;
        	queue<TreeNode *> nodeQueue;
        	nodeQueue.push(root);
        	while(!nodeQueue.empty()){
            	int n = nodeQueue.size();
            	for(int i = 0;i < n;++i){
                	TreeNode *cur = nodeQueue.front();
                	nodeQueue.pop();
                	if(cur->left != NULL)
                    	nodeQueue.push(cur->left);
                	if(cur->right != NULL)
                    	nodeQueue.push(cur->right);
            	}
            	num++;
        	}
        	return num;
    	}

		执行用时 :8 ms, 在所有 cpp 提交中击败了92.98%的用户
		内存消耗 :19.3 MB, 在所有 cpp 提交中击败了60.40%的用户



<span id="code7">对称二叉树</span>

	********************递归版本*********************
    class Solution {
	public:
		bool judgeSame(TreeNode* l, TreeNode* r){
        	if(l == NULL && r == NULL) return true;
        	if(l == NULL || r == NULL) return false;
        	if(l -> val != r -> val) return false;
        	return judgeSame(l -> right, r -> left) & judge(l -> left, r -> right);
    	}
    	bool isSymmetric(TreeNode* root) {
        	if(root == NULL) return true;
        		return judgeSame(root -> left, root -> right);
    	}
	};		
	******************非递归版本*********************
	**********该方法手动计算对应节点位置很麻烦**********
	**********老出错，后面采用队列栈来记录位置**********
	**********不用计算对应节点位置********************
    class Solution {
	public:
		bool isSymmetric_v1(TreeNode* root) {
			vector<TreeNode*> nodeList;
			if (!root)
				return true;
			nodeList.push_back(root);
			int i = 1;
			while (!nodeList.empty()) {
				vector<TreeNode*> vec_temp;
				if (nodeList.size() == 1) {
					if (nodeList[0]->left && nodeList[0]->right && (nodeList[0]->left->val == nodeList[0]->right->val){
						vec_temp.push_back(nodeList[0]->left);
						vec_temp.push_back(nodeList[0]->right);
					}
					else if (!nodeList[0]->left && !nodeList[0]->right) {
						return true;
					}
					else
						return false;
				}
				else{
					int forward_offset = 0;
					int end_offset = 0;
					for (int sIndex = 0, eIndnx = (nodeList.size() - 1); sIndex < eIndnx;) {
						bool is_same = false;
               			int gap=0;
						//结构一样
						if (nodeList[sIndex] && nodeList[eIndnx]) {
						is_same = is_construct_same_v1(nodeList[sIndex], nodeList[eIndnx]);
						}
						if (!is_same)
							return false;
						if (nodeList[sIndex]->left)
							vec_temp.insert(vec_temp.begin() + (forward_offset++), nodeList[sIndex]->left);
		
						if (nodeList[sIndex]->right)
							vec_temp.insert(vec_temp.begin() + (forward_offset++) , nodeList[sIndex]->right);
		
						if (nodeList[eIndnx]->left){
                   	 		gap++;
                    		vec_temp.insert(vec_temp.begin() + vec_temp.size()-end_offset , nodeList[eIndnx]->left);
                		}
					
						if (nodeList[eIndnx]->right){
                    		gap++;
                    		vec_temp.insert(vec_temp.begin() + vec_temp.size() - end_offset, nodeList[eIndnx]->right);
                		}
					
                		end_offset += gap;
						sIndex++;
						eIndnx--;
					}
				}
				nodeList = vec_temp;
			}
			return true;
		}
	};		
	******************非递归版本-最终优化*************
	**********从前往后和从后往前找节点对应完全可********
	*******以在入队顺序那里处理，代码简洁很多***********
    class Solution {
	public:
		bool isSymmetric(TreeNode* root) {
        	deque<TreeNode*> d;
        	d.push_back(root);
        	d.push_back(root);
        
        	while( !d.empty() ) {
            	TreeNode* t1 = d.front();
            	d.pop_front();
            	TreeNode* t2 = d.front();
            	d.pop_front();
            
            	if( t1 == nullptr && t2 == nullptr) continue; //注意！这里是continue，不是直接递归的时候那样可以直接返回true了！
            	if( t1 == nullptr || t2 == nullptr) return false;
            
            	if(t1->val != t2->val) return false;
            
            	d.push_back(t1->right);
            	d.push_back(t2->left);
            	d.push_back(t1->left);
            	d.push_back(t2->right);
        	}
     
        	return true;
    	}   
	};		
	


### 分析Two Sum ###
除了bst以外，都是通过hash表来获得最快的处理，而bst中我采用递归遍历，算法过程和普通的类似，但是最终的出来的时间会比先遍历所有节点，在来查找满足要求的慢，原因个人觉得是由于递归的栈处理时间导致。同时在多个数字相加求和的都是采用先排序后两个指针同时移动来加快遍历。
