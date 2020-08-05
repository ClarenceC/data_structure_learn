## 树

树是计算机科学家中经常用到的数据结构。树是一种非线性的数据结构，以分层的方式来存储数据，树还滓用来存储有序列表。选择树而不是其它数据结构，是因为在二叉树上查找会非常快。添加删除元素也非常快。

树由一组以边连接的节点组成，根节点是最顶层。

![](./images/WX20200801-172145@2x.png)

### 二叉树

二叉树是一种特殊的树，它的子节点个数不超过两个。在二叉树里面一个父节点下面会有两个节点，**左节点**和**右节点**。

#### 二叉查找树
在二叉查找树里面，通常会把较少的值放在左边，大的值放在右边。

#### 实现二叉查找树

```js
// 显示数据
function show()  {
  return this.data
}
// 节点
function Node(data, left, right) {
  this.data = data
  this.left = left
  this.right = right
  this.show = show
}

// 添加节点，插入后会是一个左小右大的二叉顺序树
function insert(data) {
  var n = new Node(data, null, null)
  if (this.root == null) {
    this.root = n
  } else {
    var current = this.root
    var parent
    while(true) {
      parent = current // 当前父节点给 parent
      if (data < current.data) { // 如果比当前节点值小放左边
        current = current.left
        if (current == null)  { // 遍历到当前左节点为空，放入
          parent.left = n
          break
        }
      } else { //  比当前节点值大放右边
        current = current.right
        if (current == null) { // 遍历到 当前右节点为空，放入
          parent.right = n
          break
        }
      }
    }
  }
}

// 中序遍历
function inOrder(node) {
  if (node !== null) {
    inOrder(node.left)
    console.log(node.show() + '')
    inOrder(node.right)
  }
}

function BST() { // 建立二叉查找树模型
  this.root = null  // 表示根节点
  this.insert = insert // 插入节点函数
  this.inOrder = inOrder 
}
```

测试 `inOrder` 中序遍历

```js
var nums = new BST()
nums.insert(23)
nums.insert(45)
nums.insert(16)
nums.insert(37)
nums.insert(3)
nums.insert(99)
nums.insert(22)
console.log('Inorder traversal: ')
inOrder(nums.root) // 3 16 22 23 37 45 99
```

**inOrder** 中序遍历会从最小的左节点开始遍历

先序遍历：
```js
function preOrder(node) {
  if (node !== null) {
    console.log(node.show() + ' ')
    preOrder(node.left)
    preOrder(node.right)
  }
}

var nums = new BST()
nums.insert(23)
nums.insert(45)
nums.insert(16)
nums.insert(37)
nums.insert(3)
nums.insert(99)
nums.insert(22)
console.log('Inorder traversal: ')
preOrder(nums.root) // 23 16 3 22 45 37 99
```

先序遍历, 会先从根节点遍历后再跟下最小值节点节排列。

后序遍历：

```js
function postOrder(node) {
  if (node !== null) {
    postOrder(node.left)
    postOrder(node.right)
    console.log(node.show() + ' ')
  }
}
var nums = new BST()
nums.insert(23)
nums.insert(45)
nums.insert(16)
nums.insert(37)
nums.insert(3)
nums.insert(99)
nums.insert(22)
console.log('Inorder traversal: ')
postOrder(nums.root) // 3 22 16 37 99 45 23
```

后序遍历，从孙节点先把最小值和最大值遍历后，再遍历根节点。

下面是操作视意图：
![](./images/WX20200802-124102@2x.png)

### 二叉树上进行查找

查找最小值
```js
function getMin() {
  var current = this.root
  while(current.left !== null) {
    current = current.left // 获取树节点最左孙节点的值
  }
  return current.data
}
```

查找最大值
```js
function getMax() {
  var current = this.root
  while(current.right !== null) {
    current = current.right // 获取树节点最右子孙节点的值
  }
  return current.data
}
```

查找给定的值

```js
function find(data) {
  var current = this.root
  while(current !== null) {
    if (current.data == data) {
      return current
    } else if (data < current.data){
      current = current.left
    } else {
      current = current.right
    }
  }
  return null
}
```

### 二叉树上删除节点

```js
function remove(data) {
  root = removeNode(this.root, data)
}

function removeNode(node, data) {
  if (node == null) {
    return null
  }
  if (data == node.data) {
    if (node.left == null && node.right == null) {
      return null
    }
    if (node.left == null) {
      return node.right
    }
    if (node.right == null) {
      return node.left
    }
    var getSmallest = function(node) {
      if (node.left == null && node.right == null) {
        return node
      }
      if (node.left !== null) {
        return node.left
      }
      if (node.right !== null) {
        return getSmallest(node.right)
      }
    }
    var tempNode = getSmallest(node.right)
    node.data = tempNode.data
    node.right = removeNode(node.right, tempNode.data)
  } else if (data < node.data) {
    node.left = removeNode(node.left, data)
    return node
  } else {
    node.right = removeNode(node.right, data)
    return node
  }
}
```

### 二叉树计数的实现

```js
// 创建节点
function Node(data, left, right) {
  this.data = data
  this.count = 1 // count 用来统计
  this.left = left
  this.right = right
  this.show = show
}

// 更新统计数
function update(data) {
  var grade = this.find(data)
  grade.count++
  return grade
}

function prArray(arr) {
  console.log(arr[0].toString() + ' ')
  for (var i = 1; i < arr.length; ++i) {
    console.log(arr[i].toString() + ' ')
    if (i % 10 == 0) {
      console.log('\n')
    }
  }
}

function genArray(length) {
  var arr = []
  for (var i = 0; i < length; ++i) {
    arr[i] = Math.floor(Math.random() * 101)
  }
  return arr
}
```

