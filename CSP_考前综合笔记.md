# CSP 认证考前综合笔记

> 适用：Python 3.12.3，CCF CSP 认证考前复习。
> 目标：前两题尽量稳；第三、第四题争取低成本抢部分分。

---

# 一、Python 输入与输出

## 1. 基础输入模板

```python
# 1. 读一个整数
n = int(input())

# 2. 读两个整数（空格隔开）
a, b = map(int, input().split())

# 3. 读一行，分成整数列表
nums = list(map(int, input().split()))

# 4. 读一个小数
x = float(input())

# 5. 读一个字符串
s = input()

# 6. 读多行（已知行数 n）
for _ in range(n):
    line = input()
```

## 2. `input().split()` + `map(int, ...)`

```python
n, m, k = map(int, input().split())
```

理解过程：

```text
input()                 -> 读取一行字符串
.split()                -> 按空白切分
map(int, ...)            -> 把每个字符串转成整数
解包                     -> 分配给 n、m、k
```

推荐直接使用：

```python
input().split()
```

而不是：

```python
input().split(' ')
```

因为 `split()` 能更稳地处理连续空格。

## 3. 大数据量输入

```python
import sys
input = sys.stdin.readline
```

然后正常使用：

```python
n = int(input())
```

`readline()` 会保留结尾的 `\n`；`int()` / `float()` 会自动处理，字符串通常需要 `.strip()`。

## 4. 输出

### `print()`

```python
print(1)
print(a, b)
```

### `sep`

```python
print(1, 2, 3, sep=',')
```

### `end`

```python
print(1, end=' ')
```

### `f-string`

```python
x = 6.66666
print(f'{x:.1f}')
print(f'{x:.6f}')
```

`.1f`：保留 1 位小数；`.6f`：保留 6 位小数。

> 重要：题目若要求绝对误差 `< 0.001`，通常直接输出 `:.6f` 更稳。

### `print(*列表)`

```python
nums = [1, 2, 3]
print(*nums)
```

输出：

```text
1 2 3
```

`*` 是解包，不是 C/C++ 指针。

---

# 二、Python 下标与列表

## 1. Python 下标从 0 开始

```python
a = [10, 20, 30]
```

```text
a[0] = 10
a[1] = 20
a[2] = 30
```

| 数学编号 | Python 下标 |
|---|---:|
| a1 | a[0] |
| a2 | a[1] |
| a3 | a[2] |

题目若是 `1..n` 编号，而 Python 需要 `0..n-1`，常见转换：

```python
l -= 1
r -= 1
```

## 2. 为了保留 1-based 下标，可以加一个占位元素

```python
a = [0]
a += list(map(int, input().split()))
```

这样：

```python
a[1]
a[2]
...
```

就对应题目的 `a1, a2, ...`。

## 3. 列表嵌套

把每个任务作为一个列表元素：

```python
tasks = []
for _ in range(n):
    tasks.append(list(map(int, input().split())))
```

得到：

```python
[
    [0, 2, 3, 1],
    [0, 3, 4, 2],
    [1, 5, 6, 7]
]
```

访问：

```python
tasks[i][0]
tasks[i][1]
tasks[i][2]
tasks[i][3]
```

## 4. `append`、`extend`、`+=` 的区别

### `append(x)`：加入一个元素

```python
a.append(5)
```

### `append([1, 2])`

把整个列表作为一个元素加入：

```python
a.append([1, 2])
```

结果形如：

```python
[[1, 2]]
```

### `extend(...)`

把里面的元素逐个加入：

```python
a.extend([1, 2])
```

结果形如：

```python
[1, 2]
```

### `+=`

```python
a += [1, 2]
```

效果类似 `extend`。

如果想把一个列表作为一个元素加入另一个列表：

```python
a += [[1, 2]]
```

结果：

```python
[[1, 2]]
```

---

# 三、列表常用方法

```python
a.append(x)       # 末尾加入一个元素
a.extend(b)       # 把 b 的元素全部加入
a.insert(i, x)    # 在 i 位置插入 x
a.remove(x)       # 删除第一个值为 x 的元素
a.pop()           # 删除并返回最后一个元素
a.pop(i)          # 删除并返回第 i 个元素
a.sort()          # 原地排序
a.reverse()       # 原地反转
a.count(x)        # 统计 x 出现次数
a.index(x)        # 找 x 第一次出现的位置
a.clear()         # 清空
```

注意：

```python
a.sort()
```

会直接修改 `a`，返回值是 `None`。

所以不要写：

```python
a = a.sort()      # 错
```

而应写：

```python
a.sort()           # 对
```

## 补充：集合 `set`（自动去重）

`set` 是一个**自动去重的袋子**：

```python
s = {1, 2, 3}            # 造一个集合
s.add(5)                 # 加一个 → {1,2,3,5}
s.add(3)                 # 重复的加不进去
s.update(range(1, 10))   # 加一整串，自动去重

3 in s                   # 判断在不在 → True / False
sorted(s)                # 转成从小到大排好的列表
len(s)                   # 元素个数
```

两大用途：

1. **去重**：把所有数丢进去，重复的自动消失；
2. **快速查重**：`x in s` 比在列表里 `x in a` 快得多。

真题用法（第39次《水印检查》）：

```python
ans = set()
...
ans.update(range(lo, hi + 1))   # 这个位置可行的 k 全扔进去

for v in sorted(ans):           # 排序后逐行输出
    print(v)
```

## 补充：字典 `dict`（按名字查找）

```python
d = {}                        # 空字典
d["key"] = value              # 存
v = d["key"]                  # 取（key 不存在会报 KeyError）
v = d.get("key", 默认值)       # 安全取
if "key" in d: ...            # 判断是否存在
for k, v in d.items(): ...    # 遍历
```

C++ 的 `map` = Python 的 `dict`。适合"按字符串/编号找信息"（如 HPACK 的查表、名字→数值）。

## 补充：桶计数（统计频率）

```python
cnt = [0] * (MAX + 1)         # MAX = 可能的最大值
for x in a:
    cnt[x] += 1
# cnt[v] 就是 v 出现的次数
```

---

# 四、`sort()` 排序

# 四、`sort()` 排序

## 1. 从小到大

```python
a.sort()
```

## 2. 从大到小

```python
a.sort(reverse=True)
```

## 3. 按某个属性/位置排序：`key=`

假设：

```python
tasks = [
    [o, t, a, b],
    ...
]
```

按 `b` 排序：

```python
tasks.sort(key=lambda x: x[3])
```

按 `b` 从大到小：

```python
tasks.sort(key=lambda x: x[3], reverse=True)
```

## 4. 多关键字排序

```python
tasks.sort(key=lambda x: (第一关键字, 第二关键字), reverse=True)
```

Python 会先比较第一个关键字；如果相同，再比较第二个。

### 咖啡任务中的写法

```python
tasks.sort(key=lambda x: (x[3] / x[2], x[0]), reverse=True)
```

含义：

1. 先按 `b/a` 从大到小；
2. `b/a` 相同时，`x[0]=1` 的普通型排在 `x[0]=0` 的灵活型前面。

因为：

```text
1 > 0
```

---

# 五、函数

基本形式：

```python
def add(a, b):
    return a + b
```

调用：

```python
add(1, 2)
```

---

# 六、递归、DFS、DP、记忆化

## 1. 递归

函数调用自己：

```python
def f(n):
    if n == 0:
        return 0
    return f(n - 1) + n
```

必须有结束条件。

## 2. DFS

DFS = Depth First Search，深度优先搜索。

核心理解：

> 一条路走到底，不行就退回来，再走另一条。

例如每个任务都有“选 / 不选”：

```text
任务1
├── 选
│   ├── 任务2选
│   └── 任务2不选
└── 不选
    ├── 任务2选
    └── 任务2不选
```

DFS 负责“把选择路径走出来”。

## 3. 动态规划 DP

DP = Dynamic Programming，动态规划。

核心理解：

> 相同的子问题不要重复计算，把结果保存下来。

例如：

```python
def f(i, m):
    ...
```

如果不同路径都来到：

```text
f(5, 20)
```

那么只需要算一次。

## 4. DFS 和 DP 的关系

可以这样理解：

```text
DFS：负责“走”
DP：负责“记”
```

很多 DP 可以用“记忆化 DFS”来写。

## 5. `@lru_cache(None)`

导入：

```python
from functools import lru_cache
```

模板：

```python
@lru_cache(None)
def f(i, m):
    ...
```

作用：

> 保存这个函数以前算过的结果；相同参数再次出现时直接返回缓存结果。

适合：

```text
递归 + 大量重复子问题
```

例如：

```python
from functools import lru_cache

@lru_cache(None)
def f(n):
    if n == 0:
        return 0
    return f(n - 1) + n
```

但注意：

```text
f(10^12)
```

如果第一次计算仍然要走 `10^12` 层，那么缓存也救不了。

> 记忆化只能消除“重复计算”，不能把“一次就要做的巨大计算”自动变快。

## 6. `@` 是什么

```python
@lru_cache(None)
def f(...):
```

`@lru_cache(None)` 是一个**装饰器**，表示给下面的函数增加缓存功能。

不是所有函数都必须写 `@`。

## 7. BFS 广度优先搜索（最少步数）

### 直觉：波纹扩散

DFS 是"一条路走到底再回来"；BFS 是**一圈一圈往外扩**：

```text
第0步：起点
第1步：起点一步能到的所有格子
第2步：再往外一圈……
```

**先第 1 圈，再第 2 圈，绝不跳圈** → 每个格子第一次被碰到时，走的必然是最少步数。

### 队列实现"一圈一圈"

队列 = 先进先出（排队买票）：

```text
起点入队
反复：队头取出一个格子 → 它一步能到的新格子全部入队尾
```

第 1 圈的格子比第 2 圈先入队 → 处理顺序天然按圈来。

### 模板（跳马真题：第41次《机器人复健指南》）

```python
from collections import deque

dist = [[-1]*(n+1) for _ in range(n+1)]   # -1=没到过，数字=最少步数
dist[x][y] = 0
q = deque()
q.append((x, y))                          # 起点入队

while q:
    i, j = q.popleft()                    # ① 队头出队
    if dist[i][j] == k:                   # ② 达步数上限，不再扩展
        continue
    for dx, dy in moves:                  # ③ 枚举一步能到的位置
        ni, nj = i + dx, j + dy
        if 1 <= ni <= n and 1 <= nj <= n and dist[ni][nj] == -1:
            dist[ni][nj] = dist[i][j] + 1 # ④ 记录最少步数
            q.append((ni, nj))            # ⑤ 入队尾
```

要点：

1. `dist` 双重身份：**-1 就是 visited 标记**（到过的不再入队，即剪枝），数字就是最少步数；
2. **deque 三件套**：`deque()` 建队、`append(x)` 入队尾、`popleft()` 出队头；
3. BFS 的剪枝天然安全：**第一次到达 = 最少步数**；
4. 带"步数上限"的题，DFS 的 visited 剪枝不安全（第一次到达不一定是最近），**这类题用 BFS**。

### DFS vs BFS 选择

```text
问"能不能到 / 有几条路"   → DFS
问"最少几步 / 最短路径"   → BFS
```

### 方向数组（上下左右）

```python
dr = [-1, 0, 1, 0]    # 行变化：上、右、下、左（顺时针）
dc = [0, 1, 0, -1]    # 列变化

for d in range(4):
    nr, nc = r + dr[d], c + dc[d]
    if 1 <= nr <= n and 1 <= nc <= m:
        # 安全的下一步
```

### BFS 走迷宫模板（求最短步数）

```python
from collections import deque

dist = [[-1] * (m + 1) for _ in range(n + 1)]   # -1 = 没到过（墙也这样处理）
dist[sr][sc] = 0
q = deque([(sr, sc)])
while q:
    r, c = q.popleft()
    for d in range(4):
        nr, nc = r + dr[d], c + dc[d]
        if 1 <= nr <= n and 1 <= nc <= m and a[nr][nc] != '#' and dist[nr][nc] == -1:
            dist[nr][nc] = dist[r][c] + 1
            q.append((nr, nc))

print(dist[tr][tc])     # 终点的最少步数；-1 表示走不到
```

八方向（跳马）只是把 `dr/dc` 换成 8 组 `(±1,±2)`，其余结构完全相同。

## 8. 完全背包模板（考前必背）

每件物品可以**无限次**拿（如：每天可投喂 i 个苹果）。

```python
# 物品 i（1..m）：重量 = i，价值 = v[i]；容量 n，求恰好装满 n 的最大总价值
dp = [0] * (n + 1)                # dp[j] = 恰好装满容量 j 的最大价值
for j in range(1, n + 1):         # 外层：容量 1 → n
    for i in range(1, min(m, j) + 1):   # 内层：枚举最后拿的物品
        dp[j] = max(dp[j], dp[j - i] + v[i])
print(dp[n])
```

读法：`dp[j-i] + v[i]` = "最后一步拿 i，剩下 j-i 用之前的最优方案"。

- **内层 i 正着扫 = 完全背包**（同一物品可反复拿）；
- 01 背包（每件只有一件）内层要**倒着扫**：`for i in range(min(m, j), 0, -1)`。
- 复杂度 O(n×m)。**看到 n ≥ 10⁴ 的 DP 题，写循环版，别用递归**（Python 递归上限 1000，深度大的记忆化搜索会 RecursionError）。

真题：第41次《机器人饲养指南》（苹果题）n≤10⁴、m≤100，此模板 0.02s。

---

# 七、复杂度思维

这是 CSP 很重要的能力。

## 1. 看数据范围再决定算法

例如：

```text
n <= 20
```

通常可以考虑暴力、DFS、子集枚举等。

```text
n <= 1000
```

`O(n^2)` 有时可以接受。

```text
n <= 10^5
```

通常需要接近 `O(n log n)`、`O(n)` 等。

## 2. 暴力枚举

核心：

> 把所有可能情况试一遍。

例如：

```python
for i in range(n):
    for j in range(m):
        ...
```

复杂度大致为：

```text
O(n*m)
```

## 3. 第40次认证“数字变换”中的复杂度教训

错误思路：

```python
for 每个答案:
    for x in range(512):
        for 每次变换:
            g()
```

如果变成：

```text
n × 512 × m
```

在 `n=5×10^5, m=10^3` 时数量极大，不可行。

正确思路：

```text
x 只有 512 种
↓
预处理所有 x 的最终结果
↓
保存 F(x) -> x
↓
查询直接查表
```

大致变成：

```text
O(512*m + n)
```

核心思想：

> **不是看“暴力”两个字就否定暴力，而是看暴力的对象有多大。**

---

# 八、位运算（重点）

整数本质上可以看成二进制。

例如：

```text
13 = 1101₂
```

## 1. `&` 按位与

规则：只有 `1 & 1 = 1`。

```text
5 = 101
3 = 011
---
  = 001
```

所以：

```python
5 & 3    # 1
```

常用：取某些位。

### 取最低 3 位

```python
x & 7
```

因为：

```text
7 = 111₂
```

## 2. `|` 按位或

规则：有一个 `1` 就是 `1`。

```text
101
011
---
111
```

常用：拼接二进制块。

```python
(a << 6) | (b << 3) | c
```

## 3. `^` 异或

规则：相同为 `0`，不同为 `1`。

```python
5 ^ 3    # 6
```

重要性质：

```python
x ^ x == 0
x ^ 0 == x
x ^ a ^ a == x
```

## 4. `<<` 左移

```python
5 << 1
```

相当于：

```text
101 -> 1010
```

一般可理解为：

```python
x << n ≈ x * 2**n
```

对非负整数可直接按这个规律理解。

## 5. `>>` 右移

```python
20 >> 2
```

```text
10100 -> 00101
```

结果：`5`。

非负整数可理解为：

```python
x >> n = x // (2**n)
```

---

# 九、位运算常用技巧

## 1. 取最低 n 位

```python
x & ((1 << n) - 1)
```

例如最低 3 位：

```python
x & 7
```

因为：

```text
(1<<3)-1 = 7 = 111₂
```

## 2. 取“从第 p 位开始”的 n 位

```python
(x >> p) & ((1 << n) - 1)
```

例如取中间 3 位：

```python
(x >> 3) & 7
```

## 3. 判断奇偶

```python
x & 1
```

结果：

```text
0 -> 偶数
1 -> 奇数
```

和 `x % 2` 等价。

## 4. 去掉最低位的 1

```python
x & (x - 1)
```

## 5. 判断正整数是否为 2 的幂

若 `x > 0`，可以：

```python
x & (x - 1) == 0
```

## 6. 二进制拼接

三个 3 位块 `aaa bbb ccc`：

```python
(a << 6) | (b << 3) | c
```

## 7. 进制转换

### base 进制字符串 → 十进制（内置，直接秒杀）

```python
n = int(s, base)      # int('FF', 16) = 255，int('101', 2) = 5
```

### 十进制 → base 进制（循环取余，逆序输出）

```python
alpha = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
res = []
while n > 0:
    res.append(alpha[n % base])
    n //= base
print(''.join(res[::-1]))    # 逆序；n=0 要特判输出 '0'
```

---

# 十、贪心

## 1. 核心理解

贪心 = 每一步都选择“当前看起来最好”的方案。

例如：

```text
当前单位收益最高
↓
先选它
↓
继续
```

但：

> **局部最优不一定推出整体最优。**

所以看到 `收益/成本` 时，不能条件反射地认为“一定应该排序贪心”，要看题目是否具有交换性质。

## 2. 机器人项目管理题中的经验

任务分：

### 灵活型

可以给 `0~a` 之间任意实数杯咖啡。

单位咖啡收益：

```text
b / a
```

### 普通型

只能给：

```text
0 杯 或 a 杯
```

它更像 0/1 选择。

因此单纯把所有任务按 `b/a` 排序后贪心，会被普通型的“要么全拿、要么不拿”限制卡住。

例：

```text
m = 10

普通 A：a=6,b=12，收益率=2
普通 B：a=5,b=9，收益率=1.8
普通 C：a=5,b=9，收益率=1.8
```

按 `b/a`，A 在前。

贪心选 A：花 6，收益 12，剩 4，B/C 都无法选。

但最优是 B+C：花 10，收益 18。

这说明：

> 普通型之间不能只靠 `b/a` 的局部贪心。

---

# 十一、前缀和 / 差分（考前模板）

## 1. 一维前缀和

```python
pre = [0] * (n + 1)
for i in range(1, n + 1):
    pre[i] = pre[i - 1] + a[i]
```

区间 `[l,r]` 求和：

```python
pre[r] - pre[l - 1]
```

## 2. 差分

需要把区间 `[l,r]` 全部加 `v` 时：

```python
diff[l] += v
diff[r + 1] -= v
```

最后再做一次前缀和恢复每个位置。

> 只有在”所有操作先做完、最后统一询问”的场景下，差分尤其好用；如果要在线查询，就要进一步考虑数据结构。

## 3. 二维前缀和（矩阵区域求和）

```python
# a: 原始矩阵（建议 1-based 存）；s: 前缀和，s[i][j] = (1,1) 到 (i,j) 的矩形和
s = [[0] * (m + 1) for _ in range(n + 1)]
for i in range(1, n + 1):
    for j in range(1, m + 1):
        s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j]
```

任意矩形 (x1,y1) 到 (x2,y2) 求和：

```python
def get_sum(x1, y1, x2, y2):
    return s[x2][y2] - s[x1-1][y2] - s[x2][y1-1] + s[x1-1][y1-1]
```

口诀：**大矩形 - 上方 - 左边 + 左上角**（补回多减的部分）。求和可能超出 int 范围时（Python 无碍，但值可能很大）。

---

# 十二、二分查找模板：找最大可行值

当一个数 `x` 满足单调性：

- `x` 可行，那么比 `x` 小的都可行；
- `x` 不可行，那么比 `x` 大的都不可行。

要找“最大的可行值”：

```python
left, right = 1, 10**9

while left < right:
    mid = (left + right + 1) // 2

    if check(mid):
        left = mid
    else:
        right = mid - 1

print(left)
```

为什么：

- `check(mid) == True`：答案至少是 `mid`，所以 `left = mid`。
- `False`：答案一定小于 `mid`，所以 `right = mid - 1`。
- `+1` 是为了取“上中点”，避免 `left + 1 == right` 时卡死。

记忆：

> **找最大可行值 = 上取中点 + 真留左边 + 假 `right=mid-1`。**

机器人宿管指南中：`model(x)` 就是 `check(x)`。

---

# 十三、整数除法向上取整

对于非负整数 `M`、正整数 `N`：

```python
(M + N - 1) // N
```

表示：

```text
ceil(M/N)
```

记忆：

> **向上取整 = `(被除数 + 除数 - 1) // 除数`**

例如：

```python
(10 + 3 - 1) // 3    # 4
```

---

# 十四、浮点数 / 四舍五入注意

CSP 中不要随便使用：

```python
round()
```

之前的考场经验：需要按照题目要求自己控制精度；如果题目要求绝对误差，小数输出通常直接使用足够多位，例如：

```python
print(f'{ans:.6f}')
```

---

# 十五、真题总结：第40次 CSP 第一题《集合》

题目核心：

```text
f(S) = 所有 a[i] 的按位异或
```

判断标准：

```text
真实集合是否相等
        和
小 C 根据异或认为是否相等
```

两者一致输出 `correct`，否则 `wrong`。

因为题目保证每个集合内部按严格递增顺序输入，所以：

```python
S == T
```

就可以直接判断两个集合是否相等。

你最终写出的核心代码：

```python
def f(S):
    i = 1
    result = a[S[i]]
    i += 1
    while i <= S[0]:
        result = result ^ a[S[i]]
        i += 1
    return result


def method(s, t):
    return f(s) == f(t)
```

判断：

```python
if (S[i] == T[i]) == method(S[i], T[i]):
    print('correct')
else:
    print('wrong')
```

这是正确的 100 分级别思路。

---

# 十六、真题总结：第41次 CSP《机器人项目管理》

## 1. 题目模型

任务属性：

```text
o, t, a, b
```

- `o=0`：灵活型
- `o=1`：普通型

## 2. 灵活型

可以给 `0~a` 之间任意实数杯咖啡。

单位咖啡收益：

```text
b/a
```

## 3. 普通型

只能：

```text
0 杯
```

或者：

```text
a 杯
```

获得 `b` 的收益。

## 4. 原先 80 分贪心的问题

当时思路：

```text
结构体
+ b/a 作为效益
+ 排序
+ 从前往后分咖啡
```

方向有价值，但普通型之间可能需要组合选择，单纯按 `b/a` 的局部贪心不能保证最优。

## 5. 正确方向

把普通型的决策看成：

```text
选 / 不选
```

可以用记忆化搜索形式的 DP：

```python
from functools import lru_cache

@lru_cache(None)
def f(i, m):
    if m == 0 or i == n:
        return 0

    if tasks[i][0] == 0:
        use = min(m, tasks[i][2])
        return use * tasks[i][3] / tasks[i][2] + f(i + 1, m - use)

    ans = f(i + 1, m)

    if m >= tasks[i][2]:
        ans = max(
            ans,
            tasks[i][3] + f(i + 1, m - tasks[i][2])
        )

    return ans
```

输出：

```python
print(f'{sum_t - f(0, m):.6f}')
```

你当时复现时出现了一个实际考场坑：

```python
print(f'{sum_t - sub:.1f}')
```

只保留 1 位小数，样例能过，但隐藏数据可能误差超过 `0.001`。

改成：

```python
print(f'{sum_t - sub:.6f}')
```

之后通过。

---

# 十七、真题总结：第41次 CSP《异或》

题目定义：

```text
f(0)=0
f(n)=n ⊕k f(n-1)
```

你最初写：

```python
@lru_cache(None)
def f(n):
    if n == 0:
        return 0
    return xor(n, f(n - 1), k)
```

最终超时。

## 为什么 `lru_cache` 也救不了？

因为第一次计算一个巨大 `f(n)` 时，依然可能要递归：

```text
f(n)
→ f(n-1)
→ f(n-2)
→ ...
→ f(0)
```

`lru_cache` 只能解决重复状态，不能把一次巨大的链条自动变短。

## k 进制不进位异或

每一位独立：

```text
(ai + bi) % k
```

例如最低位只看模 `k` 的结果。

## 提示中的重要方向

题目规定 `k` 为奇数，并特别设置：

```text
ai 和 v 都是 k 的倍数
```

这提示需要研究 `f(n)` 的数学性质，而不是继续暴力递归。

一个重要性质：若 `a=kx, b=ky`，则：

```text
(kx) ⊕k (ky) = k * (x ⊕k y)
```

也就是可以把两边共同的 `k` 提出去。

这说明 k 进制结构可以按位/按层分析。

> 注意：之前曾误认为“每连续 k 个数做 ⊕k 都是 0”，这个结论是错误的。比如 `k=3`：
>
> ```text
> 1 = 01₃
> 2 = 02₃
> 3 = 10₃
> ```
>
> ```text
> 01 ⊕ 02 ⊕ 10 = 11₃ = 4
> ```
>
> 因为最低位抵消了，但更高位并没有消失。

当前结论：

> 研究 `f(n)` 必须同时考虑各个 k 进制位，不能只看最低位。

---

# 十八、真题总结：第40次 CSP《数字变换》

## 1. 值域特别小

题目中每次 `F` 的输入：

```text
0 <= x < 2^9
```

所以一共只有：

```text
512
```

种可能。

这是非常明显的暴力/预处理入口。

## 2. 不应该对每个答案重新暴力

错误方向：

```text
对每个 ai
    枚举 512 个 x
    模拟 m 次 F
```

复杂度：

```text
O(n * 512 * m)
```

当 `n=5×10^5, m=1000` 时完全不可行。

## 3. 正确优化方向：预处理

因为只有 512 个初始状态：

```text
x = 0...511
↓
计算最终 F(x)
↓
保存 result -> x
↓
每个 ai 直接查询
```

大致复杂度：

```text
O(512*m + n)
```

## 4. 九位二进制拆分

一个 9 位二进制数：

```text
aaa bbb ccc
```

拆出：

```python
a = x >> 6
b = (x >> 3) & 7
c = x & 7
```

拼回：

```python
x = (a << 6) | (b << 3) | c
```

## 5. 题目中的 `f(x,k)`

```python
def f(x, k):
    return ((x * x + k * k) % 8) ^ k
```

## 6. `g(x,k)` 的代码经验

处理固定 9 位时注意：

```python
while i < 9:
```

而不是 `i < 8`。

把单个 bit 放入列表：

```python
dig.append(x % 2)
```

不要写：

```python
dig += list(x % 2)
```

因为 `x % 2` 是整数，不是可迭代对象。

连续使用 `while i < 3` 时，如果复用 `i`，每段循环结束后要重新：

```python
i = 0
```

否则后面的循环不会执行。

## 7. Python 名称覆盖问题

如果定义：

```python
def f(x, k):
    ...
```

之后又写：

```python
f = x
```

就把函数名 `f` 覆盖成整数了。

再调用：

```python
f(b, k)
```

就会报：

```text
TypeError: 'int' object is not callable
```

所以枚举变量可以叫：

```python
cur = x
```

## 8. `break` 缩进问题

如果想“找到答案后停止枚举”，应写：

```python
for x in range(512):
    cur = x
    ...
    if cur == target:
        print(cur)
        break
```

不要把 `break` 放在 `if` 外面，否则第一轮就退出。

---

# 十九、真题总结：第40次 CSP《进程通信》

这题第一眼非常复杂：

- `new`
- `send`
- `delete`
- 接口编号变化
- 内存连续空闲区间
- 最优适应
- 循环写入队列

本质是大型模拟题。

考场策略：

> 不要被代码量吓住，先看子任务，看自己能拿哪一档。

如果没有足够时间，不必要求整题 AC，可以争取完成某个低难度子任务。

---

# 二十、DFS / DP / 贪心 / 暴力 / 状压：快速区分

| 概念 | 直观理解 |
|---|---|
| 暴力 | 所有可能都试一遍 |
| DFS | 一条路走到底，不行就退回来 |
| DP | 相同子问题只算一次并利用结果 |
| 记忆化搜索 | DFS + 保存已经算过的状态 |
| 贪心 | 每一步选当前看起来最好的 |
| 状压 | 用二进制位表示“选/不选”等状态 |

状态压缩例子：

```text
0101
```

可以表示 4 个任务中：

```text
任务1：选
任务2：不选
任务3：选
任务4：不选
```

---

# 二十一、考场算法判断流程

看到题目先不要急着写代码，按下面顺序问自己：

```text
① 数据范围多大？
        ↓
② 暴力是否够？
        ↓
③ 如果不够，有没有明显规律？
        ↓
④ 是排序/贪心？
   还是 DFS/DP？
   还是前缀和/差分？
        ↓
⑤ 是否存在重复子问题？
        ↓
⑥ 是否可以预处理？
```

几个非常实用的条件反射：

```text
值域 <= 几百/几千
→ 先想暴力 / 预处理

每个元素只有选/不选
→ 先想 DFS / DP

每一步都有“当前收益最高”
→ 想贪心，但必须检查是否真的成立

区间求和、重复查询
→ 想前缀和

区间统一加值
→ 想差分

存在“可行/不可行”的单调性
→ 想二分答案

整数的位结构很重要
→ 想位运算

枚举"答案"太慢（答案值域巨大）
→ 反过来枚举"位置/对象"
→ 每个对象直接算出哪些答案可行（通常是一个连续区间）
```

## 枚举答案 → 枚举位置（真题：第39次《水印检查》）

要输出所有可行阈值 k，k 有 0~L-1 共 L 种（L 最多 65536）。

**错误方向**：枚举 k，每个 k 重建整张二值图再扫描 → O(L × n²)，L=65536 必超时。

**正确方向**：枚举水印出现的位置 (i,j)，固定位置后直接算出哪些 k 可行：

- 图案白格需要 `A ≥ k` → `k ≤ 所有白格的最小值`
- 图案黑格需要 `A < k` → `k ≥ 所有黑格的最大值 + 1`

所以每个位置对应的合法 k 是**一个连续区间**：

```text
lo = max(黑格值) + 1
hi = min(白格值)
```

若 `lo ≤ hi`，则 k 在 `[lo, hi]` 内时该位置能检出。

所有位置的区间求并集（用 `set` 去重），排序输出。

复杂度 `O(n² × 45)`，与 L 无关。

**套路总结**：

```text
答案值域巨大（k 有 65536 种）
        ↓
枚举答案必超时
        ↓
枚举对象（位置/元素），每个对象算出"哪些答案对它可行"
        ↓
通常是一个区间 / 一个集合
        ↓
合并去重输出
```

---

# 二十二、常见 Python 易错点清单

## 1. `sort()` 不返回排序后的列表

```python
a.sort()            # 对
a = a.sort()        # 错
```

## 2. `^` 是异或，不是乘方

```python
x ^ y               # 异或
```

Python 乘方是：

```python
x ** y
```

## 3. `x ^= y` 会修改 x

```python
x ^= y
```

等价于：

```python
x = x ^ y
```

## 4. 多个循环复用 `i`

每个循环都要确认 `i` 的初值。

## 5. `break` 的缩进

只在应该停止的位置缩进到对应代码块。

## 6. 函数名不要被变量覆盖

```python
def f(...):
    ...

# 不要再写
f = 3
```

## 7. `@lru_cache(None)` 不是万能加速器

它只解决重复状态。

## 8. 浮点输出精度

题目要求 `<0.001` 时，不要只输出一位小数：

```python
print(f'{ans:.6f}')
```

---

# 二十三、考试策略（当前目标）

你目前的实际目标：

> **前两题尽量稳住；第三、第四题争取低成本抢部分分。**

## 第一题

目标：尽量 100 分。

重点：

- 输入输出
- 循环
- 数组/列表
- 字符串
- 简单模拟

## 第二题

目标：尽量 100 分。

重点：

- 排序
- 枚举
- 简单贪心
- 前缀和
- 模拟
- 简单 DP

## 第三、第四题

不要求完整 AC。

优先寻找：

```text
能否直接做某个子任务？
能否用暴力过小数据？
能否预处理？
能否利用特殊性质？
```

### 时间止损

第三/第四题如果连续一段时间仍然没有建立模型，不要无限耗时间。

核心原则：

> **不是“我能不能把整道题做出来”，而是“我现在还能拿多少分”。**

---

# 二十四、已掌握的可直接套用模板

## 1. 快速输入

```python
import sys
input = sys.stdin.readline
```

## 2. 列表读取

```python
nums = list(map(int, input().split()))
```

## 3. 多关键字排序

```python
a.sort(key=lambda x: (key1(x), key2(x)), reverse=True)
```

## 4. 记忆化搜索

```python
from functools import lru_cache

@lru_cache(None)
def f(state1, state2):
    if ...:
        return ...
    ...
```

## 5. 最大可行值二分

```python
left, right = L, R
while left < right:
    mid = (left + right + 1) // 2
    if check(mid):
        left = mid
    else:
        right = mid - 1
print(left)
```

## 6. 向上取整

```python
(M + N - 1) // N
```

## 7. 低 n 位

```python
x & ((1 << n) - 1)
```

## 8. 第 p 位开始取 n 位

```python
(x >> p) & ((1 << n) - 1)
```

## 9. 9 位二进制拆分

```python
a = x >> 6
b = (x >> 3) & 7
c = x & 7
```

拼回：

```python
(a << 6) | (b << 3) | c
```

---

# 二十五、最终记忆版

### Python

```text
input().split()
map(int, ...)
list(...)
append()
extend()
sort()
sort(key=..., reverse=True)
```

### 递归 / 搜索

```text
递归 = 函数调用自己
DFS = 一条路走到底再回退
DP = 相同子问题不要重复算
lru_cache = 给递归加“记忆本”
```

### 贪心

```text
每次选当前最优
但必须检查是否能保证整体最优
```

### 复杂度

```text
先看数据范围，再决定算法
```

### 位运算

```text
&  与
|  或
^  异或
<< 左移
>> 右移
```

### 考试

```text
前两题：稳
第三四题：看子任务，低成本抢分
遇到不会：先找特殊条件和小数据范围
```

---

# 二十六、考前最后提醒

1. 不要因为一道题卡住就长时间死磕。
2. 样例能过不代表隐藏数据一定能过，尤其关注：边界、精度、复杂度。
3. 一看到很小的值域，先想“枚举/预处理”。
4. 一看到递归重复状态，想到 `@lru_cache(None)`；但先判断单次计算是否本身过大。
5. 一看到 `b/a`、收益率排序，不要直接认为贪心一定正确。
6. 题目要求误差时，输出足够多位小数。
7. Python 中 `sort()`、`break` 缩进、变量覆盖函数名，都是实际做题时很容易出现的小错误。
8. 考场首先保证前两题，再根据剩余时间决定第三、第四题抢分。
