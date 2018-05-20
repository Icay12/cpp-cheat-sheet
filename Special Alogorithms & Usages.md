#Special Algorithms

##KMP

##Uninon Find
```cpp
class UnionFind{
private:
    int *id;
    int *rank;
    int cnt;

public:
    UnionFind(int n) {
        id = new int[n];
        rank = new int[n];
        cnt = n;
    }

    ~UnionFind() {
        delete [] id;
        delete [] rank;
    }

    bool isConnected(int a, int b) {
        return find(a) == find(b);
    }

    void connect(int a, int b) {
        int i = find(a);
        int j = find(b);
        if(i == j)
            return;
        if(rank[i] > rank[j]) { //connect j to i
            id[j] = i;
        }
        else if(rank[i] < rank[j]) { //connect i to j
            id[i] = j;
        }
        else {
            id[j] = i;
            rank[i]++;
        }
        cnt--;
    }

    int find(int a) {
        while(a != id[a]) {
            id[a] = id[id[a]];
            a = id[a];
        }
        return a;
    }

    int getCount() {
        return cnt;
    }

};
```


##Bucket Sort
```cpp
//347 Leetcode
//bucket sort O(n)
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> count;
        vector<vector<int>> bucket(nums.size()+1);
        for(int i = 0; i < nums.size(); ++i) {
            ++count[nums[i]];
        }
        for(auto it : count) {
            bucket[it.second].push_back(it.first);
        }
        reverse(bucket.begin(), bucket.end());
        vector<int> res;
        for(auto it_b : bucket) {
            for(int element : it_b) {
                res.push_back(element);
                if(res.size() == k) {
                    return res;
                }
            }
            
        }
        return res;
        
    }
};
```

##Trie Tree
```cpp
class TrieNode {
public:
    TrieNode* next[26] = {NULL};
    bool isEndofWord;
    TrieNode() {
        isEndofWord = false;
    }

    void setWord() {
        isEndofWord = true;
    }

    bool isWord() {
        return isEndofWord;
    }

};

class TrieTree {
private:
    TrieNode *root;
public:
    TrieTree() {
        root = new TrieNode();
    }

    void insert(string word) {
        TrieNode *cur = root;
        for(int i = 0; i < word.length(); ++i) {
            int index = word[i] - 'a';
            if(!cur->next[index]) {
                cur->next[index] = new TrieNode();
            }
            cur = cur->next[index];
        }
        cur->setWord();
    }

    bool search(string word) {
        TrieNode *cur = root;
        for(int i = 0; i < word.length(); ++i) {
            int index = word[i] - 'a';
            if(!cur->next[index]) {
                return false;
            }
            cur = cur->next[index];
        }
        return cur->isWord();
    }

    bool startsWith(string prefix) {
        TrieNode *cur = root;
        for(int i = 0; i < prefix.length(); ++i) {
            int index = prefix[i] - 'a';
            if(!cur->next[index]) {
                return false;
            }
            cur = cur->next[index];
        }
        return true;
    }
};
```

##Segment Tree

leetcode 307

```cpp
class segmentNode{
public:
    int begin, end;
    segmentNode *left = NULL, *right = NULL;
    int sum;
    segmentNode(int begin, int end) {
        this->begin = begin;
        this->end = end;
        sum = 0;
    }
};

class NumArray {
private:
    segmentNode *root;
public:
    NumArray(vector<int> nums) {
        root = buildTree(nums, 0, (int)nums.size()-1);
    }
    
    segmentNode * buildTree(vector<int>& nums, int begin, int end) {
        if(begin > end) {
            return NULL;
        }
        segmentNode* cur = new segmentNode(begin, end);
        if(begin == end) {
            cur->sum = nums[begin];
            return cur;
        }
        int mid = (begin+end)/2;
        cur->left = buildTree(nums, begin, mid);
        cur->right = buildTree(nums,mid+1, end);
        cur->sum = cur->left->sum + cur->right->sum;
        return cur;
    }
    
    void update(int i, int val) {
        update(root, i, val);
    }
    
    void update(segmentNode *cur, int i, int val) {
        if(cur->begin == cur->end)
            cur->sum = val;
        else {
            int mid = (cur->begin + cur->end) / 2;
            if(i <= mid)
                update(cur->left, i, val);
            else
                update(cur->right, i, val);
            cur->sum = cur->left->sum + cur->right->sum;
        }
        
    }
    
    int sumRange(int i, int j) {
        return sumRange(root, i, j);
    }
    
    int sumRange(segmentNode *cur, int i, int j) {
        if(cur->begin == i && cur->end == j)
            return cur->sum;
        int mid = (cur->begin + cur->end) / 2;
        if(j <= mid) {
            return sumRange(cur->left, i, j);
        }
        else if(i > mid) {
            return sumRange(cur->right, i, j);
        }
        else {
            return sumRange(cur->left, i, mid) + sumRange(cur->right, mid+1, j);
        }
    }
};
```

##Topological Sort
```cpp
//maybe you can use this to present graphs
vector<unordered_set<int>> graph(numCourses);
vector<int> indegree(numCourses, 0);
```

##AVL Tree
[GeeksForGeeks -- AVL](https://www.geeksforgeeks.org/avl-tree-set-1-insertion/)

**Rule**
The height different between a left child and a right child with the same parent, should be equal to or less than 1.

**key points**

1. Do the balance operation when insert and delete
2. Right Rotate & Left Rotate
3. 4 kinds of status
	1. left-left: right rotate
	2. right-right: left rotate
	3. left-right: left rotate, right rotate
	4. right-left: right rotate, left rotate

##Red-Black Tree

**AVL vs Red-Bladk**
AVL: less insertion and deletion, more search operation
Red-Black: more insertion and deletion, less search operation

**Rule**

1. the root must be black
2. no two adjancent red nodes
3. the number of black nodes path 
4. every path for the root to the null node have the same number of black nodes

##Binary Indexed Tree
[GeeksForGeeks -- BIT](https://www.geeksforgeeks.org/binary-indexed-tree-or-fenwick-tree-2/)

![](https://www.geeksforgeeks.org/wp-content/uploads/BITSum.png)



![](https://www.geeksforgeeks.org/wp-content/uploads/BITUpdate12.png)


```cpp
class BIT{
private:
    vector<int> bit;
public:
    BIT(vector<int> nums) {
        bit = vector<int>(nums.size()+1, 0);
        for(int i = 0; i < nums.size(); ++i) {
            insert(i, nums[i]);
        }
    }
    
    void insert(int i, int val) {
        while(i < bit.size()) {
            bit[i] += val;
            i += i & -i;
        }
    }
    
    int search(int i) {
        int res = 0;
        while(i > 0) {
            res += bit[i];
            i -= i & -i;
        }
        return res;
    }
};
```

**Note**: If we use `i += i &-i` for search and `i -= i & -i` for insert, the sum we get for `search(i)`, is sum of ith element to the last element.


##Binary Search
###Lower Bound (first element, >= val)


```cpp
int lowerBound(vector<int> nums, int val) {
    int begin = 0, end = (int)nums.size() - 1;
    while(begin <= end) {
        int mid = begin + ((end - begin) >> 1);
        if(nums[mid] < val) {
            begin = mid + 1;
        }
        else  {
            end = mid - 1;
        }
    }
    return begin;
}
```

###Upper Bound (first element, > val)

```cpp
int upperBound(vector<int> nums, int val) {
    int begin = 0, end = (int)nums.size() - 1;
    while(begin <= end) {
        int mid = begin + ((end - begin) >> 1);
        if(nums[mid] <= val) {
            begin = mid + 1;
        }
        else  {
            end = mid - 1;
        }
    }
    return begin;
}
```


#Special Cpp Usage

## Lambda for Sort()

```cpp
vector<int> vec = { 1,2,3,4,5,6,7,8,9 };
std::sort(std::begin(vec ), std::end(vec ), [](int a, int b) {return a > b; });
```

## Priority Queue
```cpp
\\max heap
priority_queue<int> heap;

\\min heap
priority_queue<int, vector<int>, greater<int> >heap2;
```

**self-defined priority queue**

[Leetcode 692](https://leetcode.com/problems/top-k-frequent-words/discuss/108366/O(nlog(k))-Priority-Queue-C++-code)

```cpp
//1. use struct, overload operator()
priority_queue<pair<string,int>, vector<pair<string,int>>, Comp> pq;

struct Comp {
    Comp() {}
    ~Comp() {}
    bool operator()(const pair<string,int>& a, const pair<string,int>& b) {
        return a.second>b.second || (a.second==b.second && a.first<b.first);
    }
};
```

```cpp
//2. use lambda and decltype

auto compare = [](const pair<string,int>& a, const pair<string,int>& b){
            return a.second > b.second || (a.second == b.second && a.first < b.first);
        };
priority_queue<pair<string,int>, vector<pair<string,int>>, decltype(compare)> bucket(compare);

\\\\\\\\decltype can be replaced by function<>

priority_queue<pair<string,int>, vector<pair<string,int>>,function<bool(const pair<string,int>&, const pair<string,int>&)>> bucket(compare);

```

## Multiset
```cpp
vector<int> nums = {1,2,3,3,4,5};
multiset<int> s(nums.begin(), nums.begin() + 5); // 1, 2, 3, 3, 4
multiset<int>::iterator it = next(s.begin(), 5/2); // point to the middle 3
multiset<int>::iterator it2 = prev(s.end(), 1); // point to the 4
s.erase(lower_bound(s.begin(), s.end(), s) //erase the element the iterator points to
s.remove(3) //remove all element with the value 20
```

## Itegrator Related
```cpp
vector<int> nums = {1,2,3,3,3,4,5}
*nums.begin(); // 1
*(nums.end()-1); // 5
lower_bound(nums.begin(), nums.end(), 3);// point to the first 3
upper_bound(nums.begin(), nums.end(), 3);// point to 4

``` 

## decltype