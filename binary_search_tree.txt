## Binary Search Trees

Key Operations
1. #find
2. #insert
3. #delete
-> Data set and the order that it is entered into the DS should not (drastically) affect the way these operations work or how fast they perform

##Building a BST
Each BST originates with a single node called the root.  When another node is added to the tree, parent-child relationships will be developed.

1. Each node can have up to two children.  Each child is a node and is the root of its own subtree.
2. The values of each node in the left subtree must be less than the value of the root.
3. The values of each node in the right subtree must be greater than the value of the root.

Steps to #insert(value, root)
1. Compare the value to be inserted to the root to decide what side of the tree value should be inserted into.
2. If value is > root, check for the root's right child.  If the right child exists, compare its value against the value to be inserted. (perform #insert(value, root.right_child))  Otherwise, make value into the root's right child.
3. If value < root, check for the roots left child.  If the left child doesn't exist, set the root's left child to be equal to the value.  If the root's left child does exist, compare the value to be inserted against this node.  (#insert(value, root.left_child))

## Key Operation 2: #find
In order to successfully find a value in the binary search tree, we will implement an algorithm similar to #insert.  At each step, we will be moving towards the place in the BST that the node we are looking for would be if it were in fact a part of the tree.  

Steps to #find(val)
1. Check to see if the root.nil?. If true, we will return false
2. Compare root to val.  Return true if they're equal
3. If unequal, we will check val against the root to see if we should check to the left or the right.  If val is < root, we will check to see if the left child is the node we're looking for. (root.left_subtree.find(val)).  If val is > root, we will check to see if the right child is the node we're looking for. (root.right_subtree.find(val))

##Key Operation 3: #delete
Delete is a bit more complicated than the other two functions in that we'll need to account for the fact that if we delete a node, the children and parent of that node will have pointers that are no longer valid.  Additionally, we'll need to find a replacement for the node.  If the replacement has children of its own, we'll have to account for this as well.  To implement delete, we first need to find the node.  To do this, we'll be checking every node against the value we're looking to delete.  If the root node exists and is not equal to the node we're looking for, we'll compare the values to see if the node we're looking for would be located in the right or left subtree.  If the value we're looking for is less than the root node, we'll search the left subtree.  root.left_subtree.find(val)  If the value we're looking for is more than the root node, we'll search the right subtree.  root.right_subtree.find(val)  If the root is not equal to the value we're looking for and does not have any children, we'll return false.  If the val is located, we'll need to find a replacement. To find the correct replacement for the node we're looking for (the node that we'll be deleting has children), we can do one of two things.  According to the Hibbard deletion algorithm, the replacement should satisfy criteria.

1. The replacement for the root of any tree should be greater than all of the nodes in its left subtree
2. The replacement for the root of any tree should be less than all of the nodes in its right subtree.
3. We must be able to easily resolve the fates of the children of the replacement

In order to find the right node as the replacement, we can either find the largest element in the left subtree, or the smallest element in the right subtree.  This node can be found by recursively searching the right child of the left subtree until the right child's child is nil.  Conversely, it can be found by recursively searching the left child of the right subtree until the left child's child is nil.  If the node to be deleted only has one child, we can simply promote that child.

We will create a function that will return the max node in the subtree that we pass in as an argument

## Time Complexity
All operations #find, #insert, #max, #min, run in O(depth) time, since we are traversing at most depth levels of the tree.

The kth level of the tree contains (2^depth - 1).  Consecutive powers of 2 add such that the total number of nodes at any level of the tree would be (2^depth) - 1.  Since the time complexity of these operations are O(depth), solving for depth, we would be left with log2n = depth and the time complexity would be O(log n) if our BST is full or close to full.  

If the BST is degenerate, all of the operations would take place in linear time.  One of the downsides of Hibbard deletion is that if we continue to insert and delete elements, we'll eventually end up with a tree that has depth = sqrt(n).  In order to ensure that we'll get an average of O(log n) time complexity, we'll have to use a Balanced Tree.

## Balanced Trees
We want the depth of our tree to be log2n -> n being the number of elements in our balanced tree.

A Binary tree is balanced if
1. The depths of bst.right_subtree and bst.left_subtree differ by at most 1
2. Both bst.right_subtree and bst.left_subtree are balanced BSTs as well

Steps
1. Find the depths of full_bst.left_subtree and full_bst.right_subtree
2. Determine if full_bst.left_subtree is balanced
3. Compare the depths of the left and right subtrees of full_bst.left_subtree
4. Determine if full_bst.left_subtree.left_subtree is balanced
5. Determine if full_bst.left_subtree.right_subtree is balanced
6. Compare the depths of the left and right subtrees of full_bst.right_subtree

## Self-Balancing Trees
Our BST will have a method rebalance built onto its prototype, and this method will be called after every insertion and deletion

## Retrieving the Data
It's not uncommon to want to retrieve all of the elements from a BST in sorted order; this is called in-order traversal of the tree.  We should traverse the entire left subtree first followed by the entire right subtree.

1. Perform an in-order traversal of the left subtree.
2. Record the value of the root.
3. Perform an in-order traversal of the right subtree.
