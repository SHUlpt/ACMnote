# deque

## 常用方法

|         操作         |                     效果                     |
| :------------------: | :------------------------------------------: |
|      `c.size()`      |               返回当前元素数量               |
| `c.resize(num,elem)` | 重新设置大小，如果`size()`增大，用elem作为值 |
|     `c.empty()`      |                     判空                     |
| `c.insert(pos,elem)` |            在pos位置插入元素elem             |
|    `c.erase(pos)`    |             移除pos位置上的元素              |
| `c.push_back(elem)`  |                 尾部添加元素                 |
|    `c.pop_back()`    |               移除最后一个元素               |
| `c.push_front(elem)` |                 头部添加元素                 |
|   `c.pop_front()`    |                 移除头部元素                 |
|     `c.front()`      |        返回第一个元素，不检查是否存在        |
|      `c.back()`      |       返回最后一个元素，不检查是否存在       |
|     `c.clear()`      |                   容器清空                   |

