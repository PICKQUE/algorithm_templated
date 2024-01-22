## 位

## 矩阵

* 八个方向
    ```C++
        n = nums.size(), m = nums[0].size();
        for (int i = min(i - 1, 0); i < min(i + 1, n - 1); i ++)
            for (int j = min(j - 1, 0); j < min(i + 1, m - 1); j ++)
                if (i == x && j == y) continue; // 排掉自己
            ...
    ```

* 四个方向

    ```C++
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};
    for (int i = 0; i < 4; i ++) {
        int a = x + dx[i];
        int b = y + dy[i];
    }
    ```
## 二叉树
* 公共祖先
  ```C++
    TreeNode* find_ancestor() {
        if (!root) return null;
        if (root == p || root == q) return root;
        left = dfs(root->left, p, q);
        right = dfs(root->right, p, q);
        if (left && right) return root;
        if (left) return left;
        if (right) return right;
        return null;
    }
  ```

### 二叉搜索树
 * 验证二叉搜索树
    ```C++
    if (root->val <= l_min || root->val >= l_max) return false;
    return dfs(root->left, l_min, root->val) && dfs(root->right, root->val, l_max);
    ```

## 深度优先搜索

```C++
void dfs(int u) {
    if (u) {return;}
    dfs(u + 1)
}
```

## 链表

* 反转链表
  ```C++
    prev = nullptr;
    tmp = cur->next;
    cur->next = prev;
    prev = cur;
    cur = tmp;
  ```
* 分治
  * 链表排序
    ```C++
    // 用快慢指针来寻找 mid
    ListNode* merge(ListNode* head, ListNode* tail)  {
        if (!head) return nullptr;
        if (head->next == tail) {
            head->next = nullptr;
            return head;
        }
        ListNode* slow = head, *fast = head;
        while(fast != tail) {
            slow = slow->next;
            fast = fast->next;
            if (fast != tail) {
                fast = fast->next;
            }
        }
        ListNode *mid = slow;
        return merge_two_list(merge(head, mid), merge(mid, tail));
    }
    ListNode* sortList(ListNode* head) {
        return merge(head, nullptr);
    }
    ```

## 回溯

* 组合

    ```C++
    void dfs(int cur, int k) {
        if (k == line.size()) {
            ans.emplace_back(line);
            return ;
        }
        line.push_back(cur + 1);
        dfs(cur + 1, k); // 选择 cur + 1 的情况
        line.pop_back(cur + 1);
        dfs(cur + 1, k); // 不选择 cur + 1 的情况
    }
    ```

* 排列
    ```C++
    void dfs(vector nums) {
        if (line.size() == nums.size()) {
            ans.emplace_back(line);
            return;
        }
        for (int i = 0; i < nums.size(); i ++) {
            if (visit[i]) continue;

        }
    }
    ```


## 图

* 克隆图
    ```C++
        unordered_map<Node*, Node*> visited;
        Node* cloneGraph(Node* node) {
            if (!node) return nullptr;
            if (visited.find(node) != visited.end()) {
                return visited[node];
            }
            Node *cloneNode = new Node(node);
            visited[node] = cloneNode;
            for (auto &neighbor : node->neighbors) {
                cloneNode->neighbors.emplace_back(cloneGraph(neighbor));
            }
            return cloneNode;
        }
    ```

* 拓扑排序
    // 判断队列中是否存在环
    ```C++
        // 建边 建入度
        for (int i = 0; i < n; i ++) {
            edges[vec[1]];push_back(vec[0]);
            in[vec[0]] ++;
        }
        queue<int> q;
        for (int i = 0; i < n; i ++) {
            if (in[i] == 0) { // 入度为零入队
                q.push(i);
            }
        }
        if (q.empty()) return false; // 队空
        int cnt = 0; // 判断节点数
        while (q.size()) {
            cnt ++;
            auto t = q.front();
            q.pop();
            for (auto edge : edges) {
                if (--in[edge] == 0) q.push(edge);
            }
        }
        return cnt == n; // 判断是否图中的点是否全都入过队
    ```

## 二分查找

* 寻找峰值

    ```C++
        if (nums[mid] < nums[mid + 1]) r = mid;
        else l = mid + 1;
    ```
    
## 堆

* 大顶堆
    ```C++
        void down(vector<int>& heap, int u, int n) {
            int max = u;
            if (u * 2 < n && heap[u * 2] > heap[max]) max = u * 2;
            if (u * 2 + 1 < n && heap[u * 2 + 1] > heap[max]) max = u * 2 + 1;
            if (max != u) {
                swap(heap[max], heap[u]);
                down(heap, max, n);
            }
        }

        void build_heap(vector<int>& heap) {
            int n = heap.size();
            for (int i = n / 2; i >= 0; i --) {
                down(heap, i, n);
            }
        }

        void heap_sort(vector<int>& heap) {
            build_heap(heap);
            int n = heap.size();
            for (int i = n - 1; i >= 0; i ++) {
                swap(heap[0], heap[i]);
                down(heap, 0, i);
            }
        }
    ```

## 并查集
```C++
    vecotr<int> p(n);
    void init(vector<int>& p) {
        for (int i = 0; i < n; i ++) {
            p[i] = i;
        }
    }
    int find(int x) {
        if (x != p[x]) p[x] = find(p[x]); // 路径压缩 保证每个字节点都能找到并指向自己的祖先节点
        return p[x];
    }

    int _union(int a, int b) {
        if (find(a) != find(b)) {
            p[a] = b;  // 将 a 合并到 b 上去
        } 
    }
```

## 动态规划

* 距离动态规划

```C++
    for (int L = 2; L <= n; L ++) {
        for (int i = 0; i < n;  i++) { // left index
            int j = L + i - 1; // right index
            if (j >= n) break;
            if (s[i] != s[j]) dp[i][j] = false;
            else if (j - i < 3) dp[i][j] = true;
            else {
                dp[i][j] = dp[i + 1][j - 1];
            }
            
            if (dp[i][j] && L > res) {
                L = res; // res length;
                begin = i;
            }
        }
    }

    return str.substr(begin, res);
```