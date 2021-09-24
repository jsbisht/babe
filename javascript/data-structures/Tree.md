# Introduction to Trees
Applications:
* Storing hierarchical data. Eg. File System
* For quick search, insertion, deletion. Eg. Binary Search Tree
* Trie. Eg. Dictionary
* Network Routing Algorithm

Terminologies:
* Height of tree - Number of edges in the longest path from root to a leaf.
    * Height of an empty tree is `-1`.
    * Height of a tree with 1 node is `0`.

# Trees
* Binary Tree
* Binary Search Tree
* Red-Black Tree

## Binary Tree
* Each node can have at most 2 children

* `Strict Binary Tree` has each node with either 2 or 0 children.

* `Complete Binary Tree` has except the leaf nodes are having 2 nodes and all nodes are as left as possible.
    * Following is a complete binary tree:

    ```
        L1            [2]
                     /   \
        L2        [7]     [5]
                 /   \      
        L3    [3]    [6]    
             /   \         
        L4  [1]  [11]      
    ```

    * Following does not obey properties of complete binary tree:

    ```
        L1           [2]
                    /   \
        L2       [7]     [5]
                /   \      
        L3   [3]    [6]    
                   /   \   
        L4       [1]  [11] 
    ```

    * Maximum number of nodes(`n`) at level `i` is `2^(i)`
    * Minimum Height of complete binary tree will be `floor(log2(n))`.
    * Maximum Height of complete binary tree can be `n - 1`. This can happen if we have a sparse tree as following:

    ```
        L1            [2]
                     /   
        L2        [7]    
                 /       
        L3    [3]        
             /           
        L4 [1]  
    ```

    * Time complexity would be `O(log2(n))`
    * Worst time complexity would be `O(n)`

* `Perfect Binary Tree` has all levels except the leaf nodes are having 2 nodes.
    * Following is a perfect binary tree:

    ```
        L1           [2]
                    /    \
                  /        \
        L2      []           []   
              /    \        /   \      
        L3   []    []      []    []    
             / \  /  \    / \   /  \   
        L4  [] [][]  []  [] [] []  []  
    ```

    * If `h` is the height of perfect binary tree, than maximum number of nodes in a tree will be `2^(h+1) - 1`
    * From the above equation we can deduce that `n` ie number of nodes, height of perfect binary tree will be `log2(n+1) - 1`. Also, it can be deduced using `floor(log2(n))`.

* `Balanced Binary Tree` has difference between height of left and right subtree for every node not more than k(mostly 1).
    * The idea behind this tree is to optimise the running time for operations on a tree. We try to keep the height of tree to the minimum, to allow it to perform better.
    * Absolute value of difference in height of left subtree and right subtree. 
    * Following is a balanced binary tree, as difference in height is 0 or 1:

    ```
        L1               []0
                        /    \
                      /        \
        L2          []0          []1   
                  /    \        /   \      
        L3       []0   []0     []    []0    
                 / \  /  \          /  \   
        L4      [] [][]  []        []  []  
    ```

    * Following is NOT ? a balanced binary tree, as one of the difference in height is 2:

    ```
        L1               []0
                        /    \
                      /        \
        L2          []2          []1   
                  /             /   \      
        L3       []0           []    []0    
                 / \                /  \   
        L4      [] []              []  []  
    ```

We can implement binary tree using:
* dynamically created nodes
* arrays - to get a certain node at index `i`:
    * left child index = `2i + 1`
    * right child index = `2i + 2`

## Binary Search Tree `(BST)`
A binary tree in which for each node, value of all the nodes in left subtree is lesser (or equal) and value of all the nodes in right subtree is greater.

### Why use BST
Cost of performing operations on `Array` or `LinkedList` is:
* `Search` - O(n)
* `Insert` - O(1)
* `Remove` - O(n)

On sorted Array we can `Search` in `O(logn)`.

Cost of performing `Search`, `Insert` or `Remove` in a Balanced BST is `O(logn)`.

### Searching BST

```js
search(node, key) {
    // node is null or key is present at node
    if(node == null || node.key === key) return node;

    // Key is greater than root's key
    if(node.key < key) {
        return search(node.right, key);
    }

    // Key is smaller than node's key
    return search(node.left, key);
}
```

### Insertion BST

```js
insert(node, key) {
    // If tree is empty, return a new node
    if(node === null) return createNode;

    
    // Else recur down the tree
    if(key < node.key) {
        node.left = insert(node.left, key);
    }
    else if(key > node.key) {
        node.right = insert(node.right, key);
    }

    // Return the unchanged node pointer
    return node;
}
```

### Deletion BST
For the node to be deleted there can be following scenarios:
* `Case 1`: No Child
    * Set the parent node's right/left of node to be deleted as null
* `Case 2`: One Child
    * Link the parent to the only child of node being deleted. 
    * Even after this operation, the subtree if any under node being attached, will still be valid.
* `Case 3`: Two Child
    * Find minimum in right subtree OR Find maximum in left subtree
    * Copy the value in node to be deleted
    * Delete duplicate node from the right/left subtree

```js
remove(node, key) {
    // Tree is empty
    if(root == null) return node;

    // If the key to be deleted is smaller than node
    if(key < node.key) {
        node.left = remove(node.left, key);
    }
    // If key to be deleted is greater than node
    else if(key > node.key) {
        node.right = remove(node.right, key);
    }
    // If current node key value is same as key
    else {
        let temp = null;
        // CASE 1: Node itself is leaf node with no child
        if(node.left == null && node.right == null) {
            // Sets on node which called remove()
            return null;
        }
        // CASE 2: Node with only one child
        else if(node.left == null) {
            // Assigns child to node which called remove()
            return node.right;
        }
        else if(node.right == null) {
            // Assigns child to node which called remove()
            return node.left;
        }  
        // CASE 3: Node with two children
        else {
            // Find minimum of right subtree
            const temp = findMinimum(node.right);
            
        }
    }
}
```

## Red-Black Tree
Red-Black Tree is a self balancing Binary Search Tree (BST), where every node follows the following rules:
1. Every node has a color either red or black
2. Root of the tree is always black
3. There are no two adjacent red nodes (A red node cannot have a red parent or red child)
4. Every path from root to a NULL node has same number of black nodes

### Key Insert Operation
If T is a non-empty tree, then we do the following:
1. use the BST insert algorithm to add K to the tree
2. Color the node containing K red
3. Restore red-black tree properties (if necessary)

To restore the violated property, we use 
1. `Recoloring`
2. `Rotation` (Left, Right, Double)

The algorithm has mainly two cases depending upon the color of `Uncle` (Sibling of parent node)
* If `Uncle` is red, we do recoloring
* If `Uncle` is black, we do rotations and/or recoloring

#### `Uncle` is RED
Let `X` be the newly inserted node. If X's `Uncle` is RED, and X's parent is not BLACK.
1. Change color of parent and uncle as BLACK.
2. Change color of `Grand Parent` as RED.
3. Make X's `Grandparent` as the new X. Repeat steps on new X.

#### `Uncle` is BLACK


# Tree Traversal

Reading/Processing the data of a node exactly once in some order in a tree.

## Types

* Depth First Traversal
    * Preorder Traversal
    * Inorder Traversal
    * Postorder Traversal
* Breadth First Traversal (level order)

## Depth First Traversal

Types:

* Preorder Traversal <Root><Left><Right>
* Inorder Traversal <Left><Root><Right>
* Postorder Traversal <Left><Right><Root>

### Time Complexity

For all types of depth first traversal, time complexity is O(n).

### Preorder Traversal

    <Root><Left Subtree><Right Subtree>

```js
printPreorder(node) {
    if (node === null) { return; }

    // first print data of node
    log(node.data);

    // then recursively call self on left subtree
    printPreorder(node.left);

    // then recursively call self on left subtree
    printPreorder(node.right);
}
```

For the following tree:

```
L1        [A]
         /   \
L2     [B]   [C]
       / \
L3  [D]   [E]
         /
L4     [F]
```

Preorder traversal would return:

    A B D E F C

### Inorder Traversal

    <Left Subtree><Root><Right Subtree>

```js
printInorder(node) {
    if (node === null) { return; }

    // then recursively call self on left subtree
    printInorder(node.left);

    // first print data of node
    log(node.data);

    // then recursively call self on left subtree
    printInorder(node.right);
}
```

For the following tree:

```
L1        [A]
         /   \
L2     [B]   [C]
       / \
L3  [D]   [E]
         /
L4     [F]
```

Preorder traversal would return:

    B D E F A C

#### Inorder traversal without recursion

// TODO

### Postorder Traversal

    <Left Subtree><Root><Right Subtree>

```js
printPostorder(node) {
    if (node === null) { return; }

    // then recursively call self on left subtree
    printPostorder(node.left);

    // first print data of node
    log(node.data);

    // then recursively call self on left subtree
    printPostorder(node.right);
}
```

For the following tree:

```
L1        [A]
         /   \
L2     [B]   [C]
       / \
L3  [D]   [E]
         /
L4     [F]
```
Preorder traversal would return:

    D F E B C A

## Breadth First Traversal

Aka Level Order Traversal: Traverse node in layers.
Eg. L1 nodes, L2 nodes, ..., Ln nodes.

```
L1        [A]
         /   \
L2     [B]   [C]
       / \
L3  [D]   [E]
```

```js
/*Given a binary tree, print its nodes in level order using array for implementing queue */
printLevelOrder(root) {
    int front, rear;
    const queue = createQueue(front, rear);
    const temp  = root;

    while(temp) {
        log(temp.data);

        // Enqueue Left Child
        if(temp.left) enqueue(queue, rear, temp.left);

        // Enqueue Right Child
        if(temp.right) enqueue(queue, rear, temp.right);

        // Dequeue node and make it temp node
        temp = dequeue(queue, front);
    }
}
```

### Reverse Level Order Traversal
Algorithm:
* Create an empty queue and empty stack
* Enqueue the root node in the queue
* Loop while queue is not NULL
    * Dequeue a node from queue and make it current root
    * Push the current root node to the stack
    * Enqueue current root node children to queue
* Now print all items of stack by popping them one by one

# Tree Properties
## Leaf Nodes Count
Nodes which have no children are called leaf nodes.

```js
getLeafNodesCount(node) {
    // If tree is empty
    if(!node) return;

    // Count of leaf nodes
    let count = 0;

    // Initialize empty queue
    const q = createQueue();
    q.push(node);
    
    // Using level order traversal
    while(!q.empty()) {
        const temp = q.front();
        q.pop();

        if(temp.left == null && temp.right == null) {
            count++
        }

        if(temp.left != null) {
            q.push(temp.left);
        }
        if(temp.right != null) {
            q.push(temp.right);
        }
    }

    return count;
}
```

For the following tree:

```
       [2]
      /   \
   [7]     [5]
     \      \
     [6]     [9]
    /   \    /
 [1]  [11]  [4]
```

The leaf nodes are [1]  [11]  [4]. We get the count of leaf nodes as 3. 

## Half Nodes Count
Nodes which have only one child are called half nodes.

```js
getHalfNodesCount(node) {
    // If tree is empty
    if(!node) return;

    // Count of half nodes
    let count = 0;

    // Using level order traversal
    const q = createQueue();
    q.push(node);
    
    while(!q.empty()) {
        const temp = q.front();
        q.pop();

        if(!temp.left && temp.right ||
            temp.left && !temp.right) {
            count++
        }

        if(temp.left != null) {
            q.push(temp.left);
        }
        if(temp.right != null) {
            q.push(temp.right);
        }
    }

    return count;
}
```

For the following tree:

```
       [2]
      /   \
   [7]     [5]
     \      \
     [6]     [9]
    /   \    /
 [1]  [11]  [4]
```

The half nodes are [7], [5], [9]. We get the count of half nodes as 3. 

## Full Nodes Count
Nodes which have both a right child and a left child are called full nodes.

```js
getFullNodesCount(node) {
    // If tree is empty
    if(!node) return;

    // Count of full nodes
    let count = 0;

    // Using level order traversal
    const q = createQueue();
    q.push(node);
    
    while(!q.empty()) {
        const temp = q.front();
        q.pop();

        if(temp.left && temp.right) {
            count++
        }

        if(temp.left != null) {
            q.push(temp.left);
        }
        if(temp.right != null) {
            q.push(temp.right);
        }
    }

    return count;
}
```

For the following tree:

```
       [2]
      /   \
   [7]     [5]
     \      \
     [6]     [9]
    /   \    /
 [1]  [11]  [4]
```

The full nodes are [2], [6]. We get the count of half nodes as 2.