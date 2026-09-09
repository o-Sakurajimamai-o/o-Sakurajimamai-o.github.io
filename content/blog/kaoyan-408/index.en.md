---
title: "408 Exam Notes: Data Structures, Computer Organization, OS, and Networks"
date: 2026-09-09
lastmod: 2026-09-09
description: "Complete revision notes for data structures, computer organization, operating-system scenarios, and computer networks."
translationKey: kaoyan-408
draft: false
---

This edition preserves every note, example, derivation, revision reminder, and figure from the latest backup, in its original order. Headings and formatting have been organized for reading. Corrections and assumptions are provided in footnotes beside the original statements.

<!-- source-content:cs408:start -->
<!-- source: cs408:L1-L33 -->

* Data Structures
  1. Linked lists
  2. Disjoint-set union (code, optimization operations, and visualization of root merging)
  3. Implementations of Prim's and Dijkstra's algorithms without priority queues
  4. Simulation of orthogonal lists and adjacency multilists (be able to draw them)
  5. Heap implementation code, and code that uses a heap to implement a priority queue
  6. Red-black trees (definitions and conclusions, insertion and deletion)
  7. Loser trees and replacement selection (loser tree + sequence numbers), the role of loser trees in merging, and the depth of a loser tree
* Computer Organization
  1. Special floating-point values and flag generation
  2. The principles and circuits of multiplication and division, the number of shifts in Booth multiplication and division, and rounding
  3. The procedure-call process, stack frames, and their respective responsibilities
  4. Multiprocessors and modern processors
  5. A comprehensive summary of the single-cycle datapath in Yuan's textbook
* Operating Systems
  1. Interprocess communication methods
  2. Simplifying resource-allocation graphs and determining the conditions for deadlock
  3. Virtual machines and the detailed SPOOLing process
  4. File-system mounting
  5. Bitmaps and the grouped linked-list method
  6. Multiprocessor scheduling
  7. Wear leveling
* Computer Networks
  1. The roles of, and differences between, the layers of OSI and TCP/IP
  2. Representations of analog signals at the physical layer, such as Manchester encoding[^csa-25]
  3. PPP and P2P protocols
  4. Virtual circuits
  5. OSPF, BGP, and DHCP messages and their specific addresses
  6. The IP multicast process and IGMP
  7. Hamming-code checking and error correction
  8. Reservation-time calculations and NAV-value calculations in 802.11
  9. Mobile IP

<!-- source: cs408:L43-L46 -->

## Data Structures {#cs408-h-01}

> Things to remember: the various implementation procedures for external sorting; the three types of threaded binary trees; the applicability, purposes, and drawing methods of orthogonal lists and adjacency multilists; be able to write out the complete manual calculation processes for Dijkstra's algorithm and the critical path; insertion and deletion in balanced trees, red-black trees, and B-trees; loser-tree sequence numbers and their use in replacement-selection sorting

<!-- source: cs408:L47-L54 -->

### Stacks and Queues {#cs408-h-02}

* In a queue or stack implemented with a linked list, insertion only requires modifying the tail pointer (the rear of the queue) or the head pointer (the top of the stack), while deletion requires modifying the head pointer (the front of the queue) or the top of the stack. **However, note that when a queue becomes empty after a deletion, its rear pointer must also be changed and set to NULL.**[^csa-48]
* The logic of a **prefix expression** is simply the reverse of a postfix expression: a postfix expression is converted from left to right, and a prefix expression is exactly the opposite. Here is a manual calculation example for ${((1 + 2) * 3) - (4 + 2)}$: work from left to right and from the inside outward, moving the operators to the front. ${(1+2)}$ becomes ${(+12)*3}$; then handle the right-hand side, ${(4+2) \Rightarrow (+42)}$. Thus, ${((+12)*3)-(+42)\Rightarrow (*+123)-(+42)\Rightarrow -*+123+42}$. For a computer program, the steps are as follows:
  - **Reverse** the infix expression.
  - Replace every **left parenthesis `(` with a right parenthesis `)`**, and every **right parenthesis `)` with a left parenthesis `(`**.
  - Apply the **infix-to-postfix** algorithm to this reversed expression (using a stack to hold operators temporarily).[^csa-52]
  - **Reverse** the final result **again** to obtain the prefix expression.
  For both prefix and postfix expressions, the idea for conversion to infix is the same: use a **stack to hold operands**, and pop them in sequence when an operator is encountered. Simulate a **postfix expression from left to right**, or a **prefix expression from right to left**.

<!-- source: cs408:L55-L57 -->

### Tree Structures {#cs408-h-03}

#### Tree Structure {#cs408-h-04}

* Let the maximum degree in a tree be ${max_d}$. The total number of nodes is then ${\displaystyle n=\sum_{i=0}^{max_d}n_i=\sum_{i=1}^{max_d}i\times n_i+1}$. **Note that the two lower summation indices differ.**

<!-- source: cs408:L58-L75 -->

#### Binary Trees {#cs408-h-05}

* Among a binary tree's traversal sequences, the inorder traversal of a binary search tree is an increasing sequence; that is, its inorder traversal is unique. For the number of preorder/inorder/postorder traversal sequences of binary trees: for unlabeled binary trees (considering only tree shape), the number is ${\frac{1}{n+1}C_{2n}^{n}}$. For labeled binary trees (trees with the same shape but different labels count as different trees), the number is ${C_n\times n!}$. A preorder or postorder sequence together with an inorder sequence uniquely determines a binary tree.[^csa-59]
* When a tree with ${n}$ nodes is converted into a binary tree, its ${w=n-1}$ edges are divided into left pointers and right pointers. A left pointer points to a child, and a right pointer points to a sibling, giving the following properties:
  * **Number of left pointers:** each **nonleaf node** retains one left pointer to its first child, so the number of left pointers equals the number of nonleaf nodes.
  * **Number of right pointers:** the remaining edges represent sibling relationships, that is, right pointers.
  * When a tree with ${n}$ nodes is converted into a binary tree, the corresponding distinct binary-tree shapes are the distinct shapes of binary trees with ${n-1}$ nodes, so they can be counted using the Catalan numbers. This is because the root has no siblings (that is, this is not a forest), so the root has only a left subtree; therefore, subtract one when calculating the height.[^csa-63]
* When a forest is converted into a binary tree, if the forest has ${n}$ nonterminal nodes, the corresponding binary tree should have ${n+1}$ nodes without a right child. **Proof:** among ${x}$ root nodes, the rightmost root certainly has no right child. For each of the ${n}$ nonterminal nodes, similarly to the roots, its children include a rightmost child without a sibling node, giving a total of ${n+1}$.
* An inorder traversal of a binary search tree produces an ascending sequence (not a preorder traversal).
* A binary search tree is searched by comparing nodes one at a time. **Count the nodes visited along the path, rather than the tree's height or the path length.**
* The path length of a tree is defined as **the sum of the lengths from the root to all nodes**. To minimize the tree's path length, every node moves upward as far as possible, which happens to satisfy the definition of a **complete binary tree**. Therefore, a complete binary tree has the minimum path length.
* In a binary tree, B-tree, and so on, a node's predecessor should be the rightmost node in its left subtree. **However, the node may have no left subtree**; in that case, return to the parent to look for the predecessor or successor.
* Threaded binary trees
  * In a binary tree stored as a binary linked structure, the number of null pointers is ${n+1}$. For ${n}$ nodes, there should be ${2n}$ pointers, and every node except the root is pointed to by its parent, leaving ${2n-(n-1)=n+1}$ null pointers. **These null pointers are used as threads**, with each null pointer pointing to the node's predecessor or successor. However, there must be two that have no predecessor or successor, so there are **${2}$ null threads and ${n+1-2=n-1}$ nonnull threads**.[^csa-70]
  * A threaded binary tree is stored using a binary linked structure. Building one requires a traversal first: during the traversal, if a node has no right child, that pointer field points to its predecessor; if it has no left child, the pointer points to its successor.[^csa-71]
  * For preorder-, inorder-, and postorder-threaded binary trees, finding predecessors and successors is straightforward: simulate the process according to the specific description in the question. However, **a binary linked structure cannot meet the requirements for finding a successor in a postorder-threaded binary tree**. One situation is that the node's parent has a right subtree. If the node has no right subtree, its pointer field should point to its successor, but the successor should be the leftmost node of the parent's right subtree. Therefore, **a ternary linked structure is required to find the successor in this situation**. A ternary linked structure stores left-child, right-child, and parent pointer fields.[^csa-72]
* Binary-search decision trees
  * When calculating the successful search length of binary search, convert the given sequence's positions into the corresponding binary-search decision tree. Each node's height is then the number of comparisons for a successful search.[^csa-74]
  * Binary-search decision trees have a consistent shape: if a binary search goes as far left as possible, the left subtree takes priority; otherwise, the right subtree takes priority. The two cases cannot coexist.

<!-- source: cs408:L76-L80 -->

#### Balanced Binary Trees (AVL, B-Trees, B+ Trees, Red-Black Trees)[^csa-76] {#cs408-h-06}

##### AVL Trees {#cs408-h-07}

* In a balanced binary tree, the balance factor is **the height difference between the left and right subtrees**. Thus, the minimum number of nodes that can form a balanced binary tree is obtained when every nonleaf node has a balance factor of 1. In terms of height, the formula is ${C_n = C_{n-1}+C_{n-2}+1}$, where ${C_0=0,\,C_1=1,\,C_2=2}$. The maximum number of nodes is that of a perfect binary tree. <mark>Question: the maximum depth of a balanced binary tree with 16 nodes</mark>: since ${C_5=12 \lt  16 \lt  C_6=20}$, it is 5.
* Deleting a node from a balanced tree and then adding it again may produce the original tree because the tree can rebalance itself. However, a binary search tree has no balance requirement, so if the deleted node is not a leaf, the resulting tree must differ from the original tree.
* Given any sequence, to determine the number of different balanced binary trees, **choose nodes around the middle as the root**, because the sizes of the left and right subtrees need to be balanced. Then discuss these root choices separately. Once a root is fixed, recursively determine its left and right subtrees in the same way. Consider this example: `The number of balanced binary trees that can be determined by the sequence {1, 4, 5, 10, 16, 17, 21}`. First, the self-balancing requirement gives the candidate root set `{5, 10, 16}`. By symmetry, `5` and `16` are symmetric. Consider `5`: its left subtree is fixed as `1, 4`, with two possibilities; the right subtree has a maximum height of 3 and candidate roots `16, 17`, with four possibilities, giving ${2\times 4=8}$ possibilities in total. Next consider root `10`: both subtrees have a size of 3. Because a balanced tree of height 2 has at least 2 nodes and one of height 3 has at least 4 nodes, each subtree has only one possibility. Thus, the total is ${8+8+1=17}$.

<!-- source: cs408:L81-L87 -->

##### B-Trees and B+ Trees {#cs408-h-08}

* Insertion and deletion in B-trees
  * Insertion: a B-tree of order ${m}$ has at least ${(m + 1)/2 - 1}$ keys and at most ${m-1}$ keys. Therefore, when a node's keys are already full, split at ${(m+1)/2}$ (that is, ${m/2}$ rounded upward). **The middle key moves up one level to become the root key**, and the two children are the portions to the left and right of that key. Subsequent insertion goes into a child. If the child fills up again, promote the middle key in the same way. If the level above is also full, continue recursively upward.[^csa-83]
  * Deletion: **be sure to note that deletion replaces the current node with a leaf node (terminal node). That is, if the node being deleted is not a leaf, find its predecessor or successor, replace it, and then delete after replacement. Thus, the actual deletion is essentially from a leaf.** See the Wangdao textbook for details. There are three cases: direct deletion, borrowing from a left or right sibling with enough keys, and merging a sibling with the parent.
  * Here is an additional example in which the siblings do not have enough keys to lend:

    <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20250913190339.png" alt="Original note figure 1" width="1440" height="467" loading="lazy" decoding="async"><figcaption>Original note figure 1</figcaption></figure>

    Since 71 is a leaf node, delete it directly, leaving an empty node. Then inspect the left sibling (55) to see whether it has enough keys to lend. It does not, so merge the parent and the left sibling into a leaf node. The parent is now empty, so inspect its left sibling (20) to see whether it has enough keys to lend. It does not, so merge the parent and the left sibling. The resulting root is `20,47`, with three children: `[18],[23],[55, 60]`.
* A fixed-length code set converts data into binary strings of a fixed length, so the binary tree it forms is fixed, and every data item is at a leaf node.

<!-- source: cs408:L88-L92 -->

#### Heaps {#cs408-h-09}

* The number of comparisons during heap insertion and deletion
  * During insertion, insert directly at the rightmost leaf position and then adjust the heap: **starting from this node, compare it with its parent in sequence and swap if it is larger. Since the original heap property remains, no other comparisons are needed.**
  * During deletion, first delete the root, and then let the rightmost leaf replace it as the root. Adjust the heap as follows: **starting from the root, first compare the values of its two children, select the extreme value, and compare the root with it. Continue until the root no longer swaps downward.**
  * If an initial sequence must be adjusted into a heap, work backward starting from the last **nonleaf node**.

<!-- source: cs408:L93-L103 -->

#### Loser Trees (Tournament Trees) {#cs408-h-10}

* Loser trees (tournament trees)
  * Loser trees are suitable for multiway merging, so first use an external-selection algorithm to choose the initial merge runs, and then construct the loser tree. Suppose internal sorting of the initial file produces four initial merge runs: `{6,8}; {3,9}; {1, 5}; {2, 7}`. The loser-tree construction process is as follows.
  * <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251107194753.png" alt="Original note figure 2" width="1073" height="599" loading="lazy" decoding="async"><figcaption>Original note figure 2</figcaption></figure>
  * A loser tree is a complete binary tree. Its root points to the smallest number, and its nodes store merge-run numbers.
  * As the figure shows, a ${k}$-way loser tree has depth ${\lceil log_2k \rceil+1}$, but its height is ${\lceil log_2k \rceil}$ (**excluding the champion node and leaf nodes**), independent of the number of nodes.
  * In one pass, a loser tree selects one smallest merge run, using a number of comparisons equal to the tree height. However, the total number of comparisons depends only on the number of initial merge runs and is independent of ${k}$. Therefore, when memory permits, increasing ${k}$ reduces the loser-tree height, reduces I/O operations, and speeds up external sorting.[^csa-99]
  * A loser tree can optimize the generation of initial merge runs, that is, improve replacement-selection sorting by selecting MINMAX more quickly.
    1. The records in the in-memory workspace serve as the loser tree's external nodes, while the parent of the loser tree's root points to the record with the smallest key in the workspace.
    2. To make it easier to select the MINIMAX record, attach a merge-run sequence number to each record. When comparing keys, compare run numbers first: the record with the smaller run number wins; if the run numbers match, the record with the smaller key wins.
    3. Construction of the loser tree can begin by setting the run numbers of all workspace records to 0. Then, as w records are read from FI into the workspace one by one, adjust the loser tree from top to bottom. Since these records have run number “1,” they all lose to records with run number 0, thereby filling the loser tree's nodes one by one.

       <figure class="fig"><img src="/blog/kaoyan-408/figures/note-53aaae82c15d078c2f3c14ee7500752.png" alt="Original note figure 3" width="962" height="858" loading="lazy" decoding="async"><figcaption>Original note figure 3</figcaption></figure>

<!-- source: cs408:L104-L107 -->

#### (Multiway) Huffman Trees and Code Sets {#cs408-h-11}

* To determine the number of empty nodes that must be added in a multiway Huffman tree or multiway merge, consider the following algorithm. Consider an optimal merge tree with ${n}$ nodes and a fan-in of ${k}$. Suppose a strict ${k}$-ary tree has ${n_0}$ leaf nodes and ${n_k}$ nodes of degree ${k}$. Then ${n=n_0+n_k=k\times n_k + 1}$. Eliminating ${n}$ gives ${n_0=(k-1)n_k+1\Rightarrow n_k=(n_0-1)/(k-1)}$. Let ${(n_0-1)\mod(k-1)=x}$. If ${x=0}$, a ${k}$-ary merge can be constructed; otherwise, there are ${x}$ extra nodes. We know that ${x\lt k}$, so let these ${x}$ nodes form a new merge run. However, this requires one additional node to be the combined node for ${\sum x}$. Therefore, only **${k-x-1}$ new nodes need to be added**, with one node lying internally and having degree ${k}$.
* For multiway balanced merge sorting of ${\displaystyle n}$ records, the number of merge passes depends on the chosen merge fan-in. Here, one record is one merge run. If the given number of merge passes is ${\displaystyle k}$, the selected fan-in must satisfy ${\displaystyle k- 1\lt \lceil log_{d}(n) \rceil \leq k}$. For example, <mark>if ${\displaystyle 149}$ records undergo only 4 passes of multiway balanced merging, the valid fan-ins are ${\displaystyle 4,\,5}$, whereas ${\displaystyle \lceil log_{6}(149) \rceil =3}$</mark>.

<!-- source: cs408:L108-L112 -->

### Graph Structures {#cs408-h-12}

* In an undirected graph, each node's degree is its number of incident edges, so the total degree of the graph is twice the number of edges. In a directed graph, each node's degree is the sum of its outdegree and indegree.
* An adjacency matrix is suitable for dense graphs, frequent queries of “does an edge exist between any two vertices?”, and adding or removing edges. An adjacency list is suitable for sparse graphs and frequently traversing neighbors or the entire graph. Many algorithms use adjacency lists, such as DFS, BFS, Prim, Kruskal, and topological sort.
* In an adjacency matrix A, ${A^n[i][j]}$ represents the **number of paths** from vertex ${i}$ to vertex ${j}$ with length ${n}$.[^csa-111]
* Prim's algorithm has time complexity ${O(n^2)}$, independent of the number of edges, and is suitable for **dense graphs**. Kruskal's algorithm has time complexity ${O(m\times log_2m)}$, independent of the number of vertices, and is suitable for **graphs with sparse edges and many vertices**.

<!-- source: cs408:L113-L117 -->

### Searching {#cs408-h-13}

#### Hashing {#cs408-h-14}

* The average unsuccessful-search length for a hash table depends on its hash function. For example, if the array length is 10 and the hash function uses modulus 7, calculate the unsuccessful-search lengths for indices 0–6 only; do not include the later positions.
* When calculating unsuccessful hash-search lengths, with separate chaining, search only the pointers that have values; the next null pointer is not counted as a comparison. With probing, if the current position is the last occupied position and the next position is empty, the next position is also counted as a search.
* When calculating the load factor for separate chaining in a hash table, the count of nodes loaded into the hash table must include all the pointers. That is, if the linked list corresponding to 2 has size 2, both nodes count toward the load factor's numerator.

<!-- source: cs408:L118-L127 -->

### Strings and Miscellaneous Topics {#cs408-h-15}

* When merging two sorted sequences, the worst-case number of comparisons is O(m + n - 1).
* When calculating the KMP algorithm using my method, array indices start at 1, so position 0 is the beginning (that is, nothing is there; move the entire segment forward). When the corresponding next-array value is 0, move the entire segment forward and then perform `i+1`, that is, `j=next[j], i++, j++`. **Note:** when constructing the next array, the head and tail must not lie within the same substring. For `aaaab`, the next value of b should be 4, that is, a|aaa = 3 + 1.
* In a multiway linked structure, the links correspond to different pointer fields. For example, a binary or ternary linked structure has three or two pointer fields, respectively.[^csa-121]
* Sparse matrices
  * Compressing a sparse matrix uses triples and necessarily loses random-access capability.
  * Transposing a sparse matrix `A[m][n]` stored as triples requires at least three steps: (1) swap the matrix's row and column values; (2) swap i and j in every triple; (3) reorder the triples to complete the transpose, because the triples are arranged primarily in row order.
* In quicksort, if a pass is defined as processing all elements once, the following cases determine whether the current sequence could be the result of the second pass:
  * If the pivot processed in the first pass is at the beginning or end (that is, the maximum or minimum), this pass fixes one final position. In the second pass, only one side can be chosen, fixing one more final position. There are then two final positions.
  * If the pivot is not at the beginning or end, three final positions are fixed.

<!-- source: cs408:L128-L130 -->

## Computer Organization {#cs408-h-16}

> Things to remember
> The various special cases of IEEE754 floating-point numbers; the conceptual part of ISA; parallel-processing techniques and multicore-processing concepts; the ranges of the various internal computer number representations; biased representation and the concept of machine zero in floating-point numbers; specific floating-point operations, exponent alignment, rounding, and so on; **the basic concepts of multiprocessors**; the scope of ISA

<!-- source: cs408:L131-L146 -->

### Computer Architecture {#cs408-h-17}

* CPI is the number of clock cycles required by one instruction. It reflects how many cycles the instruction needs to run on the hardware (because registers and so on can only be used once in each clock cycle), so CPI is independent of the clock period.
* When calculating the size of an entire program segment, the question generally gives the complete code segment. Calculate from its first line, the starting address ${A}$, and its last line, the ending address ${B}$. **Note here** that the ending address of the last line is not the starting address of the last instruction, **but the starting address of that instruction + the instruction length**. If the last instruction is ${x}$ addressable units long, then ${Lenth=B+x-A\,\,\text{or}\,\,B+x-1-A+1}$.
* General-purpose registers store operands and various addresses. **Their width is determined by the machine word length; word length/machine word length is the width of the datapath.**
* According to the principles of the von Neumann computer architecture:
  - Instructions and data are stored in the same memory and have the same format; they are **not physically separated**.
  - The CPU fetches instructions from memory at the address specified by the program counter (PC).
  - Data is accessed through the address fields in instructions.
  - Instructions and data are distinguished mainly because **the CPU accesses memory in different phases of the instruction cycle**: the instruction-fetch phase accesses instructions, while the execution phase accesses data.
* Differences and relationships among word length, machine word length, instruction length, and memory word length
  * Word length: the number of binary bits of data that the CPU data bus can handle simultaneously, that is, **the width of the data bus (or MDR)**.[^csa-141]
  * Machine word length: the width of the datapath (data bus, MDR).
  * Instruction length: **it must be an integer multiple of the memory word length**. If the instruction length is twice the memory word length, fetching one instruction requires 2 memory-access cycles. If the instruction length equals the memory word length, the instruction-fetch cycle = the machine cycle.[^csa-143]
  * Memory word length: the number of bits that one memory location can store. The machine word length is generally equal to, or an integer multiple of, the memory word length, which usually equals the width of the **MDR**. **The instruction length is specified as an integer multiple of the memory word length.**
  * A main-memory address is not specified to be any integer multiple; it is the width of the MAR (maximum addressable space).
* Calculate CPI strictly according to its definition: the number of instructions completed in a certain number of clock cycles, that is, the number of clock cycles required for one instruction.[^csa-146]

<!-- source: cs408:L147-L164 -->

### Data Representation and Computation {#cs408-h-18}

* With the same number of bits, biased representation and two's-complement representation have the same representable range.
* When calculating the extreme values of floating-point numbers, pay particular attention to those special cases, such as an all-1 exponent or an all-0 exponent. This part **must be memorized**.
* The purposes and meanings of flags in the PSW
  * CF is the carry/borrow flag. It is meaningful only for unsigned numbers and is used to determine overflow and related issues. It indicates carry in addition and borrow in subtraction, giving the combined formula ${CF=Sub\oplus C_{out}}$. Thus, when subtracting two numbers, ${A-B}$, if ${A \gt  B}$, then ${CF = 1}$. When adding two numbers without overflow, ${C_{out}=0}$, which is the same as ${CF}$. In other words, in addition, ${CF}$ and ${C_{out}}$ have the same value; subtraction is the opposite. If overflow occurs, that means ${C_{out}=1}$, and the formula gives ${CF=0}$.[^csa-151]
  * OF is the overflow flag and is meaningful only for signed numbers. ${OF=C_n \oplus C_{n-1}}$: a signed-number operation overflows if the carries associated with the sign bit and the highest numerical bit differ.
  * SF is the sign bit, the most significant bit of the number, and is meaningful only for signed numbers.
  * ZF is the zero flag and is meaningful for both.

* For unsigned numbers, SF and OF have no meaning and can be ruled out directly. In unsigned subtraction, if the minuend is greater than the subtrahend, there is no carry: ZF = 0, CF = 0. Therefore, no overflow should mean ${\overline{ZF+CF}=1}$.[^csa-156]
* For the evaluation order of casts, their precedence is higher than multiplication and division. Thus, `(float)(x+y)/2` is evaluated as `x+y -> (float)(x+y) -> /2`. When computing with variables of different types, follow type promotion: a calculation involving `float` and `int` produces a `float` result, while a calculation involving signed and unsigned numbers produces an unsigned result. For an unsigned conversion such as `short -> unsigned int`, **the extension is formally a sign extension**. Conversely, `unsigned short -> int` uses unsigned extension. **This can be summarized as follows: when extending a shorter word to a longer word, the shorter type determines the extension method.** For a cast, only the representation is changed (that is, the definition of signed versus unsigned numbers).[^csa-157]

  ```c
  unsigned int u = 0xffff0fff;
  short s = -3940;
  int i = 50000;
  short temp = u + s;
  int result = temp + i; printf(" %d\n", result);
  ```

  ${\displaystyle result=50155}$

<!-- source: cs408:L165-L180 -->

* Hamming-code checking and error correction
  * Suppose a Hamming code has ${k}$ parity bits and ${n}$ data bits. Then ${k}$ must satisfy ${2^k \geq n+k+1}$. The sender must then convert the data into Hamming-code format. Using `1001011` as an example, ${k=4}$ parity bits are needed, for 11 transmitted bits in total.
  * Set the Hamming parity-bit positions, conventionally at ${2^i}$, that is, ${1, \,2,\,4,\,8}$. Then calculate the values ${a_i}$ to store in these four positions: take the XOR of all non-parity positions ${j}$ whose binary index has a 1 at bit ${i}$. For example, when calculating ${a_1}$, the positions in ${3,5,6,7,9,10,11}$ whose first binary bit (the ${2^0}$ bit) is 1 are ${3, 5,7,9,11}$. Hence ${a_1=a_3 \oplus a_5 \oplus a_7\oplus a_9\oplus a_{11}=1}$. Similarly, ${a_2=a_3\oplus a_6\oplus a_7\oplus a_{10}=0}$. Continuing gives ${a_4=1}$ and ${a_8=0}$, so the transmitted data is `10110010011`.[^csa-167]
  * The receiver corrects errors using a method similar to setting the Hamming parity bits: calculate the four corresponding values ${h_i}$. Each ${h_i}$ is calculated in the same way as the parity bit, but additionally XORed with ${a_i}$. For example, if the receiver obtains `10110110011`, then ${h_1=a_3 \oplus a_5 \oplus a_7\oplus a_9\oplus a_{11}\oplus a_1=0}$ and ${h_2=a_3\oplus a_6\oplus a_7\oplus a_{10}\oplus a_2=1}$. Similarly, ${h_4=1,\,h_8=0}$. This gives 0110, indicating an error in the sixth bit; invert that bit. If the result is 0000, it proves there is no error.[^csa-168]
* The concept of machine zero: **it exists only in floating-point numbers and represents underflow**. It is not a true zero; it means that the number is smaller than the smallest representable value. As long as the mantissa is 0, the number is considered machine zero regardless of the exponent. Therefore, **only when the exponent is represented in biased form** can all 0s represent machine zero: biased representation shifts the negative range into the positive range, so an all-zero biased exponent corresponds to the smallest negative number.[^csa-169]
* The numerical range of IEEE 754 floating-point numbers
  * Normalized numbers (the exponent is neither all 0s nor all 1s): the smallest normalized number is ${1.0\times 2^{1-bias}}$, and the largest normalized number is ${(2-2^{-p})\times 2^{(2^{k-1}-2)}}$, where ${p}$ is the number of significant mantissa bits and ${k}$ is the number of exponent bits.[^csa-171]
  * Subnormal numbers (the exponent is all 0s but the mantissa is nonzero): the smallest positive subnormal number is ${1.0\times 2^{1-bias+p}}$ (corresponding to mantissa 0.00..01, with ${p}$ significant mantissa bits, 23 or 55).[^csa-172]
* In **floating-point rounding**, the other three methods are relatively simple. The important method is rounding to nearest: retain a certain number of bits for floating-point rounding, and round these retained bits to the nearest value. That is, check whether the bit closest to the least significant retained bit is 1. If it is, round by adding 1; otherwise, truncate.[^csa-173]
* An operation on two normalized floating-point numbers can produce a subnormal number. For example, if ${\displaystyle A=0x00800001,\,B=0x80800000}$, then ${\displaystyle A+B=0x00000001}$.
* Adders, multipliers, and dividers
  * **For subtraction of either signed or unsigned numbers**, in `x-y`, feed x unchanged into one side of the adder, bitwise invert y for the other side, and set sub to 1.
  * Whether the ALU adds the partial product and the multiplicand is determined by the multiplier's least significant bit (0: do nothing; 1: add).
  * When shifting during sign-magnitude multiplication, perform a **logical right shift** on the carry bit, partial product, and product portion together.
  * A fast array multiplier **does not contain** a shifter.
  * ${\displaystyle n}$-bit **multiplication and division** require approximately ${\displaystyle n}$ addition/subtraction operations and ${\displaystyle n}$ shift operations. Remember the single XOR operation on the sign bits.

<!-- source: cs408:L181-L202 -->

### Memory Systems {#cs408-h-19}

#### Memory Classification and Functions {#cs408-h-20}

* The classifications and purposes of various memories
  * Random-access memory (RAM, volatile); read-only memory (ROM, also supports random access, and writing is slower than reading); sequential-access memory (magnetic tape, supports only sequential access); direct-access memory (magnetic disks and optical discs, whose access process includes both random access, to locate a track, and sequential access, to locate a sector).
  * Static memory (SRAM, higher power consumption, nondestructive readout, no refresh required); dynamic memory (DRAM, lower power consumption, destructive readout, regeneration required after reading, only for the cells read, and periodic data refresh by row to prevent data loss).
* Main memory is implemented using RAM and ROM; control memory (in microprogramming) is implemented using ROM.
* **The width of the memory address register MAR depends on the size of the computer's addressable space**. That is, consider both the size of the computer's main-memory address space and its addressing unit. For example, if a 16-bit computer has an address space of 128KB, its MAR width is 128KB/2B = ${2^{16}}$B, that is, 16 bits.[^csa-188]
* The alignment rule for boundary-aligned storage is to place a value at a byte address that is an integer multiple of the size of its type. A ${short}$ must be stored at a multiple of 2, and an ${int}$ at a multiple of 4. If the intended location does not meet this requirement, leave the corresponding number of bytes empty. **Note:** C uses boundary alignment for variable types by default, and floating-point values use IEEE754 format by default.[^csa-189]
* A main-memory address consists of a main-memory block number + an offset within the block. The block number maps to the cache's ${tag}$ + cache-set-number fields, and the offset is the same.
* The memory cycle is **the minimum waiting time from the start of one access until the memory bank can begin the next independent access**. It does not include data-transfer time.[^csa-191]
* DRAM refresh
  * Every DRAM refresh reads the row to be refreshed into the DRAM's built-in row-buffer register (implemented using SRAM), so the register's size should equal the number of bits in one row.
  * DRAM refresh does not depend on external refresh and is transparent to the CPU (regeneration is different).[^csa-194]
  * DRAM refresh needs only a row address and does not require a chip-select signal to specify the chip.
  * **The number of refreshes is the same for multiple DRAM chips as for one DRAM chip**, because each DRAM's refreshes are independent and they therefore refresh in parallel. The time spent corresponds to the refresh count of one DRAM, that is, its number of rows.
  * Here is a comprehensive example: <mark>There are 16 DRAM chips, each 1M X 4 bits. Every pair of chips is combined by width expansion into a 1M X 8-bit memory bank. These banks use low-order interleaving to form an 8MB memory. The memory is byte-addressable and connected to a 64-bit memory bus. Main memory can read or write at most 64 bits at a time, and its memory cycle is 1ns.</mark>:
    1. Each DRAM chip is 1M x 4 bits, with 1024 rows and 1024 columns. Given a row number, all the corresponding row's data is read into the chip's row-buffer register. The number of entries in the row buffer equals the number of columns, and each entry is 4 bits wide. Thus, one chip's row-buffer register has size ${\displaystyle 1024 \times 4bit}$, and the total row-buffer size for the memory is ${\displaystyle 1024*4*16/8=8192B}$. Row-buffer registers are generally implemented using SRAM.
    2. The memory is byte-addressable, meaning that each byte corresponds to one memory location. One chip supplies 4 bits of data, so selecting one memory location means selecting one location in a memory bank. Selecting a location in a bank simultaneously selects the 4-bit locations at the same address in two DRAM chips, which output together onto 8 data lines of the memory bus. Width expansion does not change the number of memory locations; it increases the size of each location. The number of addresses stays the same, while the data width corresponding to each address increases. Depth expansion does not change the size of a location; it increases the number of locations and the number of address indices, while the amount of data corresponding to each address remains unchanged.
    3. DRAM chips use multiplexed address pins. There are 10 address pins and 4 data pins, giving ${\displaystyle 10 +4 = 14}$ pins.
    4. A double variable x has main-memory address `01 2345H`. The bus is 64 bits wide. With simultaneous activation, 8 banks are activated in one memory cycle, each supplying 1B of data. Because low-order interleaving is used, the main-memory address consists of a 20bit within-block address + a 3bit block number. Thus, the first 20 address bits of these 8B are the same, and the final three address bits range from 000 to 111, representing the 8 banks. The low three bits of x's address are 101. The first simultaneous activation obtains the 8B from `01 2340H` through `01 2347H`, of which `01 2345H` through `01 2347H` are x's first three bytes. The second memory cycle obtains the 8B from `01 2348H` through `01 234FH`, and 01 2348H through 01 234CH are x's final five bytes. Reading x therefore requires two memory cycles.
    5. Refresh is carried out by hardware inside the chips. During refresh, the same row of every chip in the entire memory is refreshed simultaneously, so only one chip's refresh needs to be considered. One refresh requires one memory cycle (1ns). Both concentrated and asynchronous refresh require 1024ns to refresh all rows. The CPU cannot access memory during refresh, so the dead time is 1024ns (the difference is whether the dead time is continuous). All rows must be refreshed once in every refresh period, so the proportion of dead time is ${\displaystyle (1024/20480)*100 =5}$.[^csa-202]

<!-- source: cs408:L203-L218 -->

* Calculating the numbers of pins and chips for SRAM and DRAM
  * In a specification such as `8MB x 8` bits, 8MB is the address space. Thus, the address-line count is ${log_{2}(8MB=2^{23})=23}$ bits, while 8 bits gives the number of data lines. A total of ${23+8=31}$ pins is required. For DRAM, divide the address lines equally between rows and columns and transmit the two parts separately. This needs only ${11.5}$, that is, 11 row bits and 12 column bits (for the optimal allocation, use fewer rows), giving a total of ${12+8=20}$ pins.[^csa-204]
  * If the required number of RAM chips is asked for, **first observe the computer's addressing unit**, then calculate the required total address-space size and the size of one RAM chip, divide the former by the latter, and round upward.
  * Here is an example of expansion in both dimensions: <mark>"Several 8K x 8-bit chips form a 32K x 32-bit memory. The memory word length is 32 bits, and memory is word-addressable. Find the highest address in the chip containing address 41F0H."</mark> First perform width expansion: 4 chips form an 8K x 32-bit unit, connected directly to the data lines without chip selection. Then perform depth expansion: clearly, 4 groups of the width-expanded chips are required, so use the high 2 bits for chip selection. Each group represents ${8K=2^{13}}$, that is, a 13-bit address space. Thus, **bits ${1 \sim 13}$ represent the address space, and bits ${14 \sim 15}$ select the chip**. Since ${41F0H=010|0...}$, the corresponding highest address is ${010|1FFFH =5FFFH}$.
* One bus transaction includes:
  * Sending the starting address and command
  * Memory preparing/reading data; the preparation stage takes one memory cycle
  * Placing the data on the data bus for transfer
  * **Note: for a multimodule interleaved memory, after the address has been sent, the first bank takes one full memory cycle. All subsequent banks then prepare in a pipelined fashion.** Thus, the time required for an ${n}$-bit multimodule interleaved memory is (assuming that sending the address and command takes 1 clock cycle, and ${T_{\text{bus}}}$ is the bus clock period): starting address and command (1 clock cycle) + memory time (the first bank's data-preparation time) + ${n\times T_{\text{bus}}}$ (transferring one bit takes one bus cycle).[^csa-211]

  <figure class="fig"><img src="/blog/kaoyan-408/figures/interleaved-memory-timing.png" alt="Original note figure 4" width="1007" height="400" loading="lazy" decoding="async"><figcaption>Original note figure 4</figcaption></figure>

* Low-order interleaving in multimodule memory has two activation methods: staggered activation and simultaneous activation.
  * Multibank parallel memory has multiple modules, each with its own memory bank, MAR, MDR, and read/write/control circuits. Each module is an independent memory.
  * **When the number of bits transferred by each module equals the data-bus width**, staggered activation is used. Its storage process was described in the earlier discussion of a bus transaction. Let the memory cycle be ${T}$ and the bus cycle be ${r}$. The number of modules must be at least ${m=T/r}$. One datum can be read or written every ${1/m}$ memory cycle, and conflicts may occur during this process.
  * When **the sum of all module widths equals the data-bus width**, simultaneous activation is used: the modules send data to the bus together, exactly filling its width. Some transferred bits may be unused. For example, suppose a computer is built from 4 interleaved DRAM chips of ${64KB \times 4}$ bits, and main memory can read or write at most 32 bits at a time. To fetch a double (8B) at main-memory address ....AH, the number of modules is 4, so the low two address bits are 10. Since the data bus is 32 bits wide, the total module width equals the data-bus width, and simultaneous activation is used. The access sequence is: 00, 10, **10, 11; 00, 01, 10, 11; 00, 01**, 10, 11. The portions not marked red are unused bits.[^csa-216]
  * Multibank interleaved memory may encounter bank conflicts during access. For example, with 4-way interleaved memory, the CPU's main-memory address sequence is `03，09，11，02，33，25，0D，04，08，29，43，0A` (all hexadecimal). Ideally, one datum can be read or written every 1/4 cycle; call that time 𝛥𝑡. The addresses in each memory module are determined by the final two binary address bits: module 0: 00, 04, 08, 0C, 10; module 1: 01, 05, 09, 0D, 11, 25, 29; module 2: 02, 06, 0A, 0E, 12; module 3: 03, 07, 0B, 0F, 13, 33. Clearly, if the addresses of four consecutive accesses include addresses in the same module, a memory-access conflict occurs. Therefore, 11 and 09, 25 and 11, 0D and 25, 08 and 04, and so on cause conflicts. 29 and 0D also belong to the same module, with an access interval of less than 4𝛥𝑡. However, the conflict when accessing location 08 delays that access by 3𝛥𝑡, which also delays access to location 29 by 3𝛥𝑡. As a result, its access no longer conflicts with access to location 0D.
  * Because multimodule memory is activated in a pipeline, once steady state is reached, its average transfer speed should be calculated as a pipeline: one bank is transferred per cycle.

<!-- source: cs408:L219-L230 -->

#### Cache {#cs408-h-21}

* For an instruction, inspect carefully how many accesses actually occur. For example, a[x] = a[x] + 1; **a[x] is actually accessed twice here**.
* Unless otherwise specified, write-through counts as one main-memory access even when the write is placed in a write buffer.
* To calculate cache efficiency, first calculate the cache hit rate H, then the average access time ${T_A=H\times T_C +(1-H)\times T_M}$. The factor by which main memory is slower than cache is ${r=\dfrac{T_M}{T_C}}$. Cache efficiency is then ${e=\dfrac{T_C}{T_A}=\dfrac{1}{H+r(1-H)}}$, and the memory-performance improvement factor is ${X = \dfrac{T_M}{T_A}}$.[^csa-222]
* Main memory and cache **each have an independent address space**. Cache addresses map to the main-memory address space. Different cache-mapping methods have different cache-address structures:
  * Direct-mapped cache address: line number + within-block address
  * Fully associative cache address: line number + within-block address
  * Set-associative cache address: set number + line number + within-block address
  * **Note:** although a cache address contains a set number and line number, the cache structure does not store the set number or line number. It contains a valid bit, tag, data (line length), LRU information (if any), and a dirty bit (if any).
  * **Note:** cache capacity means its data-storage capacity, **excluding the tag array (tag bits, valid bits, dirty bits, and so on)**. However, calculations of the total number of cache bits or total cache capacity include the tag array.
  * **Note:** the cache's control fields are the tag array: the valid bit, tag, dirty bit, LRU bits, and so on.
  * In a ${d}$-way set-associative cache, after the main-memory address identifies a specific set, d comparators (associative memory) are needed to select 1 line, and then a multiplexer **selects a block**. The number of multiplexer-selection bits depends on how many main-memory blocks one cache line can store.[^csa-230]

<!-- source: cs408:L231-L238 -->

#### Disks {#cs408-h-22}

* When calculating average disk read/write time, **remember that the initial average time until the target sector rotates under the head is half a revolution** (do not use this calculation if the question explicitly specifies the head's initial position). The disk's data-access rate may not be given explicitly, but it can be derived from the amount of data in one revolution and the time for one revolution.
* Track density is the number of tracks per unit length in the radial direction; bit density is the amount of data stored per unit track length. **As the radius increases, track density remains unchanged and bit density decreases.**
* RAID: redundant array of independent disks
  * RAID0: a disk array with no redundancy and no parity. **Striping gives it the fastest read/write speed among RAID levels.**
  * RAID1: a mirrored disk array. Two disks are used as one, improving safety and reliability.
  * RAID2: uses Hamming-code error correction, improving safety and reliability.
  * RAID3–5: all use parity, improving safety and reliability.

<!-- source: cs408:L239-L253 -->

### Instruction Sets and the CPU {#cs408-h-23}

#### Instruction Sets {#cs408-h-24}

##### Instruction Formats {#cs408-h-25}

* The “1” in PC + “1” is the size of the computer's addressing unit. For example, on a 16-bit computer, one instruction occupies two bytes. Since 2B = 16bit, one instruction makes PC = PC + 1.[^csa-242]
* Ordinary interrupt-related and I/O-related instructions, and instructions that set/modify memory-management registers or the PSW, are **privileged instructions**. Arithmetic and logical instructions, data-transfer instructions (memory transfers, transfers between registers, and so on), and control-transfer instructions (conditional/unconditional jumps, returns/calls, and so on) are nonprivileged.[^csa-243]
* Instruction formats and the corresponding computer encodings
  * Suppose the computer is a 16-bit computer. With fixed-length opcode encoding, there are half-word instructions (8 bits, common for RR instructions), single-word instructions (16-bit instruction length, with the specific format given by the question), and double-word instructions (32 bits, with the second word generally holding a displacement or effective address).
  * Common instruction formats are: **RR** (register–register), **RX and RS** (register–memory instructions). RX has the format `OP R1 X B D`, where X is the index register, B the base register, and D the displacement: **one register operand R1 and one memory operand**, with `EA = D+(X)+(B)`. RS has the format `OP R2 R3 B D`, that is, **two registers and one memory operand**, with `EA = (B) + D`. Other formats are SI (storage–immediate) and SS (storage–storage).
  * Given this analysis, if a question supplies instructions ${(F0F1)_H(3CD2)_H}$ and ${(2856)_H}$, the first is a double-word instruction and the second a single-word instruction. Their operations can therefore be identified using the instruction formats supplied in the question.
  * The opcode portion of a fixed-length instruction word is determined only by the `OP` field; that is, the number of `OP` bits represents all operands. Variable-length instruction words can use opcode expansion to increase the number of operands. For example, a 20-bit instruction with two 8-bit address fields leaves 4 bits for `OP`. With a fixed-length instruction word, there can be at most ${2^4-2=14}$ two-address operands (reserving two patterns for one-address and zero-address instructions). With variable length, only one pattern needs to be reserved for zero-address and one-address opcodes, allowing at most ${2^4-1=15}$.[^csa-248]
  * Because fixed-length instruction words have a fixed length, they **do not require a PC counter**, whereas variable-length instruction words do. Variable-length instruction words can be fetched according to the maximum instruction length and then split into their specific portions. From a processor-design perspective, the fixed-length instruction-word format is better.[^csa-249]
  * Two kinds of instructions may have implicit operands: zero-address instructions obtain their operands from **the top and second-to-top of the stack**; one-address instructions obtain an operand from the **ACC**.
* MIPS assembly language and machine language
  * MIPS is a typical RISC processor: byte-addressable, using 32-bit fixed-length instruction words and fixed-length opcodes. If a given field is 16 bits wide, sign extension or zero extension is required.
  * There are three instruction formats: R-type, I-type, and J-type. A **J-type instruction** has the format `OP(31-26) direct address(25-0)` and is mainly used for unconditional jumps. Its target is calculated by **concatenating the high four bits of the current PC with the 26-bit direct address, and finally shifting right by two bits (appending two 0s)**. The two 0s are appended because one instruction must occupy 4 memory locations (32 bits), so its final two bits are always 0 and need not be supplied in the instruction.[^csa-253]

<!-- source: cs408:L254-L265 -->

##### Instruction Cycles and Addressing Modes {#cs408-h-26}

* One instruction corresponds to one microprogram; that is, both take one instruction cycle.
* In displacement addressing, do not forget the calculation (starting from the address of the current displacement instruction): **PC = PC + “1” + Offset ${\times}$ “1”**. (**Supplement:** this statement is not correct; the specific displacement-addressing mode still matters. For relative addressing, simply add “1” + Offset. The above mostly uses indexed addressing to make array loops convenient, so understand it instead of memorizing it rigidly.)[^csa-256]
* After an instruction's execution cycle ends, the processor checks for an interrupt request. If there is one, it enters the interrupt cycle. In that cycle, the CPU performs preparatory work (saving the return point, disabling interrupts, obtaining the interrupt vector, and so on), and then jumps to the interrupt service routine's entry address. The interrupt service routine (ISR) is then a concrete program (code, but not a process) containing multiple instructions.
* During instruction execution, addressing methods for fetching operands include register addressing, such as `R[R1]`, and direct addressing, such as `M[imm]`, where the operand itself is the memory address.[^csa-258]
* MIPS procedure calls: suppose procedure P calls procedure Q. The steps are:
  1. P places the entry arguments somewhere Q can access.
  2. **P stores the return address in a specific location**, then transfers control to Q.
  3. **Q saves P's context** and allocates space for its own local variables.[^csa-262]
  4. Execute procedure Q.
  5. Q puts the return value somewhere P can access.
  6. Q retrieves the return address and transfers control to P.

<!-- source: cs408:L266-L275 -->

#### The CPU, Datapath, and Control Logic {#cs408-h-27}

* A register-file read port rs uses a **multiplexer** because it is fast, purely combinational, and supports multiple read ports. A register-file write port rd uses an **address decoder**, because rd must be specified by the instruction. **Strict timing control is required, and the address decoder ensures a unique selection to prevent erroneous writes.**
* Single-cycle (multicycle) processors
  * Every instruction completes in one clock cycle, so every instruction has a CPI of 1. The computer's clock period must be set according to the maximum processing time of all instructions.
  * Note that a single-cycle processor does not mean a single-bus processor. A single bus can carry only one signal in a clock cycle, so it is unsuitable for a single-cycle processor.
  * In a single-cycle processor, each computer component can be used only once per clock cycle.
  * In a single-cycle datapath, an instruction completes in one clock cycle, so the PC must be written in that cycle and **does not need a write-enable signal**. Because a multicycle datapath must be combined with pipelining, some pipeline stages do not need to write the PC and others do. Thus, **a multicycle datapath needs a write-enable signal to control when the PC is written**.[^csa-272]
  * In the single-cycle processor above, the PC is not the only element that needs no write enable. No state element is written during instruction execution; otherwise, one instruction cycle could not complete within one clock cycle. Thus, no instruction register (IR) is needed.[^csa-273]
  * Within this clock cycle, the instruction needs only one clock cycle, and its control signals are generated once and do not change again. A multicycle controller, in contrast, must coordinate the various pipeline stages to complete one instruction, so the control-signal values need to change during instruction execution.
* When MAR and MDR read or write memory in an operation sequence, they occupy the **address lines and data lines**, respectively, and do not occupy the internal bus. They can therefore run in parallel with other operation sequences within one clock cycle.

<!-- source: cs408:L276-L290 -->

##### CPU Pipelining {#cs408-h-28}

* To identify an ordinary data dependency (that is, not load-use), simply check whether more than two instructions separate the modification and use of the two related registers, because two pipeline stages must separate write-back and operand fetch.[^csa-277]
* In the MIPS pipeline (IF, ID, EX, MEM, WB), both IF and MEM may access memory and cause a page-fault exception. EX performs **overflow detection** and may cause an overflow exception. External interrupts must wait until an instruction completes, that is, until after WB. ID performs decoding and so on, and may cause exceptions such as an **invalid instruction opcode**.
* In a CPU pipeline, the actions in instruction fetch and decode are consistent and do not require control signals. **Control signals are generated together in the decode stage**, and then travel with the data through the pipeline registers. When they reach their relevant pipeline stages, the corresponding control signals are fed to the appropriate interfaces.
* Pipelining techniques
  * Pipeline performance metrics: throughput = ${TP=\dfrac{n}{T_k}}$, the number of tasks completed per unit time. Speedup is the ratio between the time without pipelining, ${T_s}$, and the time with pipelining, ${T_k}$: ${S=\dfrac{T_s}{T_k}}$. Efficiency is **the ratio of the equipment's actual usage time to the total running time**, that is, pipeline utilization. For a pipeline with equal-length stages, ${e=\dfrac{n\Delta t}{T_k}}$. It can also be calculated by definition: ${e=\dfrac{\text{area occupied by tasks in the space-time diagram}}{\text{total area of the space-time diagram}}}$.
  * For a pipeline whose stages all have the same duration, the space-time diagram is the same as that of a CPU pipeline. The total time required to finish all tasks is ${T_k=k\Delta t + (n-1)\Delta t}$.
  * When the pipeline stages differ in duration, the longest stage is the bottleneck. **The bottleneck stage needs to be replicated**, so that a bottleneck operation can be processed every clock cycle.

    <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20250902195333.png" alt="Original note figure 5" width="832" height="645" loading="lazy" decoding="async"><figcaption>Original note figure 5</figcaption></figure>

* A summary of CPU pipelining
  * A typical RISC CPU pipeline has five stages: IF, ID, EX, MEM, and WB. The possible conflicts include structural hazards (hardware conflicts), data hazards, load-use hazards, and control hazards.
  * For a data hazard, if there is neither half-cycle register reading/writing nor forwarding, ID must wait until the preceding instruction finishes WB. **This requires stalling for three cycles (EX, MEM, WB)**. If only half-cycle register reading/writing is used, ID can execute in the second half of WB, **requiring two stall cycles (EX, MEM)**. With forwarding, the result can be passed directly to the next instruction after EX, resolving the conflict with **no stall**.
  * For load-use, if there is neither half-cycle register reading/writing nor forwarding, execution must wait until WB finishes, **requiring three stall cycles (EX, MEM, WB)**. With only half-cycle reading/writing, **two cycles are needed, as above**. With both half-cycle reading/writing and forwarding, data can be forwarded directly to the ID-EX pipeline register after MEM, so **only one stall cycle (MEM) is required**.
  * For a control hazard, various jump instructions change the PC at different stages. For example, both `beq` and `J`-type instructions generate the target address in the execute stage, allowing the PC to obtain its next address through a multiplexer in MEM under the write-control signal. Thus, the basic reference point is that the next operation after the PC is determined is instruction fetch (IF). Unless otherwise specified, a jump instruction generally determines the final PC **after MEM**, so the next instruction must stall for 3 cycles (ID, EX, MEM). **Note:** a load-use conflict may occur, requiring one more cycle. Because the jump instruction requires that extra cycle, the corresponding next instruction naturally stalls for 3+1 = 4 cycles.
  * Identifying data hazards: remember to check for pipeline conflicts and **whether the current instruction uses the register value produced by a particular instruction**. If t1 modifies R1 and t2 also modifies it, then t3 uses it, the data hazard is between t3 and t2, not t1. **When identifying data hazards, ignore half-cycle register reading/writing and branch forwarding, and analyze the conflicts from the pipeline alone.**
  * Identifying branch hazards: determine only whether the current instruction may cause a branch hazard. These commonly arise with conditional and unconditional jump instructions.

<!-- source: cs408:L291-L299 -->

### Buses and Device I/O {#cs408-h-29}

#### Bus Systems {#cs408-h-30}

* On an asynchronous bus, the two devices may differ greatly in speed and rely entirely on mutually constraining handshake signals, so time must be allocated as needed.
* On an I/O bus, the contents of the data-buffer register and command/status registers all travel over the data lines. The address lines carry the address of the port exchanging data with the CPU. The control lines send read/write signals to I/O ports and are used only to control port reads and writes.
* In asynchronous serial communication, the two clocks are not strictly synchronized when transmitting a character. Therefore, every character transmission (**which may transmit multiple data bits**) must use **start and stop bits to mark the beginning and end**.
* The **maximum transfer rate** in bus bandwidth assumes that every clock cycle transfers data. The **average transfer rate** accounts for actual conditions (time spent transmitting instruction addresses, preparing data, and so on). Thus, the maximum transfer rate is essentially: number of clock cycles ${\times}$ amount of data transferred per clock cycle.[^csa-296]

<!-- source: cs408:L300-L310 -->

#### I/O Systems {#cs408-h-31}

##### Interrupts {#cs408-h-32}

* Interrupt handling and subroutine calls both push information onto a stack to save context, and both save the PC. Interrupt handling saves the PC so that the interrupted program can continue from its return point; a subroutine call saves the PC so that execution can return to the main program after the subroutine finishes. Because interrupts can change at any time, interrupt handling must save the PSW to prevent the previous program's interrupt handler from being interrupted again by another interrupt. A call instruction does not normally modify the PSW automatically, and the function runs in the same processor mode (usually without changing interrupt enables or the privilege level), so saving the entire PSW is unnecessary.[^csa-302]
* The interrupt-response process executes the implicit interrupt instruction, whereas the interrupt-response cycle occurs after that implicit instruction finishes. That is, the interrupt-response process is performed in the interrupt cycle of an instruction cycle, while the interrupt service routine runs during the interrupt-response cycle.[^csa-303]
* Interrupt-priority selection can use software or hardware. **The interrupt vector is the result of hardware priority selection.**[^csa-304]

##### I/O Transfer Methods and Calculations {#cs408-h-33}

* If a device requires a transfer rate of ${x\,B/s}$ and the data-buffer register in its interface has a capacity of ${y\,B}$, the CPU must poll at intervals no longer than ${\dfrac{x}{y}\,s}$; otherwise, data will be overwritten and lost.[^csa-306]
* DMA is controlled entirely by hardware during I/O. After the data transfer finishes, it sends a completion interrupt to the CPU, which executes an interrupt service routine. This step tells the CPU that the DMA controller has completed the transfer and that the CPU can proceed to the next task. During interrupt service, the CPU checks the DMA controller's program status word for data errors. In other words, **the DMA controller checks data for errors and records them in its own status-word register, then reports them together when the transfer ends, leaving the CPU to handle them**.
* A DMA controller contains a word counter indicating the number of blocks in the current DMA transfer. Note that if the counter has ${n}$ bits and is decremented once for each transferred data block, completion at a counter value of 0 means a total of ${2^n-1}$ blocks are transferred; completion when the counter overflows means a total of ${2^n}$ blocks.[^csa-308]
* **The external device issues a DMA request**. After receiving it, the DMA controller requests the bus from the CPU, and the CPU responds by granting it bus control.

<!-- Editorial corrections and clarifications; additions to the complete source translation. -->

<!-- source: cs408:L311-L328 -->

## Operating Systems {#cs408-h-34}

> Items to memorize:
> 	The advantages, disadvantages, and applicability of different operating systems; compare single-job processing and batch processing; the data structures for device allocation and reclamation (the various tables and implementation details); microkernels versus monolithic kernels; virtual file systems; the various forms of interprocess communication; the various memory and disk allocation methods in file systems; circular buffering and buffer-pool methods for disk buffering; the definition, functions, and procedure of memory-mapped files.

### Operating-System Functions and Processes {#cs408-h-35}

#### Operating-System Categories and Functions {#cs408-h-36}

* The operating-system boot process and disk initialization
  * Installing an operating system on a disk:
    * Perform low-level initialization (low-level formatting) of the disk, dividing it into sectors consisting of a header, a data area, and a trailer. Identify a sector using its track number, head number, and sector number.
    * Partition the disk (C: and D: drives). Each partition's starting sector is recorded in the partition table (PBR) of the disk's master boot record.[^csb-319]
    * Perform logical formatting (high-level formatting): store the initial file-system data structures (including **global file information**) on the disk, create the root directory, and initialize the information about free disk blocks.
  * Booting the operating system:
    * Activate the CPU to read the boot program in ROM (the **ROM bootstrap program**) and execute BIOS instructions.
    * Perform the hardware self-test, construct the interrupt vector table at the very beginning of memory, and then use interrupts to check for faults.
    * The BIOS reads the startup program, loads the hard disk containing the operating system, and starts the **disk bootstrap program**. The CPU **loads this boot sector into memory**.
    * Load the master boot record (MBR), use the MBR to find the boot device, and then have the MBR scan the disk's partition table to find and load the active hard-disk partition (the **partition bootstrap program**).[^csb-325]
    * The active partition's first sector is called the partition boot record (PBR). Its purpose is to find and activate the program in the partition's root directory that boots the operating system (that is, to start the operating-system manager).
    * Once found, load the manager and start the operating system, then **load the operating system into memory**.
    * Points to note: ROM stores the operating-system bootstrap program; ROM is responsible for initializing hardware, starting the bootloader, and so on. All running program data, file caches, and similar contents are stored in RAM.

<!-- source: cs408:L329-L343 -->

* The four characteristics of a multiprogram system are concurrency, sharing, virtualization, and asynchrony. **Concurrency and sharing** are the basic characteristics. A single-program system has a closed execution environment, whereas a multiprogram system does not.
* A C program goes through preprocessing ${\to}$ compilation (converting a high-level language to a low-level language, namely assembly language) ${\to}$ assembly (further converting assembly language to machine language) ${\to}$ linking ${\to}$ loading.
  * In this process, linking is **the stage that establishes logical addresses**: it combines different files and then assigns a continuous range of logical addresses. The stage that completes the address transformation is **loading (setting up page tables to establish the mapping between logical addresses ↔ physical memory)**.
  * The loading stage has three different loading methods. **Absolute loading** is suitable only for single-program jobs. The job's exact physical location in memory is already known, so its logical addresses are identical to its actual physical addresses. In **static relocatable loading**, instruction and data addresses also start at 0, but are relative to an initial value, so the job can be loaded into the part of memory beginning at a chosen initial address, in one operation. Both methods above load the job all at once and occupy a contiguous memory region equal to the job's size. **Dynamic relocatable loading** differs from these: its relative addresses are not converted during loading; conversion waits until they are actually used. Thus its address space is not contiguous, and **address conversion occurs only during program execution**. Relocation requires a **relocation register (base register)** for address conversion and a limit-address register for bounds checking. The physical address is the relocation-register value + the relative address.[^csb-332]
* During a system call, a user process enters kernel mode. In this process, part of the user stack's contents (registers, stack pointers, and so on) is stored on the kernel stack for kernel code to use, while the PCB saves only the kernel stack's context (such as system-call parameters, return addresses, and the kernel stack pointer). During the execution of a system call or interrupt service routine, only the interrupt-response phase is atomic; the later part can be scheduled and blocked (consider operations such as nested interrupts).[^csb-333]
* Storage areas for different types of data
  * Code segment or functions: stores executable code.
  * Read-only data segment: stores constants (local **constants** and global **constants**).[^csb-336]
  * Read/write data segment (initialized data segment): stores readable and writable global variables and static variables.[^csb-337]
  * Heap: stores dynamically allocated memory (malloc/new).
  * Stack: function parameters, local **variables** (function pointers), return addresses, and other contents of **stack frames maintained separately by each thread**.
* Analysis of shared and private data among threads
  * Data in the code segment, read-only data segment, read/write data segment, and heap, along with file descriptors and similar resources, belongs to the process. The heap is part of the process's address space and **can be shared by threads**.
  * Data on the stack is **private to each thread**. Each thread has its own thread stack and stack pointer.[^csb-342]
* On the same computer, that is, with the same CPU architecture, different operating systems have **the same system-call instruction**. The same operating system on different computers (CPU architectures) has **different system-call instructions**. The underlying reason is that the CPU architecture determines how the operating system passes parameters, while the instruction is the same, so different operating systems pass parameters in the same way. If the system call is the same but the computers differ, their instructions differ, and the corresponding system-call instructions also differ. **Note: On one computer, different operating systems have different system-call interfaces. Distinguish the interface from the instruction.**[^csb-343]

<!-- source: cs408:L344-L364 -->

#### Processes and Threads {#cs408-h-37}

* General principles for setting process priorities:
  * System processes &gt; user processes.
  * Interactive processes &gt; noninteractive processes.
  * I/O-bound processes &gt; CPU-bound processes (to prevent data from being overwritten; starting I/O earlier can improve the overall efficiency of the system).[^csb-348]
* The CPU clock is the processor's internal reference clock, also called its beat; the commonly mentioned clock cycle refers to this CPU clock. In contrast, **a timer interrupt is issued by the system clock, is an external interrupt, and acts on the operating system**, so the operating system controls the system clock. It is commonly used for scheduling, time slices, and timed interrupts.
* Situations that cause a process to terminate:
  * Normal completion.
  * Abnormal termination: an exception occurs that makes the process unable to continue.
  * External intervention: termination at an external request, such as intervention by an administrator or the operating system, a request from the parent process, termination of the parent process, and so on.
* Comparing the priorities of different process types: interrupt handler &gt; real-time process &gt; interactive process &gt; I/O-bound process &gt; CPU-bound process &gt; background (noninteractive) process; system process &gt; user process.[^csb-354]
* A process cannot be scheduled only under particular kernel constraints, such as primitive operations. Even a process in a critical section can be scheduled/preempted.
* If a newly created high-priority process enters the ready queue, the preemption sequence is as follows: a new process enters the ready queue; the CPU (or operating system) checks its priority; if its priority is higher, stop the current process and perform CPU scheduling. **This process does not require interrupt participation.**[^csb-356]
* Condition variables in a monitor do not necessarily have to obey strict conservation, meaning one wait for every signal. However, a monitor's wait function is always called **when the condition is not satisfied, so it always causes the process to block**. The signal function checks whether any process is in the blocked queue: if there is one, wake it; otherwise, skip the operation.
* Among mutual-exclusion methods, semaphores and monitors apply to both single-threaded and multithreaded (multi-CPU) settings. Because test-and-set is atomic, it also applies to multiple CPUs. Disabling interrupts does not apply to multiple CPUs: other CPUs can still access shared resources, so mutual exclusion cannot be achieved.
* An atomic operation is not equivalent to disabling interrupts and then re-enabling them. An atomic operation directly locks the bus to prevent other CPUs from accessing it.[^csb-359]
* The three factors considered for the CPU round-robin algorithm are the system's response requirements, the number of ready queues, and the system's processing capacity. **The running time of each process cannot be predicted, so it is not considered.**[^csb-360]
* If an interrupt occurs while a process is executing, and it is not an I/O interrupt, meaning the process will not voluntarily give up the CPU, then the CPU enters kernel mode to execute the corresponding interrupt routine. Here, the interrupt handler is only a piece of code: the CPU simply enters kernel mode and runs some code; it has not switched to a new process. Therefore, the process is still in the running state, and process scheduling does not necessarily occur.
* Interprocess communication
  * Pipe: when a pipe is created, two entries are added to the system file table, which two processes use to control file reading and writing. A pipe is unidirectional; simultaneous communication in both directions requires two pipes. Mutual exclusion and synchronization between pipe reads and writes are controlled by the pipe process.[^csb-363]
  *

<!-- source: cs408:L365-L378 -->

#### Multiprocessors and Multiple Threads (Processes) {#cs408-h-38}

* Multiprocessor systems
  * If a multiprocessor system uses ${n}$ processors, its speedup is not ${n}$ because of increased hardware-processing overhead.
  * Multiprocessors are more reliable: when one processor develops a problem, its task can be carried out by another processor.
* Multithreaded (multiprocess) systems
  * Hardware multithreading is a technique for sharing a single processor. Each thread can be viewed as an instruction sequence. A separate register set and program counter must be provided for each thread, so a thread switch only requires changing register sets, reducing overhead.
* For details of process (thread) scheduling methods, see **Tang Xiaodan's Operating Systems**.

### Synchronization and Mutual Exclusion {#cs408-h-39}

* Resource-allocation graphs
  * To simplify a resource-allocation graph, if the resources needed by a process have already been satisfied, the corresponding edge can be removed, allowing resources to be allocated to other processes and thereby simplifying the graph, as follows:

    <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251105195615.png" alt="Original note figure 6" width="858" height="355" loading="lazy" decoding="async"><figcaption>Original note figure 6</figcaption></figure>

    At this point, processes ${P_2,\,P_4}$ have already obtained the resources they want, **and have no edges pointing to other resources**, so these two edges can be eliminated. Then ${R1,\,R2}$ can be allocated to ${P_1,\,P_3}$ to simplify the graph. There is no deadlock in this case.
  * In a resource-allocation graph with only one instance of each resource type, a cycle means that the system is deadlocked.

<!-- source: cs408:L379-L388 -->

### Memory Management and Virtual Memory {#cs408-h-40}

#### Memory Management {#cs408-h-41}

* The **page-table register** stores **the starting physical address of the outermost page table**, meaning the initial address of the first-level page table (**the 408 syllabus calls the outermost page table the first-level page table**). The physical addresses of the subsequent page-table levels are stored in entries in the preceding level. Because each process has its own independent address space, each process has a different outermost page table; therefore, every process switch **always changes the contents of the page-table register**, whereas switching threads does not (threads share memory space).[^csb-381]
  * Distinguish it from the dynamic relocation register. Paging and segmentation (including demand paging/segmentation) also count as dynamic relocation methods, but usually only one of the two is needed when converting a logical address to a physical address. Paging generally uses the page-table register; the relocation register is better suited to contiguous allocation (segmentation).[^csb-382]
* Memory-management methods in which a process's memory space need not be contiguous include paged allocation, segmented allocation (**divide an entire job into several logical segments and place them in memory as needed**), and segmented paging.
* When calculating a cache or TLB hit rate, even if there is only one miss in 1000 accesses, calculate it; do not simply approximate the hit rate as 100%.
* The detailed sequence for an access by a process should be as follows:
  * First look up the TLB. If it does not contain the corresponding page number, access the page table in memory (these accesses can be simultaneous here, depending on the problem's requirements). If the valid bit for the corresponding page number is 0, or no corresponding page frame exists, access secondary storage, then write the entry into the TLB, and then start again by looking up the page table through the TLB. The TLB is consulted twice in this process. In fact, the page-fault interrupt routine completes this work and then returns to the original instruction to execute it again, which is why the TLB is checked first. **Note: This has only obtained the corresponding physical address; memory must still be accessed! Pay particular attention when calculating the time.**
  * Once the corresponding physical address is obtained, check the cache. If the cache misses, main memory must be accessed directly (possibly at the same time), and the corresponding memory unit is then written into the cache.
* Compaction is suitable for dynamic allocation with variable-sized regions, that is, for memory management using dynamically relocated partition allocation.

<!-- source: cs408:L389-L401 -->

#### Virtual Memory {#cs408-h-42}

* The main-memory–secondary-storage mapping (virtual page ↔ physical page frame) is **fully associative**, because the operating system must be able to load any virtual page into any main-memory page frame to ensure flexibility and good memory utilization. The page table records the mappings, and the TLB speeds up their lookup.
* The MMU is the process of translating a virtual address into a physical address. During translation, access permissions, bounds, and other conditions are checked.[^csb-391]
* Demand-paged (or demand-segmented) storage differs from ordinary paged storage as follows: **the former is a virtual-memory technique, resides permanently in secondary storage, and requires page-fault interrupts. In other words, it can also be called "virtual paged memory," so it can logically enlarge the memory space**. Ordinary (or simple) paged (or segmented) storage resides permanently in memory, requires only a page table, and does not experience "thrashing."[^csb-392]
* In a demand-segmented storage system, the physical address in a segment-table entry is a base address. Thus the actual physical address should be calculated as **base address + offset within the segment**.
* Cache is implemented by hardware; virtual memory is implemented jointly by hardware and software. System programmers can see virtual memory, whereas application programmers cannot: it is transparent to them.
  * In more depth, things visible only to system programmers (in fact, most things related to the operating system are visible to system programmers) include: **hardware details**: PSW (program status word register), device registers, the instruction set, I/O channels, DMA controllers, interrupt vector tables, and so on; **kernel data structures and mechanisms**: PCBs (process control blocks), ready queues, blocked queues, page tables, segment tables, page-replacement algorithms, file control blocks (FCBs), and index nodes (inodes); **interrupt handlers and drivers**.[^csb-395]
* To determine the specific division into page-table levels, determine how many pages the entire virtual address space requires and how many page-table entries fit in one page, then use the relationship between them to find the number of levels. For example: "<mark>A 64-bit computer has a 64-bit address bus and a virtual address space of ${2^{48}}$. It uses a virtual-memory system with 4 KB pages and 8 B page-table entries. Find the minimum number of levels in its multilevel page table.</mark>" **Answer:** The virtual address is 48 bits, independently of the size of the physical address space. A total of ${48-12=36}$ bits are needed for the page number. One page holds 8 KB / 8 B = 1 K = 10 bits of page-table entries, so the minimum is ${36/10=3.6\Rightarrow 4}$ page-table levels.[^csb-396]
* Address translation can produce page-fault interrupts, bounds-violation interrupts, and access-permission/protection interrupts (indicating that execution, writing, or user access is forbidden, or that a segment/page access exceeds its permissions).
* Here is an example of calculations for a two-level virtual-memory system:
  * <mark>A computer uses two-level virtual paged storage, has 4 MB of main memory, and uses virtual paged memory management. A process has a 256 MB address space, pages are 1 KB, both the page directory and the page table occupy one page, and all entries are 2 B. Entries contain a presence bit, a physical page-frame number, and access-permission bits. The contiguous k bits starting at the most significant bit are the physical page-frame number, the least significant bit is the presence bit, and all remaining bits are permission bits. The page-table base register PTBR contains 0x000C00H. Given that P1 accesses logical address 0x2000400H, show the procedure for calculating the physical address.</mark> **Answer:** Main memory is 4 MB, requiring 22 bits. A page takes 10 bits, so the physical page-frame number uses 12 bits. The virtual address uses 28 bits, and one page can hold ${512 = 2^9}$ page-table entries. Therefore, the virtual-address format for the two-level system is 9 (page directory) + 9 (page number) + 10 (Offset), giving page-directory number 040H and page number 01H. First locate the page-directory entry, using PTBR to obtain ${000C00H+040H\times 2=000C80H}$ (multiply by 2 here because a page-table entry occupies 2 B). **Read the contents of 2 B. If the directory entry is only 1 B, obtain the corresponding directory-entry value below according to the big-endian or little-endian convention.** Suppose the value obtained here is 4523H. The physical page frame occupies 12 bits, and the final bit is 1, so the entry is valid. The page table's physical address is therefore ${452H\times 1KB}$. The physical address of page 1 where the page table is located is ${452H \times 1KB + 1 \times 2=114802H}$, from which the corresponding physical page frame is obtained.[^csb-399]

<!-- source: cs408:L402-L415 -->

### File Systems {#cs408-h-43}

* File operations
  * The open operation requires a file path or filename and then returns a file descriptor, that is, `FILE *fp = fopen(const char *pathname, const char *mode);`. Here, `*fp` is the file descriptor. Subsequent read calls do not need to search the directory in secondary storage again, making them faster.[^csb-404]
  * The system call used to delete a file is unlink, the same call used to delete a hard link. unlink takes a filename, not a file descriptor. Deleting a file requires the user to have "write" permission on that file. Only when this count becomes 0 is the secondary-storage space occupied by the file released, including both the file's data and its inode.[^csb-405]
* The procedure for reading a file:
  1. Use the filename (or file descriptor, depending on whether it has been read in) to find the file's directory entry in the active file table.[^csb-407]
  2. **Check whether the access is legal according to the access-control specification.**
  3. If the requested file data is not in memory, the process enters the blocked state and waits for the contents to be read into memory. **Note:** If the file system uses memory-mapped files, a page-fault interrupt will occur.
  4. According to the file's logical and physical organization recorded in its directory entry, convert the logical record number into a physical block number.
  5. Send an I/O request to the device driver to complete the data transfer.
* When inserting a file record and calculating the number of disk-block accesses, consider the different allocation methods.
  * With sequential allocation, if the minimum number of disk-block accesses is required, **decide whether to shift part of the array forward or backward according to the insertion position**. For example, when inserting at record 20 among 1000 records, moving the prefix forward is clearly better. **During the move, each disk block must be read once and written once, meaning two accesses per disk block**: read and write the first 19 records, for two accesses each, and then write the disk block at the corresponding insertion position once, for a total of ${19 \times 2 + 1 = 39}$ accesses.[^csb-413]
  * With linked allocation, **reading must proceed from front to back**: read 19 times, then find a free disk block and write to it, setting its next pointer to the next pointer of disk block 19. Then change disk block 19's pointer to point to the new block, meaning **two writes (one to the free disk block and one to disk block 19)**, for a total of ${19 + 2 = 21}$ accesses.
* In multilevel file indexing, inodes have a size, which must be included when calculating the maximum file size. Take the smaller of the maximum data capacity and the maximum inode capacity. For example, if an inode is 64 B, a cluster is 1 KB, 1 M clusters store inodes, and 512 M clusters store file data, then the system has ${\dfrac{1M\times1KB}{64B}=16M}$ inodes. The data area has ${512M \times 1KB=512GB}$, so the maximum file size the system can store is ${min\{512GB,\,16M\times 1KB\}=16GB}$.[^csb-415]

<!-- source: cs408:L416-L430 -->

* File-storage systems:
  * Contiguous allocation records the file's starting physical block number and size (or number of contiguous blocks) in the **process control block FCB**, so no dedicated block is needed to record them.[^csb-417]
  * Implicit linked allocation also records the starting and ending block numbers in the **process control block FCB**, without needing a dedicated block. **Note:** In implicit linked allocation, each physical block must store a **next-block pointer**, so its usable data capacity is smaller. The size of disk blocks that this method can accommodate is also limited (by the number of pointer bits), so file size is limited.[^csb-418]
* A disk buffer is similar to a cache for an external disk. Pages that are needed can be obtained from the buffer, reducing the number of disk I/O operations.
* Be sure to remember that an explicit linked FAT table stores the physical block number of the next cluster, not the number of the current block. For example: `A cluster is 2 KB, and file B's cluster-number chain in the FAT is 5000 -> 4000 -> 4500. What are the physical block numbers corresponding to file B's 5000th byte and 9000th byte?` One cluster is 2048 B, so these two bytes correspond to cluster numbers 2 and 4, respectively, and their corresponding physical block numbers are 5000 and 4500.[^csb-420]
* In file-storage techniques, a file-storage method (sequential, implicit linked, or mixed indexing) **is a storage method for an individual file**. That is, with sequential storage, the file's FCB records the starting physical block number and length of its data; with linking, it records the starting and ending pointers of the file data; with (mixed) indexing, the directory entries for direct, single-indirect, and double-indirect addressing are all in the `inode`. Thus this method lets us calculate the maximum size of a file. **Note:** This is not the size of the entire secondary-storage space, but the maximum amount of secondary storage that can be allocated to a file. It follows that the number of files is limited by the number of inodes and by how many physical blocks secondary storage can hold.[^csb-421]
* File sharing
  * Hard links: a hard link stores the file's address information in a shared inode, and each process accesses that address. Thus additions, deletions, and modifications to the file all happen together, and the count is also the same shared value.[^csb-423]
  * Symbolic links: only the file's owner has an instruction pointing to its inode; others need only the pathname. Similarly, they can share information after additions, deletions, and modifications. With this method, sharing requires only the corresponding host's network address and the file path, without being limited to one host.[^csb-424]
  * A symbolic link adds storage for the file path, so its overhead is greater.
  * A disadvantage shared by **all link-based sharing methods** is that a file has two or more paths. A program searching for all files in a specified directory will locate multiple matching files, that is, copy the same file multiple times.[^csb-426]
* Directory structures
  * A single-level directory structure has only a system directory. All users use one directory. It provides access by name, but is slow and does not allow duplicate names.
  * A two-level directory structure contains a system directory and user directories, but user directories cannot be subdivided further. It improves speed, allows duplicate names belonging to different users, and can enforce access restrictions.
  * **Both of the above structures prevent users from organizing their own files into categories.**

<!-- source: cs408:L431-L449 -->

### External Devices and I/O {#cs408-h-44}

* Several concepts in device allocation
  * Static device allocation: before a job executes, the operating system allocates all the devices, controllers, and channels it needs in one operation. It is generally used for exclusive devices. Its advantage is deadlock prevention; its disadvantage is reduced device utilization.
  * Dynamic device allocation: the system allocates a device only when the job needs it. It is generally used for shared devices. Its advantage is improved device utilization; its disadvantage is the possibility of deadlock.
  * Safe device allocation: a process can use only one device during the same time period. Once it obtains a device, it immediately enters the blocked state and cannot request any other resources. Its advantage is deadlock prevention; its disadvantage is that, for the same process, the CPU and device can only work sequentially, so process progress is inefficient.
  * Unsafe device allocation: after issuing an I/O request, a process continues running without blocking and can issue further I/O requests when it needs another device. It blocks only if the requested device is already occupied. Its advantage is that one process can operate several devices simultaneously and make rapid progress. Its disadvantage is that it can cause deadlock.
  * **Both safe device allocation and static device allocation break the "hold and wait" condition.**
* To achieve device independence, a process accesses a device by its logical device name (of course, a physical device can also be accessed by filename). The operating system can then map the logical device name to the physical device table and choose an idle device to allocate. There is a system device table (SDT), with an entry containing the driver entry point for a device class. Thus, if the physical device is replaced, only its driver entry point needs to be changed.
* Buffering: mainly **solves the mismatch between CPU speed and peripheral speed**. Channel technology: **relieves the CPU of excessive participation in complex I/O control, rather than merely addressing a speed mismatch**, improving CPU efficiency. Parallelism: improves overall system efficiency and throughput. Virtual memory: addresses storage-system capacity. DMA and other interrupt-driven I/O techniques: address inefficient data transfer between the CPU and I/O.
* The respective roles of device-independent software and device drivers
  * Device independence provides **device naming, device protection, and device allocation and release**. When a user requests a device using a logical device name, device-independent software allocates the device, maps it to a physical device name, and prevents direct user access to the device.
  * A device driver translates commands and parameters into specific requirements (for example, translating a disk-block number into a surface, track, and sector number); checks whether the user has permission; checks whether the device is idle and, if so, issues an I/O command; and specifies the exact interrupt service routine entry point or interrupt vector for the CPU. Because drivers must control I/O devices, different I/O methods (or different devices) correspond to different drivers.
  * Translating a **device name into a port address** is the responsibility of the driver software. Device-independent software provides a logical address, that is, the logical objects of the I/O interface and channel, and maps them to the relevant driver. The **driver then performs the mapping (it can consult the device tables maintained by device-independent software)**.
* Spooling technology
  * It is device-independent software, built on multiprogramming, with data transfer controlled by the system (the spool-management program).
  * Both the input process and the output process run in kernel mode. When an input or output spool has no data and no input or output tasks, the corresponding input/output process enters the blocked state. The two processes can execute concurrently.[^csb-446]
* A channel's operation: **1.** The CPU wants to operate an I/O device, sends a command to the I/O channel, and goes on to other work. **2.** The I/O channel finds the relevant channel program in memory. **3.** Start the I/O channel, carry out I/O, and organize the I/O operations. **4.** Send an interrupt signal to the CPU to report completion.
* Functions of I/O management and device management: track the state of external devices in real time; provide access operations; allocate and reclaim devices; handle device-driver, completion, and fault interrupts; and provide virtual devices (spooling technology).

<!-- source: cs408:L450-L472 -->

### Systematic Review {#cs408-h-45}

#### Scenario 1 - Application Execution Environment {#cs408-h-46}

>This scenario describes the entire process from a disk's manufacture to the creation of an environment in which applications execute normally. For normal disk access, the disk's storage space must be addressed. Through specific operations, its physical space is divided into different cylinders, disks, and sectors.[^csb-452]
Once an available disk is connected to a computer host, the computer can access a particular address on that disk using the physical disk name + disk address. For flexibility, however, we often divide the entire physical disk space into several logical spaces, similar to the C: and D: drives commonly used in Windows. Different logical spaces are called different logical disks, also known as different partitions.
Afterward, according to specific needs, the computer sets up different file systems on different logical disks. The file systems on different logical disks can then also be accessed through the virtual file system (VFS) layer using the same format.
During the above processing, an MBR, the master boot sector, is established in the first sector of the physical disk, and a PBR, the partition boot sector, is established in the first sector of each logical disk. The logical partition storing the operating system's code and data is called the primary partition.[^csb-455]
When the computer is powered on, the PC is set to an initial value, and the area to which the PC points is called the BIOS. After the BIOS completes the most basic initialization required at startup, it loads and executes the MBR from disk. According to the operating system selected by the user, the MBR loads the PBR of the corresponding primary partition. The PBR then loads the corresponding operating system into memory and transfers CPU control to the newly loaded operating system.[^csb-456]
The operating system then completes a series of initialization steps and starts the programs associated with the graphical interface, displaying that interface to the user. The user opens an executable through the graphical interface, and the program begins executing to provide services to the user.
Answer the following questions about this process:
1. What kind of process divides a physical disk's storage area into cylinders, tracks, and sectors? Who provides this process?
2. What is the step that divides a disk's physical space into different logical disks?
3. What is the process of loading file-system information onto each logical disk according to its file system called?
4. Why can the MBR load the corresponding logical disk partition according to the operating-system type?
5. Is the BIOS stored in main memory or secondary storage? Is its storage medium ROM or RAM? Why?
6. Do the programs associated with the graphical interface run in user mode or kernel mode?
7. Opening a graphical file still essentially interacts with the file system to access a file in the directory system. When was the directory system's root directory written to disk?
8. After the user double-clicks the mouse, the operating system handles this mouse operation and eventually interacts with the file system in some way. What mechanism does the operating system use to respond to mouse actions? Why can the computer provide a corresponding response to a mouse click? During which process mentioned in the passage is this connection between mouse operations and system responses established?
9. After the operating system has been loaded and initialized, applications work with its support. An application is essentially an executable binary file. Producing an executable involves the three processes of preprocessing, compilation, and linking; an executable can run only after loading. Answer the following questions about this process:[^csb-467]
   1. When a program is loaded into memory, it is divided into the code segment, data segment, .bss segment, and heap/stack areas. Which of these regions already exist logically before loading, and which are newly allocated after loading?
   2. With static linking, when is the program's virtual address space formed?
   3. With static relocation and dynamic relocation, respectively, when are the physical addresses of data formed?
10. In a modern operating system, what is the relationship between program execution and a process?
11. In the preceding description, we sometimes say that the BIOS program loads the MBR, and sometimes that the CPU loads the MBR. How are these descriptions related? Can you give similar examples from your subsequent study of operating systems?

<!-- source: cs408:L473-L484 -->

#### Scenario 2 - Process Control Blocks {#cs408-h-47}

##### Subscenario 1: The Process Control Block {#cs408-h-48}

>The process control block (PCB) is the sole marker of a process's existence. It contains all the information the operating system uses to manage the process. This scenario connects all PCB-related knowledge within the scope of the examination syllabus.
>The basic definition and organization of the PCB: the first question is, what is a PCB? We can look at a simple partial definition of a PCB in kernel code:

<figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251128133304.png" alt="Original note figure 7" width="1055" height="554" loading="lazy" decoding="async"><figcaption>Original note figure 7</figcaption></figure>

As shown, a PCB is a structure whose fields represent information about its corresponding process. The functions of these fields will be explained below. Operating systems organize PCBs very flexibly. In the 408 syllabus, "queue" is often used to describe the organization of PCBs, but a queue here need not be a strictly first-in, first-out queue in the data-structure sense. It may instead be organized using an array, linked list, or even a priority queue; "queue" is only a particular name used here. At the same time, separate queues can be established for PCBs of different process types, such as a ready queue and blocked queues for different blocking conditions. When the operating system needs to find a particular type of process, it only needs to search the corresponding PCB queue. For example, during process scheduling, the operating system searches the ready queue for a suitable process to use the CPU next.
Based on the above description, answer the following questions:

1. Can a user program access its own PCB? Why?
2. Should all blocked processes be placed in the same blocked queue? Why?

<!-- source: cs408:L485-L502 -->

##### Subscenario 2: Process Control Blocks and Process Scheduling {#cs408-h-49}

>This subscenario analyzes how PCBs participate in process scheduling. All situations in this process use a single-CPU, single-core operating system, meaning only one process can occupy the CPU at a time. Scheduling triggers depend on information stored in the PCB.
First, the operating system maintains a global variable current (its implementation can differ across operating systems; the focus here is on the principle) that points to the current process's PCB. By accessing current, the operating system obtains all the information about the current process. Whenever particular interrupts or exceptions occur, the operating system analyzes the current process to determine whether process scheduling should take place.
In round-robin scheduling, a process's PCB maintains a variable time_slice representing its time quantum. When handling a timer interrupt, the OS decrements the current process's time_silce by 1. If time_slice reaches 0, the current process should be preempted from the CPU, triggering scheduling.[^csb-488]
In preemptive priority scheduling, if the operating system discovers that a process has finished being created or has changed from blocked to ready, it should compare the newly ready process's priority with that of the current process. If the current process has lower priority, scheduling should be triggered. The priority field is also stored in the process's PCB.
In cooperative scheduling, a process can voluntarily give up the CPU through a particular system call, allowing the processes to share CPU resources. Modern operating systems usually combine cooperative and preemptive scheduling: a process can voluntarily give up the CPU, and can also be preempted when particular timing events occur.
**Selecting the new process during scheduling depends on the PCB.**
When the current process needs scheduling, it selects a suitable PCB from the ready-process queue as the new current process to use the CPU. With priority scheduling, the process with the highest priority is selected to occupy the CPU. With multilevel feedback and similar scheduling algorithms, processes of different priorities are organized into separate ready queues.
**Process switching also requires the PCB.**
When a process switch occurs, the operating system uses the PCB of the current process and that of the process being switched in to switch contexts. The main parts of a process context are its saved execution point and the values of its general-purpose registers. During a process switch, the operating system saves the current PC and general-purpose register values in the current process's PCB, then retrieves the saved execution point and register values from the new process's PCB and loads them into the registers, completing the context switch. A process switch also requires an address-space switch. With paged memory management, the page-table base register PTR determines the mapping from logical to physical addresses, and the PCB stores the PTR value for each process's page table. When a process switch occurs, the operating system loads the PTR value saved in the PCB into the page-table base register, switching address spaces.
We also know that different processes have different user stacks and kernel stacks. A process switch should therefore include a stack switch, so the PCB also stores the addresses of the process's user stack and kernel stack to ensure that the stacks can be switched. Based on this description, answer the following questions:

1. How does the current variable change after a process switch? What happens to the previously current process?
2. When a thread switch occurs between different threads of the same process, which of the context switch, address-space switch, and stack switch described above occur, and which do not? Why?
3. With priority scheduling, which data structure would organize the ready queue most efficiently? Why?
4. If the root page table always occupies an entire page frame, what optimization can be made to the PTR value stored in the PCB?
5. How do cooperative and preemptive scheduling differ in the timing of the current process's decision to give up the CPU?

<!-- source: cs408:L503-L506 -->

##### Subscenario 3: Recording State {#cs408-h-50}

>The process control block stores the process's current state. When one process creates another, the operating system allocates a PCB for the new process. From receiving its PCB until creation is complete, the process is in the new state. Afterward, as its state changes, the field identifying its state in the PCB changes accordingly.
>The most important file-system resource stored in a PCB is the pointer to the process-level open-file table. The open-file table is stored as an array or another structure in the operating system's kernel area, and the PCB stores its entry address. When the process opens a file, the operating system accesses its open-file table through the entry address stored in the current process's PCB and adds a table entry. This entry contains a pointer to the file metadata cached in memory; opening the file ultimately returns the descriptor of that entry. When the process needs to read or write the file, it passes the file descriptor as a parameter. The operating system uses the same method to find the location of the corresponding file metadata in the open-file table, allowing access to the actual file. The PCB also records the process's possession of system device resources. These fields are important for the execution of algorithms such as deadlock detection and deadlock avoidance.[^csb-505]

<!-- source: cs408:L507-L525 -->

#### Scenario 3 - The Complete Operating-System System-Call and Interrupt-Handling Process {#cs408-h-51}

>Suppose a computer system has four types of maskable interrupts, I1, I2, I3, and I4. Their interrupt-response priorities are I1&gt;I2&gt;I3&gt;I4, while their interrupt-service priorities are I2&gt;I3&gt;I1&gt;I4. I4 is the keyboard-device interrupt. Process P1 has the following code:
>`scanf("%d", &a); printf("%d", a);`
The computer uses blocking I/O. Answer the following questions:

1. Give the interrupt-mask-word table for the interrupt types in the problem.
2. Briefly describe the state changes of process P1 before and after it executes scanf().
3. During scanf execution, 5 is entered on the keyboard and Enter is pressed, and the keyboard input is transferred to the corresponding location. Answer the following questions about this process:
   1. How does the computer system know that keyboard input has occurred? Where is the keyboard data stored at this point?
   2. After keyboard input, if no other interrupts occur, which processing stages does the computer enter?
   3. How does the CPU retrieve the input data from the keyboard device? Which program controls retrieval of the data? At which stage of interrupt processing is this program called?
   4. What functions does the hardware perform during this process?
   5. After the keyboard-device interrupt has been accepted, a peripheral generates an I1 interrupt. Will the CPU necessarily respond to I1 immediately? Why?
4. From the user program's perspective, scanf() simply reads the data stored by the keyboard into variable a. Answer the following questions about this process:
   1. Is the address of variable a passed in user mode or kernel mode?
   2. Does the user program trigger an interrupt or an exception? How is that interrupt or exception triggered?
   3. Does the system rely on hardware or software to identify the specific type of system call requested by the user?
   4. How does the system jump to the execution location of the system-call service routine?
   5. Briefly describe how the system-call execution in this example interacts with the keyboard peripheral interrupt.

<!-- source: cs408:L526-L536 -->

#### Scenario 4 - Processes and Threads {#cs408-h-52}

##### Subscenario 1: Interprocess Communication {#cs408-h-53}

>Processes P1 and P2 want to communicate. Three methods are available:
>1. Shared memory: shared memory modifies the segment tables/page tables of different processes so that they map to the same physical memory:
>

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251128134658.png" alt="Original note figure 8" width="856" height="470" loading="lazy" decoding="async"><figcaption>Original note figure 8</figcaption></figure>

>
>2. Message-passing systems: message-passing systems are divided into direct communication and indirect communication.
>3. Pipe systems: process A creates an anonymous pipe, pipe, and process B communicates with process A through pipe.

1. Is the shared memory in kernel space or in the process's user space?
2. Is shared memory created in user mode or kernel mode? Why?
3. Is shared data accessed in user mode or kernel mode? Why?

<!-- source: cs408:L537-L551 -->

4. Processes P1 and P2 both use dynamically linked library S and use runtime dynamic linking. When S is loaded into memory, its physical page-frame number is M. In P1's page table, the page-table entry for S corresponds to page number N. Which of the following statements is correct? ()
   1. A. In P2, the page-table entry for S also corresponds to page number N and stores physical page-frame number M.
   2. B. When S is loaded into memory, the page tables of both P1 and P2 contain a page-table entry whose physical page-frame number is N.
   3. C. This computer must have hardware that supports dynamic relocation.
   4. D. Throughout the entire execution of P1 and P2, S's location in memory cannot change.
5. Under direct communication, what field should be added to a process's PCB? What field, at minimum, should be attached to a message sent by a process? Are the regions where processes send and receive messages in user space or kernel space?
6. Under indirect communication, what information other than the message must the send interface provide? What about the receiver? What additional fields should messages in a mailbox contain?
7. What is the relationship between process B and process A? Through what kind of interface do they access the pipe?
8. Process A wants to send the binary characters "abcdefg" to process B, while process B wants to send "1234567" to process A. If both use pipe to transmit them, can B write data before B has read the data A wrote to pipe? Why?
9. Under the same conditions as question 8, if A needs to complete an I/O operation after writing "abcd", and B continues reading the pipe after reading those four characters, what happens? When will that state change?
10. All processes have now closed the pipe's read end, so the pipe can actually already be destroyed. Why?[^csb-547]
11. Do pipe reads require mutual exclusion? Why?
12. Which elements should an operating system's pipe data structure contain? ()
    1. ① Kernel buffer ② Read and write pointers ③ Two blocked queues ④ Mutex
    2. A. ①②③④ B. ①② C. ①②③ D. ②④

<!-- source: cs408:L552-L552 -->

#### Scenario 5 - Process Address Spaces {#cs408-h-54}

<!-- source: cs408:L553-L561 -->

##### Scenario 1: Process Address Space {#cs408-h-55}

>Scenario description: In this scenario, demand paging provides the background for demonstrating "address space," a concept of vital importance in operating systems.
We will also introduce designs such as the TLB and cache to show the entire process of a user program accessing memory. M is a computer that supports two-level page tables. It has 4MB of physical memory and includes a TLB and cache. The cache is directly mapped and contains 16 cache lines, each with a data capacity of 256B. M is byte-addressable and uses fixed-length instructions, each 4B long.
Operating system N manages M. It supports multiple processes and kernel-level threads and uses virtual paged memory management. A process has an address space of 256MB, and the page size is 1KB. The page directory and each page table both occupy one page, and all entries are 2B long. An entry contains a present bit, a physical page frame number, and access-permission bits. The consecutive k bits starting at the most significant bit hold the physical page frame number; the least significant bit is the present bit; all remaining bits are permission bits.
The system currently contains two processes, P1 and P2. P1 has also created three threads, T1, T2, and T3, managed by the operating system. Some memory contents are shown below:

<figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251128154021.png" alt="Original note figure 9" width="836" height="385" loading="lazy" decoding="async"><figcaption>Original note figure 9</figcaption></figure>

Thread T1 of process P1 is currently executing. The page-table base register PTBR contains 0x000C00H, and SP contains 0x0000020H.
Answer the following questions for this scenario:

<!-- source: cs408:L562-L569 -->

1. What is the value of k?
2. T1 accesses logical address `0x2000400H`. What is the corresponding physical address?
3. If both a TLB miss and a cache miss occur during this access, what logical page number and physical page frame number should the new TLB entry contain? Which cache line should receive the memory block's data, and what should its tag be?
4. If a thread switch occurs and T2 starts executing, will the PTBR and SP registers change? T2 wants to access logical address `0x2000000H`. What will happen, and why? What specific operations does this process involve?
5. If a process switch occurs and P2 wants to access logical address `0x2000400H`, are TLB and cache hits guaranteed? Why?
6. How can process P1 be prevented from writing data for which P1 has "read-only" permission? Is this check performed by software or hardware? What happens if P1 tries to write "read-only" data?
7. What happens if a user process attempts to access a page table? Why?

<!-- source: cs408:L570-L574 -->

##### Scenario 2: Performance Analysis of Memory Allocation Methods {#cs408-h-56}

>Scenario description: Memory allocation methods
>Broadly include contiguous allocation and noncontiguous memory allocation. Contiguous memory allocation includes single contiguous allocation, fixed partition allocation, and dynamic partition allocation. Noncontiguous memory allocation mainly includes paging, segmentation, and segmented paging.
>Answer the following questions about these methods:

<!-- source: cs408:L575-L587 -->

1. Which method can be used only in a single-program system?
2. Which methods produce internal fragmentation? Which produce external fragmentation?
3. What are the main methods for allocating free blocks in dynamic partition allocation? Which method is most likely to create fragments? Which method causes large free blocks to be continually broken up?
4. Which is more suitable for sharing and protecting programs: paging or segmentation? Why?
5. Suppose a computer has 1024MB of contiguous physical memory available for allocation and uses the buddy system. Three processes, P1, P2, and P3, each require 100MB of memory. Describe the free memory regions after each operation below, and analyze how many block splits or merges each operation involves. Use the notation ${\displaystyle [}$start address of free space, end address of free space). For example, if the two memory regions ${\displaystyle [350MB,400MB )}$ and ${\displaystyle [450MB,500MB )}$ are occupied, describe the free regions as ${\displaystyle [0,350MB)、[400MB,450MB)、[500MB,1024MB)}$. Initially, memory is empty. The processes perform these operations:
   1. P1 requests 100MB.
   2. P2 requests 100MB.
   3. P3 requests 100MB.
   4. P2's memory is reclaimed.
   5. P1's memory is reclaimed.
   6. P3's memory is reclaimed.
6. Turn the scenario and questions above into a mind map to deepen your memory and understanding.

<!-- source: cs408:L588-L593 -->

##### Scenario 3: Page Faults {#cs408-h-57}

>Scenario description: In this scenario, we use an example to analyze the work performed by the operating system when a page fault occurs. When we access a logical address whose page is not in memory, a page fault occurs. Handling the page fault mainly involves the following work:
>1. Frame allocation: Prepare a free page frame for the process page to be loaded from disk. If all frames allocated to the process by the operating system are already in use, a page-replacement algorithm must be invoked to evict a page currently in memory.
>2. Page loading: Locate the corresponding page on disk and load it into the free frame that has been allocated. In this scenario, we use DMA to transfer pages between disk and memory and study some related concepts.
>3. Answer the following questions about page replacement and loading:

<!-- source: cs408:L594-L602 -->

1. What is the maximum number of DMA transfers needed for one page replacement caused by a page fault? Under what circumstances are that many transfers needed?
2. What preparations must the device driver make before a DMA transfer?
3. When is the process blocked, and when is it awakened?
4. During a DMA transfer, can another process take over the physical page frame whose data is being transferred? Why? Would the answer differ if page buffering were used for the transfer? Why?
5. What is the maximum number of page-table entries that might change while handling one page fault? What changes might each undergo?
6. Can storing a page in contiguous disk sectors usually save I/O time? Why?
7. Is a page fault an interrupt or an exception?
8. What do local replacement, global replacement, fixed allocation, and variable allocation mean? Which combinations of replacement and allocation methods are supported?

<!-- source: cs408:L603-L609 -->

#### Scenario 6 - File System {#cs408-h-58}

##### Scenario 1: Comprehensive File System Scenario {#cs408-h-59}

>Scenario description: In this scenario, we will connect as much of the entire file-system material as possible. If you can reproduce this scenario yourself during later study, you should be in good shape on file systems. With that said, let us begin.
>

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129174249.png" alt="Original note figure 10" width="912" height="612" loading="lazy" decoding="async"><figcaption>Original note figure 10</figcaption></figure>

>
The figure above shows a computer's directory hierarchy. Here, list.db is a database file containing information about every student in a school. Each student's information forms one record in the file. Because every student has a distinct student ID, we use the student ID as the key for each record. The most common operations on this database file are retrieving student information by student ID and the related create, read, update, and delete operations. We will demonstrate these later.
First, answer the following questions:

<!-- source: cs408:L610-L614 -->

1. From the perspective of a user accessing list.db, is the file's data stored contiguously or noncontiguously?
2. Is list.db a stream file or a structured file?
3. When the file's logical organization is sequential, indexed, or direct/hashed, respectively, how is a record's logical address within the file located using its key?
4. Is a file's logical address defined from the user's perspective or the operating system's perspective? What similar concept have you previously studied that uses this kind of design?

<!-- source: cs408:L615-L619 -->

>In this file system, a disk block is 1KB, and the total size of all records in list.db is 102656B. A user now wants to read all information about the student whose student ID is "6661314." Analysis shows that this student's record has logical address 51548B within the file and is 128B long. We will now analyze the work performed by the operating system to complete this operation.
>First, to access list.db, the process must open the file. The steps for opening it are as follows:
>

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129174628.png" alt="Original note figure 11" width="782" height="167" loading="lazy" decoding="async"><figcaption>Original note figure 11</figcaption></figure>

>
>Process P is the first process to open list.db, and exactly two directory entries fit in one disk block. Answer the following questions about this process:

<!-- source: cs408:L620-L630 -->

5. Which of the following information is loaded into memory during this process?
   1. The inode of list.db.
   2. The inode of dir4.
   3. The file contents of list.db.
   4. The file contents of dir4.
6. How do the per-process open-file table and the system-wide open-file table change? What does fd mean?
7. If process P2 performs the same open operation, how do the per-process and system-wide open-file tables change? If its return value is assigned to fd2, is fd2 equal to fd?
8. Directory entries are organized sequentially within a directory. What is the maximum number of disk I/O operations required for process P to open list.db?
9. When is the root directory created? How does the operating system access it?
10. To allow the corresponding record in list.db to be read later, what requirements apply to the open operation's parameters other than the filename?

<!-- source: cs408:L631-L635 -->

>The process then starts reading the relevant record in list.db. As established above, its logical address in the file is 51548B and its read length is 128B. We read the record into a previously prepared array, buffer. The specific operations are as follows:
>

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129174818.png" alt="Original note figure 12" width="1164" height="290" loading="lazy" decoding="async"><figcaption>Original note figure 12</figcaption></figure>

>
>

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129174935.png" alt="Original note figure 13" width="1152" height="268" loading="lazy" decoding="async"><figcaption>Original note figure 13</figcaption></figure>

>
>Answer the following questions:

<!-- source: cs408:L636-L642 -->

11. What exactly does operation a in the figure do?
12. What are parameter 1 and parameter 3 in the read() function?
13. If process P continues by executing operation b after completing this operation, what is the logical address within list.db of the data read into buffer2? Why?
14. Process P2, which also opened list.db earlier, now tries to access the file using the operation shown for P2. P2's parameter 1 and parameter 3 are the same as P's. Is the data that P2 reads into buffer the same as the data that P reads into buffer? Why?
15. A third process, P3, now tries to write to list.db. How do its open() parameters differ from those used by P and P2?
16. What conditions must be met for this open() operation to succeed? After open() completes, how do the per-process and system-wide open-file tables change?

<!-- source: cs408:L643-L644 -->

>Suppose all the operations above complete successfully. Processes P, P2, and P3 have opened and accessed list.db in that order. They next close list.db in the order P, P2, P3. P's specific operation is shown below:

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129175033.png" alt="Original note figure 14" width="1253" height="302" loading="lazy" decoding="async"><figcaption>Original note figure 14</figcaption></figure>

>

<!-- source: cs408:L645-L648 -->

17. What is parameter 4 in the figure above?
18. After process P's close() operation completes, how do the per-process and system-wide open-file tables change?
19. Will the inode of list.db that was loaded into memory be removed from memory? Why? What about after P2 completes close(), and after P3 completes close()?

<!-- source: cs408:L649-L650 -->

>Next, we continue exploring how the operating system locates the exact disk position of this record, whose logical address is 51548B. This involves the organization of physical file allocation. For this process, assume that the file metadata for list.db has already been loaded into memory, and answer the following questions:

<!-- source: cs408:L651-L657 -->

20. How many disk blocks does this file's content, totaling 102656B, occupy when each disk block is 1KB? Which file block contains this record? Is the answer to the preceding question logical or physical?
21. If physical file allocation is contiguous, what is stored in the physical-address field of the file metadata? How many disk I/O operations are needed to read this record?
22. If physical file allocation is linked, what is stored in the physical-address field of the file metadata? How many disk I/O operations are needed to read this record?
23. If physical file allocation uses FAT, what is stored in the physical-address field of the file metadata? How many disk I/O operations are needed to read this record?
24. If file allocation uses mixed indexing and the file metadata contains 10 direct pointers, one single-indirect pointer, and one double-indirect pointer, with each address entry occupying 4B, how many disk I/O operations are needed to read this record? What is the maximum file length supported by this file system?
25. How many disk data blocks and disk index blocks does the file occupy in total, respectively?

<!-- source: cs408:L658-L660 -->

>We have analyzed which disk blocks require I/O operations. Next, we examine how I/O operations on those disk blocks are actually performed.
>First, for the file system to read a disk block into memory or write memory contents into a disk block, a lower-level program must provide an interface for directly reading disk blocks. Answer the following questions about this process:

<!-- source: cs408:L661-L664 -->

26. What program provides the file system with an interface for directly reading disk blocks? Which part of an I/O device can this program exchange data with?
27. Reading and writing disk blocks here, and page replacement in the earlier scenario, generally both use DMA for data transfers.
28. How does the operating system learn that a DMA transfer has completed? Where is the data stored when the DMA transfer finishes? For a character device using interrupt-driven I/O, where is the data that has been read when the device raises an interrupt?

<!-- source: cs408:L665-L666 -->

>To improve system performance, we establish a disk-block buffer cache in memory. Answer the following questions about the buffer cache:

<!-- source: cs408:L667-L672 -->

29. Is the buffer cache in user space or kernel space?
30. When a process wants to access a disk block, what does the operating system do first?
31. If the disk block the process wants to access is not in the buffer cache, what does the operating system do next?
32. Why can a buffer cache improve system performance?
33. When the DMA controller responsible for disk data transfers raises an I/O interrupt, is the data already in the buffer cache? Why?

<!-- source: cs408:L673-L676 -->

>After working through the entire process of accessing the corresponding record in list.db, we still have some unresolved details. Let us discuss them one by one:
>

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129175629.png" alt="Original note figure 15" width="1254" height="396" loading="lazy" decoding="async"><figcaption>Original note figure 15</figcaption></figure>

>
>This figure shows the fields of the superblock and their meanings. Each field's specific value is marked above it. The file system's disk block size is 1KB, and an index node (inode) is 4B. Using this information, answer the following questions:

<!-- source: cs408:L677-L682 -->

34. When is the information in the superblock written?
35. What is the purpose of the verification area?
36. What is the maximum number of files that can exist in this file system?
37. Ignoring disk capacity, what is the maximum number of disk blocks that can exist in the data area?
38. If the inode number of list.db is 326, in which physical disk block is its inode stored, counting blocks from 0? Which inode within that block is it?

<!-- source: cs408:L683-L686 -->

>Suppose this file system is mounted under directory dir6 in another file system, as illustrated below:
>

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129175729.png" alt="Original note figure 16" width="900" height="566" loading="lazy" decoding="async"><figcaption>Original note figure 16</figcaption></figure>

>
>Here, fs1_1 and fs1_2 are other mounted file systems. Answer the following questions:

<!-- source: cs408:L687-L689 -->

39. Must the file systems represented by fs1_1, fs1_2, and dir6 use the same physical file allocation method?
40. The entire process of accessing a record in list.db corresponds to the following:

    <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129180042.png" alt="Original note figure 17" width="1214" height="565" loading="lazy" decoding="async"><figcaption>Original note figure 17</figcaption></figure>

<!-- source: cs408:L690-L693 -->

>Next, we discuss some other file operations. For simplicity, we return to our original file system for this analysis:
>

> <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20251129180151.png" alt="Original note figure 18" width="837" height="536" loading="lazy" decoding="async"><figcaption>Original note figure 18</figcaption></figure>

>
>First, we write to list.db, continually appending records until even the last disk block allocated to the file is full. If we continue writing, the file system must allocate a new free disk block to the file. Answer the following questions:

<!-- source: cs408:L694-L697 -->

41. If file allocation uses direct indexed allocation, what operation is performed on the file's index?
42. If free disk blocks are managed with a bitmap, how is a new disk block allocated to the file?
43. If free disk blocks are managed with grouped linking and group 0, the first group, contains only one remaining index block, how do the groups and the in-memory index block change after one free disk block is allocated?

<!-- source: cs408:L698-L699 -->

>We delete list.db. When a certain condition is detected, the system chooses to release all disk space occupied by list.db. Answer the following questions:

<!-- source: cs408:L700-L708 -->

44. What is the condition referred to in the question?
45. In addition to releasing all the space occupied by list.db, what other operations does the file system perform?
46. If free disk blocks are managed with a bitmap, how are the blocks previously occupied by the file released?
47. If free disk blocks are managed with grouped linking and the number of blocks in group 0 has reached its upper limit, how do the groups and the in-memory index block change after a disk block is reclaimed?
48. Now create a hard link, root/dir1/dir4/01.txt, and a symbolic link, root/dir1/dir4/02.txt, to root/dir1/a.txt. Answer the following questions:
    1. Briefly describe the specific process of creating a hard link.
    2. Briefly describe the specific process of accessing a record through a symbolic link.
    3. If hard link 01.txt is deleted, can symbolic link 02.txt still be accessed? Why?

<!-- source: cs408:L709-L712 -->

## Computer Networks {#cs408-h-60}

> Things that currently need to be memorized:
> 	1. The specific meanings of syntax, semantics, and timing (synchronization); digital data encoding (Manchester, etc.); the 802.11 data-frame format (AP); the detailed procedures of the CSMA/CD/CA backoff algorithms; interframe spaces, channel reservation, and NAV values in CSMA/CA; DHCP message types; ICMP message types; OSPF packet types; BGP message types; port numbers of the various application-layer protocols; IPv6; the complete TCP process from establishment to termination, including stage names; the various signal frequency-modulation modes; the meanings of IP address fields and special IP addresses for NAT; the characteristics of virtual circuits and datagrams at the network layer; IP address classes.
> 	2. PPP frames; basic SDN concepts; Mobile IP; the Network File System, NFS, in FTP.[^csc-712]

<!-- source: cs408:L713-L719 -->

* The maximum frame length is 1518 bytes. Its payload is at most 1500 bytes and at least 46 bytes (the MAC frame occupies 18 bytes).[^csc-713]
* The relationship between baud rate B and data rate C is ${C=Blog_2N}$, where N is the number of discrete values that one symbol can take. One symbol represents a distinct state; this can be understood as ${x}$ symbols corresponding to base ${x}$.[^csc-714]
* Network layers corresponding to the various protocols:
  * Link layer: PPP, HDLC, CSMA.
  * Network layer: ARP, ICMP, IP, OSPF.
  * Transport layer: TCP, UDP.
  * Application layer: DHCP, RIP, BGP, DNS, FTP, POP3, SMTP, HTTP, MIME.

<!-- source: cs408:L720-L723 -->

* For HTTP transmission, a TCP connection must first be established, taking 1.5 RTTs. The third handshake, or 0.5 RTT, can carry data, so requesting a web page needs only 2 RTTs.[^csc-720]
  * Pay attention to the transmission order: **TCP must be established before the page response. Examine the segment and the TCP connection's SYN field. If it is not 1, this proves that TCP has already been established, so the web request needs only 1 RTT.**[^csc-721]
  * For HTTP/1.0, every request takes 2 RTTs: 1 RTT for TCP and 1 RTT for the request and response.
  * For HTTP/1.1 without pipelining and with pipelining, the connection is persistent. Without pipelining, one request takes one RTT; with pipelining, multiple requests take one RTT.

<!-- source: cs408:L724-L728 -->

* The IP datagram fields that can change along a transmission path are summarized below:
  * **If it is a private network, NAT translation is required.** At this point, a router acting on the **sender's side must change the source IP address; on the receiver's side, it must change the destination IP address.**[^csc-725]
  * Each router changes the TTL and header-checksum fields.
  * Because different links can have different MTUs, the following may change: total length, identification, flags, fragment offset, and the FLAG bits (MF, DF).[^csc-727]
  * The Mobile IP process may change the destination IP address.

<!-- source: cs408:L729-L732 -->

* **The data link layer can provide either reliable or unreliable service**, depending on the specific protocol and operating mode.
  * For example, **Ethernet MAC**: a typical implementation is **connectionless and unreliable**. It only checks the CRC, discards erroneous frames, and does not retransmit.
  - For example, **acknowledged modes of HDLC/PPP, with sequence numbers and ACKs**, can provide **connection-oriented and reliable** link service using ARQ, windows, and so on.[^csc-731]
  - **802.11 wireless MAC** has frame-level ACKs and retransmissions for unicast frames, so unicast transmission at the MAC layer can be considered **reliable to a degree, with "reliability" defined at the frame level**.

<!-- source: cs408:L733-L738 -->

* Two operating modes of Ethernet switches:
  * Cut-through switching: immediately forward through the interface selected from the destination MAC address. Delay is low, and errors are not checked. Because **only the destination MAC is examined, only 6B of data are processed**.
  * Store-and-forward switching: buffer and check the frame. Delay is high, and it is reliable. Because errors must be checked, **an entire Ethernet frame is processed, meaning at least 64B of data**.[^csc-735]
* A routing table can contain entries such as 128.20.96.0/23 and 128.20.96.128/25, because forwarding follows the best-mask, or longest-prefix-match, rule.
* Among routing protocols, RIP is an application-layer protocol transported over UDP; OSPF is a network-layer protocol transported over IP; BGP is an application-layer protocol transported over TCP.
* **The bandwidth-delay product is the maximum number of bits traveling through the channel at any instant.** Bandwidth-delay product = propagation delay ${\times}$ channel bandwidth. It represents the maximum number of bits the channel can hold.

<!-- source: cs408:L739-L744 -->

* Shannon's theorem, Nyquist's theorem, and signal-to-noise ratio:
  * Shannon's theorem: signal transmission rate = ${Wlog_2(1+S/N)}$, where W is the frequency bandwidth, and S (signal power) / N (noise power) is the signal-to-noise ratio. S/N can be expressed in decibels (dB). For example, if S/N = 1000, then ${10\times log_{10}(S/N)=30dB}$.[^csc-740]
  * Nyquist's theorem: signal transmission rate = ${2Wlog_2V}$, where W is bandwidth and V is the number of symbols.[^csc-741]
  * **Distinguish transmission from propagation in a channel: transmission refers to the rate at which the signal is sent; propagation speed is the speed at which the signal travels through the channel.**
  * The higher the signal-to-noise ratio, the greater the signal power, and the more energy each transmitted symbol receives. The symbols are therefore more stable, and the transmission error rate decreases.[^csc-743]
  * With a **fixed signal-to-noise ratio**, a lower transmission rate means that each symbol receives more energy and becomes more stable, as above.

<!-- source: cs408:L745-L749 -->

* Window sizes and points to remember for reliable transmission mechanisms, or ARQ protocols, at the data link layer:
  * Single-frame sliding window, or Stop-and-Wait (S-W): both the sending and receiving windows have size 1.
  * Go-Back-N (GBN): with ${n}$-bit sequence numbering, the receiving window always has size 1, and the sending window is ${1\lt W\leq 2^n-1}$.
  * Selective Repeat (SR): with ${n}$-bit sequence numbering, let the sending and receiving windows be ${W_T、W_R}$. Then ${1\lt W_R、1\lt W_T、W_R+W_T \leq 2^n}$. **However, note** that if the sending window is smaller than the receiving window, the receiving window will never be full; if the sending window is larger than the receiving window, the receiving window will necessarily discard some frames. It is therefore best to use ${W_R=W_T}$, giving a **maximum receiving-window size of ${W_T=2^{n-1}}$**.[^csc-748]
  * Reminder: **When calculating channel utilization, the ${n}$ in the numerator means ${n}$ packets, namely ${W_T}$.**

<!-- source: cs408:L750-L758 -->

* In CSMA/CD, ${\tau}$ is the time corresponding to the greatest separation between two hosts. To confirm that transmission is collision-free, include the time for collision information to return after a collision, giving the **contention period**, ${2\tau}$. This transmission delay is also the minimum ${RTT}$ delay in the protocol. If other devices introduce delay during transmission, the contention period can be shortened. Consider a 100BaseT device: because Ethernet's minimum frame length is 64B, the contention period is ${64B/100Mbit=5.12\mu s}$. If other devices introduce a delay of ${\Delta x\mu s}$, the **one-way propagation delay can be written as ${5.12 \mu s/2 - \Delta x \mu s}$**. The distance between the two hosts can then be determined from signal propagation speed. **Remember to convert to one-way propagation delay here, because the contention period, or minimum frame length, is calculated for a round trip.**[^csc-750]
  * `In a CSMA/CD network, signals propagate at 200m/us. A 100BASE-T cut-through switch is added halfway between stations A and B. By how many meters can the theoretical maximum distance between A and B be reduced?` Because the switch uses cut-through switching, it only needs to examine the 6-byte destination MAC address, or 48 bits, taking 0.48us. The one-way propagation delay can therefore be reduced to at least ${2.56us-0.48us=2.08us}$, giving a distance of ${416m}$. Thus ${\Delta x=512m-416m=96m}$.[^csc-751]
  * `Hosts A and B are connected to opposite ends of an 800m cable. At t=0, both send a frame to the other. Each frame is 1500 bits long, including the header and preamble. There are four repeaters between A and B, each introducing a delay of 20 bit times when forwarding a frame. The data rate is 100Mbit/s. CSMA/CD backoff lasts r contention periods, each 512 bit times. After the first collision, A selects r=0 and B selects r=1. Signal propagation speed is 200000km/s. Find the time when B has completely received A's frame.` Propagation delay is ${\dfrac{800}{2\times 10^8}=4\mu s}$, repeater delay is ${4\times \dfrac{20bit}{100\times 10^6}=0.8\mu s}$, and total propagation delay is ${4.8\mu s}$. The transmission delay for one frame is ${\dfrac{1500bit}{100Mb/s}=15\mu s}$.[^csc-752]
    1. At ${t_0=0}$, A and B transmit simultaneously.
    2. At ${t_1=4.8\mu s}$, both receive the other's first ${bit}$, detect a collision, stop transmitting, and back off. Each has already transmitted for ${t_1}$, so data remains on the channel.
    3. At ${t_2=4.8+4.8\mu s}$, B's final bit reaches A. A now senses no collision on the channel and immediately retransmits because ${r=0}$.
    4. At ${t_3=t_2 + 4.8 \mu s}$, A's first bit reaches B. Because the contention period is ${5.12\mu s \lt  4.8\mu s}$, B is still backing off. B therefore detects a collision and does not transmit.[^csc-756]
    5. At ${t_4=t_3+15 \mu s}$, A's final bit reaches B.
    6. After ${t_4}$, B's backoff time has elapsed and there is no collision on the channel, so B sends data to A.

<!-- source: cs408:L759-L764 -->

* TCP-related notes:
  * Sequence numbers in TCP segments are measured in **bytes**. For example, if MSS = 1KB, sending 2MSS = 2KB of data increases the sequence number by 2K.
  * Be sensitive to the units above. Example: <mark>The data rate is 2.5Gb/s, and the PDU lifetime is 51.2s. To transmit as much data as possible, what is the minimum bit length of the sequence-number field in the PDU header?</mark> Multiplying the two values gives a maximum amount of data in transmission of ${2^{37}b}$. Because sequence numbers are in bytes, they must cover at least ${2^{34}B}$, meaning ${34b}$.[^csc-761]
  * In TCP congestion control, the congestion window **increases by one whenever an acknowledgment segment is received**.[^csc-762]
  * During TCP connection establishment and release, **only the first handshake segment of a TCP connection may have ACK other than 1**. The ACK flag in all the other segments is 1.
  * TCP defines four timers: the **retransmission timer**, the **persist timer** (when the receiver's window is 0, the sender periodically probes the window size using the persist timer to avoid waiting in deadlock), the **keepalive timer** (if the client fails, it prevents the server from waiting indefinitely), and the **time-wait timer** (the Time_Wait interval in TCP connection termination, lasting ${2\times MSL}$).

<!-- source: cs408:L765-L771 -->

* Do not forget that DNS servers also have caches and may not need to query a root name server.
* In GBN, the other side's sequence numbers are received one by one. Suppose a data frame has the form ${R_{x,y}}$, where ${x}$ is our own data-frame sequence number and ${y}$ is the acknowledgment, namely the next sequence number expected. If we receive ${R_{2,\,2}}$ and ${R_{4,3}}$, the frame we should send is ${R_{3, 3}}$: **we want frame 3, not frame 5**.
* Determining MAC addresses with ARP:
  * ARP operates only within the local network. If H1 sends data to H2 in the same network, the destination MAC address is ${MAC_{H2}}$. If it sends to H3 in another network, it gives the data directly to the default gateway, so the destination address is ${MAC_R}$. R1 then handles the task. **If the network segment between R1 and H1 uses NAT, the source IP address must also be changed.**
  * If H3 is on a network segment directly connected to R1, meaning that the next hop for that network is --- in R1's routing table, R1 encapsulates the frame with destination physical address ${MAC_{H3}}$. Otherwise, it consults its routing table for the next hop and sets the destination physical address to that router's ${MAC_{R2}}$.
  * Continue in this way until the destination host receives an ARP request and sends an ARP reply frame. This reply goes only to the party that sent the request: H2 replies to H1; H3 replies to R1; R1 replies to H1. **H1 receives R1's reply frame, not H3's.**[^csc-770]
  * **Note:** In the process above, ARP between hosts on different subnets does not happen as one uninterrupted operation. H1 uses the destination IP address to determine that H3 is not in the same subnet, so it directly uses ARP to obtain the default gateway's MAC address and passes the data to the default gateway, or router. The default gateway then performs ARP, and the process continues hop by hop.

<!-- source: cs408:L772-L775 -->

* A network's **subnet address** is the address with all host bits set to 0; it represents the network itself.
  * If an address-allocation question gives information about several subnets, allocation must follow their subnet numbers. For example, suppose four subnets are given: subnet 1, 130.130.19.0; subnet 2, 130.130.20.0; subnet 3, 130.130.11.0; subnet 4, 130.130.12.0. Because a subnet number has all-zero host bits, the allocation must use 255.255.255.0.[^csc-773]
  * A router's forwarding-table destination IP is the corresponding host's subnet address, rather than the specific host IP address.[^csc-774]
* A virtual circuit is a logical circuit, so bandwidth does not need to be allocated. When the virtual circuit is first established, a VCID, or virtual circuit identifier, is assigned to distinguish it from other virtual circuits. **The full destination address is used only when initially establishing the connection; subsequent virtual-circuit headers use the virtual circuit identifier, reducing overhead.** Virtual circuits still require routing and forwarding, deliver data in order, and provide reliable connections.[^csc-775]

<!-- source: cs408:L776-L783 -->

* The process by which host H accesses a web server, `www.bilibili/o-Sakurajimamai-o/home.com`, starting from the initial state:
  * If host H needs an IP address, DHCP must configure one. The exchange includes a DHCP Discover message (H sends a MAC broadcast to the server, using source IP 0.0.0.0), a DHCP Offer message (the server sends a MAC unicast to H, using the server's IP as the source but still 255.255.255.255 as the destination IP), a DHCP Request message similar to DHCP Discover, and a DHCP ACK message similar to DHCP Offer. This process uses the client/server model, with UDP encapsulation at the transport layer. **Supplement:** The DHCP Offer contains <mark>IP addresses for the host's request, DHCP, the default gateway, DNS, and so on, as well as the subnet mask</mark>.[^csc-777]
  * To access `www.bilibili/o-Sakurajimamai-o/home.com`, the host first needs the MAC address of the local DNS server. It broadcasts an ARP request, which is forwarded by switches, if present, updating the relevant device tables. After obtaining the MAC address, it queries the local DNS server. This process is not counted among the DNS accesses. The local DNS server also has a periodically updated cache: if the answer is cached, it returns it directly; otherwise, it queries a root name server, a com name server, and a .bilibili name server, totaling three DNS queries. This stage uses DNS, UDP encapsulation at the transport layer, IP at the network layer, and CSMA/CD at the data link layer, with Ethernet V2 frames.[^csc-778]
  * **Note:** During the domain-name resolution above, the host **only needs to send one DNS request**. Depending on its cache and query method, the local DNS server may need no outgoing DNS query if it has a cached answer; with recursive queries, it only needs to send one query to a root name server; with iterative queries, it needs to send three DNS requests.[^csc-779]
  * Once the specific IP address is known, **the MAC address of the default gateway R1 is still needed, assuming the web server is outside the subnet; if it is inside, the web server's MAC is requested instead**. Another ARP request must therefore be sent. Only after the MAC address is obtained can an HTTP request be sent to the web server, forwarded through switches and routers, updating relevant device tables. If the router is a NAT router, the source IP address must change. **TCP** establishes the connection: the first and second handshake messages exchange no data, and the third carries the resource request. The server then sends the resource and an acknowledgment number. **This process takes ${2RTT}$.** Note that this fundamentally concerns transmitting the .html file, assumed here to be no larger than 1MSS. The TCP flow-control window limits transmission, which may take more RTTs; follow the specific question's requirements. H now displays the relevant website page. This stage uses HTTP, with TCP encapsulation at the transport layer. **Note that HTTP itself is connectionless: an HTTP connection need not be established first, but a TCP connection must be established.** The network layer uses IP, and the data link layer uses CSMA/CD.[^csc-780]
  * During the entire process above, the DNS server receives three frames related to this exchange: `the ARP broadcast frame requesting the DNS server's MAC`, `the MAC unicast frame asking for the web server's MAC (the DNS request)`, and `the ARP broadcast frame requesting the default gateway's MAC, or the web server's MAC if it is in the same subnet`.[^csc-781]
  * H and the web server exchange the relevant data, subject to the flow-control window and differences among HTTP variants, such as HTTP 1.0/1.1, persistent/nonpersistent connections, and pipelined/nonpipelined requests.
  * H and the web server disconnect, entering TCP connection termination.

<!-- source: cs408:L784-L790 -->

* UDP checksum calculation resembles IP checksum calculation and uses **16-bit addition**. IP checks only the header, whereas UDP checks both header and data. **Sender:** Put all 0s in the checksum field. If the data does not contain an even number of 0s, pad it with one 0. Neither the pseudo-header nor this padding 0 is transmitted. **Add these values, remembering end-around carry**, then **take the one's complement** and transmit. **Receiver:** Add the pseudo-header and any padding 0, then sum the values, remembering end-around carry. If the result is all 1s, there is no error.[^csc-784]
* If a question provides a specific IP datagram, identify the TCP data by "examining the TCP header length; what remains is the data," then perform the required sequence-number or ACK prediction, window calculation, and so on.
* A network's local broadcast address consists of 32 ones: IP 255.255.255.255. This is the **broadcast IP for the local network**, or limited broadcast, and it can be broadcast internally. Understand its purpose by comparing it with directed broadcast. A directed broadcast consists of the subnet number followed by all-one host bits, as in the usual broadcast packet addressed to xxx.xxx.xxx.255/24. The fundamental difference is that **directed broadcasts can be forwarded by routers and intercepted externally, reducing network security**.[^csc-786]
* In CSMA/CD, the number of bits that a sender may have transmitted before detecting a collision and stopping is called the **maximum fragment-frame length**. It equals **the maximum amount of data that can be sent within 2τ**. Thus Ethernet's minimum frame length must not be smaller than this value. **Why a minimum frame length is needed:** If the data rate is R, the maximum number of bits sent during $2\tau$ is ${R\cdot 2\tau}$. To ensure that a sender is still transmitting when a collision occurs and can detect it, the frame cannot be too short. Otherwise, the sender might finish before the collision returns and assume success, causing **erroneous data through an undetected collision**.
* A repeater reshapes and amplifies a signal as it passes through to prevent it from weakening, so repeaters are needed for transmission over certain distances. **Repeaters amplify digital signals; amplifiers amplify analog signals.** Amplifiers are used for long-distance analog signal transmission.
* The data link layer implements flow control between two nodes; the network layer provides logical communication between two hosts; the transport layer provides logical communication between two processes.
* IP fragmentation requires the lengths of the resulting IP datagrams to be multiples of 8. Note that this does not apply to the initial IP datagram. For example, H1 sends data to H2 through routers R1 and R2. The MTU from H1 to R1 is 1500B, and the MTU from R1 to R2 is 400B. If H1 sends more than 1500B of data, H1 may send 1500B datagrams; fragmentation is needed only when R1 sends them to R2, **at which point the fragments must be multiples of 8**. If the MTU from R1 to R2 is also 1500B, fragmentation is likewise unnecessary. **The multiple-of-8 requirement applies only to fragmented datagrams.**[^csc-790]

<!-- source: cs408:L791-L799 -->

* IP multicast sends to multiple MAC addresses according to the destination IP address, using the form 01-00-5E-xx-xx-xx. **The last 24 bits consist of 0 followed by the last 23 bits of the IP address.** For example, H1, H2, and H3 belong to multicast IP 224.0.64.32, while H4 belongs to 224.128.64.32. Both networks map to 01-00-5E-00-40-20, so **all can receive the datagram**. After H4 receives it, however, it discovers that the destination IP is not its own and discards it.[^csc-791]
* The ports in a NAT forwarding table are the host's internal and external port numbers. For example, H1 sends a datagram to establish a control connection with a web server using internal IP 197.128.0.23, internal port 1234, external IP 110.1.2.3, and external port 4321. The web server's IP is 110.0.0.0. The NAT table then contains internal IP 197.128.0.23 and port 1234, and external IP 110.1.2.3 and port 4321.
* When FTP establishes a data connection, it accesses TCP port 20 and closes the connection immediately after transferring the data. Another data transfer requires a new connection. A multithreaded process needs multiple data connections but only one control connection.[^csc-793]
* DHCP follows the C/S model, so it is an application-layer protocol and uses UDP for transport.
* In IP addressing: `0.0.0.0` may be used as a source address, for example in DHCP messages, but may not be used as a source address; `xxx.xxx.xx.0`, meaning an IP address whose **host number** is all 0s, represents the network itself, or the subnet, and cannot be a destination address; `224~239.x.x.x` are multicast addresses and may only be destination addresses, never source addresses; `255.255.255.255 or x.x.x.255` are limited and directed broadcast addresses, respectively, and neither may be a source address.[^csc-795]
* When host H1 communicates with host H2, consider the following:
  * First, check whether the destination IP belongs to the same LAN by performing an `and` operation with the subnet mask. If the destination is on the same LAN, the data can be sent directly, so communication does not pass through the default gateway. If the two hosts are on different LANs and H1's default gateway is misconfigured, meaning that H1 and the router are not on the same LAN, H1 cannot reach the router and normal communication fails. H1 and its gateway must be in the same subnet; this is also why a router has multiple IP addresses.
  * If a host's subnet mask is misconfigured, it may cause H1 to incorrectly believe that H2 is on the local LAN even though it is not, preventing normal communication.
* The three private IP address ranges are (1) `10.0.0.0/8 ~ 10.255.255.255`, (2) `172.16.0.0/12 ~ 172.31.255.255`, and (3) `192.168.0.0/16 ~ 192.168.255.255`. **They are called reusable addresses** and require NAT translation. NAT examines port_id, so it operates at the transport layer.[^csc-799]

<!-- source: cs408:L800-L807 -->

* CSMA/CA wireless LANs:
  * CSMA/CA transmits as follows. When a station has data to send, it listens to the channel. If the channel is idle, it **waits for DIFS** and then sends the entire data frame directly. Otherwise, a collision is detected and backoff is required. Backoff must be used in these cases: `1. The channel is found busy before the first frame is sent. 2. Every retransmission. 3. Sending the next frame after each successful transmission.` The station then waits for the preceding station to finish transmitting the frame, taking DIFS + transmission delay + SIFS + ACK transmission delay. After detecting an idle channel, it first waits for DIFS, then chooses a random backoff interval, with a slot count of ${cnt}$. For the ${k}$th backoff, the interval is randomly chosen from ${[0,\,(2^{4+k}-1)]}$, with the same maximum range as CSMA/CD: 1023. It then counts down ${cnt}$ backoff slots and sends the data frame only after all have elapsed. **If a transmission is detected during backoff, that backoff interval is not counted.**[^csc-801]
  * Except when the channel is detected idle, **all other cases require** the backoff algorithm, including:
    * Finding the wireless channel busy before transmitting a frame.
    * Every **retransmission of a frame**, or **continuing to send the next frame after a successful transmission**.
  * Therefore, when a station successfully sends one piece of data and wants to send the next, it should **wait for DIFS and a random backoff interval before contending for the channel again**.
  * In the process above, each station wishing to send a data frame uses the source station to notify other stations on the channel of how long it will occupy the channel, including the ACK return time. This is also why, after detecting a collision, it must wait for the interval described above before sending again. **This process is not exclusive to the reservation mechanism.**
  * Example: `DIFS is 128us, SIFS is 28us, and an ACK frame is 2B. Stations A, B, and C each want to send a 100B data frame. The channel data rate is 8Mb/s. A plans to transmit at t=0s; B and C both plan to transmit at t=50us. A backoff slot lasts 20us, and B and C choose 3 and 5 slots, respectively. Find when A, B, and C each finish sending their data.` A senses that the channel is idle and sends directly, taking DIFS + transmission delay = 228us. After SIFS + ACK transmission delay, the time is 258us. B detects that the channel is idle. After DIFS, B and C begin counting down backoff slots. B finishes first after ${3\times 20=60\mu s}$, then sends immediately. After the transmission delay of 100us, the time is t=546 and B has finished transmitting. After SIFS and ACK transmission delay, at 576us, C detects an idle channel, waits for DIFS, then uses its remaining two backoff slots. It finally completes data transmission at t=844us.

    <figure class="fig"><img src="/blog/kaoyan-408/figures/note-20250921190717.png" alt="Original note figure 19" width="1425" height="428" loading="lazy" decoding="async"><figcaption>Original note figure 19</figcaption></figure>

<!-- source: cs408:L808-L815 -->

* In fast retransmit, or the duplicate-ACK technique, the receiver immediately sends an ACK for every segment received. If it receives out-of-order segments, it sends an ACK for each one. For example, the sender transmits datagrams 0–10. If datagram 5 is lost, receiving segments `6, 7, 8` causes the receiver to send three ACKs, all with the same information, ${ACK_{seq}=5}$. After receiving three duplicate ACKs, the receiver retransmits datagram 5. If segment 8 is lost, however, datagrams `9, 10` only cause the receiver to send two duplicate ACKs, so fast retransmit cannot occur.[^csc-808]
* Mobile IP communication:
  * Foreign-agent care-of addresses can be the same and are not unique, because a foreign agent forwards datagrams according to MAC addresses.[^csc-810]
  * H2, whose permanent IP address is 20.1.2.2, communicates with mobile host H1. H1's home-agent IP is 100.0.2.0, its permanent IP is 100.0.2.100, and its care-of address is 15.0.8.8.
    1. H2 sends a datagram to H1 with `source IP: 20.1.2.2, destination IP: 100.0.2.100`. The home agent then intercepts it, encapsulates it, and sends it to the foreign agent.
    2. In the datagram the home agent sends to the foreign agent, the outer, encapsulating IP header is `source IP: 100.0.2.0, destination IP: 15.0.8.8`, while the inner, encapsulated IP header is `source IP: 20.1.2.2, destination IP: 15.0.8.8`. The foreign agent receives it, decapsulates it, then re-encapsulates it and sends it to mobile host H1.[^csc-813]
    3. H1 receives the inner, encapsulated datagram: `source IP: 20.1.2.2, destination IP: 100.0.2.100`.
    4. H1 sends a datagram to H2 with `source IP: 100.0.2.100, destination IP: 20.1.2.2`, **forwarded directly by the foreign agent without any processing**.[^csc-815]

<!-- source: cs408:L816-L816 -->

* $\begin{array}{|c|c|c|c|c|}\hline\text{Network number} & \text{Host number} & \text{Usable as source IP address} & \text{Usable as destination IP address} & \text{Purpose} \\\hline\text{All 0s} & \text{All 0s} & \text{Yes} & \text{No} & \text{Default route; represents the Internet; this host on this network} \\\hline\text{All 1s} & \text{All 1s} & \text{No} & \text{Yes} & \text{Broadcast address for this network} \\\hline\text{All 0s} & \text{Specific value} & \text{Yes} & \text{No} & \text{A specific host on this network} \\\hline\text{Ordinary network number} & \text{All 0s} & \text{No} & \text{No} & \text{Represents a network} \\\hline\text{Ordinary network number} & \text{All 1s} & \text{No} & \text{Yes} & \text{Broadcast address for a network} \\\hline\text{Class D network number} & \text{Specific value} & \text{No} & \text{Yes} & \text{Multicast; some addresses are not assigned} \\\hline\text{127} & \text{Specific value} & \text{Yes} & \text{Yes} & \text{Loopback address} \\\hline\text{Class E network number} & \text{Specific value} & \text{No} & \text{No} & \text{Reserved address} \\\hline\end{array}$[^csc-816]

<!-- source: cs408:L817-L826 -->

* VLAN:
  * A LAN is divided into multiple broadcast domains, with each VLAN forming one broadcast domain. Because a VLAN itself is a switch, it also divides collision domains: ${n}$ hosts have ${n}$ collision domains.[^csc-818]
  * Compared with an Ethernet frame, an 802.1Q frame must specify a 4B VLAN tag. The minimum data-field size therefore changes from 46B to 42B, while the maximum frame size increases from 1518B, consisting of 1500B of data + 18B of MAC overhead, to 1522B.
  * How VLANs work:
    * **"Add Tag":** When a frame is about to **leave** a **Trunk port** and its VLAN ID **differs from** that Trunk port's PVID, or native VLAN, the switch must add an 802.1Q tag.[^csc-821]
    * **"Strip Tag":**
      - When a frame is about to **leave** an **Access port**, its tag must be removed regardless of its VLAN, and it must be sent to the host as an ordinary MAC frame.[^csc-823]
      - When a frame is about to **leave** a **Trunk port** and its VLAN ID **equals** that Trunk port's PVID, or native VLAN, the switch must remove the tag, or leave it untagged, and transmit it as an ordinary MAC frame.
    * Interfaces connected to hosts are usually configured as **Access** ports with a **PVID**, such as PVID 10, indicating that the host belongs to VLAN 10. Interfaces between switches are configured as **Trunk** ports. When H1 in VLAN 10 sends an **ordinary MAC frame, or untagged frame**, switch S1 receives it on an Access port with PVID 10 and immediately classifies it internally as VLAN 10. S1 looks up VLAN 10's MAC address table. If the destination host is on another Access port of S1 with PVID 10, S1 "strips the tag," ensuring that it is an ordinary MAC frame, and forwards it directly. If the frame is a broadcast or its destination MAC is on another switch, S2, S1 must forward it through a Trunk port. A Trunk port also has a PVID, called the native VLAN; assume PVID 1. **S1 checks whether the frame's VLAN ID, 10, equals the Trunk port's PVID, 1.** Because 10 does not equal 1, S1 must "add a tag" and send an 802.1Q frame with VID=10. Only when S1 sends a frame belonging to the native VLAN, VLAN 1, meaning that the frame's PVID equals the Trunk port's PVID, does S1 "strip the tag" and transmit an **ordinary MAC frame**. **Therefore, switches do not necessarily exchange only 802.1Q frames.** Finally, when S2 receives the 802.1Q frame, it reads the tag and identifies VLAN 10. If it must forward the frame to a host on an Access port, S2 also "strips the tag" and sends an ordinary MAC frame to the host. For more detail, see Huke University's question 35: [2022 408 Postgraduate Entrance Examination Computer Networks Mock Test 03: Answers and Explanations](https://www.bilibili.com/opus/596286729367485481).

<!-- source: cs408:L827-L829 -->

* FTP has active and passive modes, with active mode being the default. In active mode, the FTP server establishes a connection to the client. Therefore, when the TCP connection is released, the Time-Wait timer should run at the FTP server: it should wait 2 x MSL before entering CLOSED.[^csc-827]
* A symbol can carry several bits, but Manchester encoding is special: one high-to-low or low-to-high level pattern represents one bit, so Manchester uses two symbols to transmit one bit.
* Border routers, using eBGP or iBGP, must establish a TCP connection before exchanging information.

<!-- source-content:cs408:end -->

[^csa-25]: **Correction or assumption:** Correction: Manchester encoding is a line-coding method for digital data, not an analog-signal modulation scheme.

[^csa-48]: **Correction or assumption:** Clarification: Inserting into an empty linked queue normally initializes both front and rear, and insertion/deletion also changes link fields. The front and rear values after removing the last node depend on whether the implementation uses a dummy head.

[^csa-52]: **Correction or assumption:** Clarification: When applying infix-to-postfix conversion after reversal, reverse the handling of associativity for equal-precedence operators; the usual pop condition cannot simply be copied unchanged. Reverse tokens, keeping a multidigit number as one token.

[^csa-59]: **Correction or assumption:** Correction: The Catalan formula and its product with n! count binary-tree shapes and labeled binary trees, respectively, not the number of individual traversal sequences. Unique reconstruction from preorder/postorder plus inorder also requires distinct node identities.

[^csa-63]: **Correction or assumption:** Correction: C_(n-1) uses the node count after removing the root. The quantity reduced by one for this count is the number of nodes, not a height supplied to a Catalan formula.

[^csa-70]: **Correction or assumption:** Clarification: Two null threads and n-1 nonnull threads apply to a nonempty inorder-threaded binary tree without an additional head node. In preorder or postorder threading, an endpoint may have a real child pointer, so the same thread count is not universal.

[^csa-71]: **Correction or assumption:** Correction: The directions are reversed. Ordinarily, a missing left-child pointer becomes a predecessor thread, and a missing right-child pointer becomes a successor thread.

[^csa-72]: **Correction or assumption:** Correction: If a missing right-child pointer has already been threaded to the postorder successor, that thread gives the successor directly. The difficulty arises when a real right child occupies the field and parent information is needed. The successor may be the parent or the first node in the postorder traversal of the parent's right subtree; that node is not always found by following only left pointers. A parent pointer supports the general case.

[^csa-74]: **Correction or assumption:** Correction: The successful-search comparison count is the node's level measured from the root, with the root at level 1, not the height of the subtree rooted at that node.

[^csa-76]: **Correction or assumption:** Correction: AVL and red-black trees are balanced binary search trees; B-trees and B+ trees are balanced multiway search trees, not binary trees.

[^csa-83]: **Correction or assumption:** Correction: A nonroot node in an order-m B-tree has at least ceil(m/2)-1 keys, with a separate exception for a nonempty root. A common insertion algorithm splits after insertion temporarily produces m keys and promotes a middle key. The original (m+1)/2 expression needs explicit rounding when m is even.

[^csa-99]: **Correction or assumption:** Correction: A loser tree selects the current smallest record. Increasing k increases the approximately log2(k) tree height and the comparisons per adjustment, while reducing merge passes and I/O. Total comparisons also depend on the record count and other factors; they are not unconditionally determined only by the number of initial runs.

[^csa-111]: **Correction or assumption:** Clarification: The nth power of an ordinary adjacency matrix counts walks of length n, allowing repeated vertices and edges. It does not count simple paths if “path” is restricted to paths without repeated vertices.

[^csa-121]: **Correction or assumption:** Correction: A binary linked structure has two pointer fields and a ternary linked structure has three; the order in the latter part of the original sentence is reversed.

[^csa-141]: **Correction or assumption:** Clarification: Machine word length ordinarily describes the processor's natural data-processing width, often associated with its general-purpose registers and ALU. The external data bus, MDR, and machine word length need not all have equal widths; use the architecture specified in the question.

[^csa-143]: **Correction or assumption:** Clarification: Requiring instruction length to be an integer multiple of memory word length, and deriving fetch-cycle counts from that ratio, assumes a particular whole-word fetch model. This is not universal across ISAs: instructions may be encoded in bytes, and one memory read may cover multiple instructions or only part of an instruction.

[^csa-146]: **Correction or assumption:** Correction: CPI = total clock cycles / completed instructions. Instructions completed per clock cycle is IPC, which is the reciprocal of CPI when measured over the same execution.

[^csa-151]: **Correction or assumption:** Correction: Under the stated borrow-flag convention CF=Sub XOR C_out, unsigned A-B with A&gt;=B gives C_out=1 and CF=0. If A&lt;B, borrowing occurs, C_out=0, and CF=1. The original explanations for A&gt;B and subtraction underflow are reversed.

[^csa-156]: **Correction or assumption:** Correction: No borrow in unsigned subtraction means CF=0, including A=B. NOT(ZF OR CF)=1 additionally requires ZF=0 and therefore represents unsigned strictly greater, not merely “no overflow.”

[^csa-157]: **Correction or assumption:** Clarification: Mixed signed/unsigned arithmetic does not always convert to unsigned; integer promotions, conversion ranks, and representable ranges determine the result type. A cast also need not merely reinterpret a bit pattern: integer-to-floating conversion changes representation and may round. The example's extensions and result assume the usual short/int widths and two's-complement model.

[^csa-167]: **Correction or assumption:** Correction: Position 11 is binary 1011 and belongs to parity group 2. The a2 formula must also XOR a11, and the h2 formula on the next source line omits a11 as well. Including it makes the stated a2=0, h2=1, transmitted codeword, and correction result consistent.

[^csa-168]: **Correction or assumption:** Clarification: A syndrome of 0000 means that no error was detected. For an ordinary Hamming code it establishes error-free reception only under the assumption of at most one bit error; some multiple-bit errors can produce a zero syndrome.

[^csa-169]: **Correction or assumption:** Correction: This mixes traditional floating-point conventions with IEEE 754. Machine zero can represent an actual zero. IEEE 754 positive and negative zero require both the exponent field and fraction field to be zero; a normalized number with a zero fraction field still has an implicit leading 1. Underflow may also produce a subnormal value rather than zero, and an all-zero zero encoding is not possible only with a biased exponent.

[^csa-171]: **Correction or assumption:** Correction: If p is the stored fraction width, the exponent has k bits, and bias=2^(k-1)-1, the largest positive normalized value is (2-2^(-p))*2^(2^(k-1)-1). The original maximum unbiased exponent is one too small.

[^csa-172]: **Correction or assumption:** Correction: The smallest positive subnormal is 2^(1-bias-p), so the original exponent should subtract p. The stored fraction widths of binary32 and binary64 are 23 and 52 bits; normalized precision including the implicit bit is 24 and 53 bits. The stated 55 is a typo.

[^csa-173]: **Correction or assumption:** Correction: IEEE 754's default is round to nearest, ties to even. A discarded leading 1 does not always mean increment: later discarded bits distinguish more than half from exactly half, and an exact tie is resolved by the parity of the retained least significant bit.

[^csa-188]: **Correction or assumption:** Correction: A 16-bit machine is not necessarily addressed in 2B units. If the question specifies 16-bit word addressing, 128KB/2B gives 2^16 addressable locations and a 16-bit MAR. Byte addressing requires a 17-bit MAR. The quotient's unit is locations, not B.

[^csa-189]: **Correction or assumption:** Clarification: A 2B short, a 4B int, alignment equal to type size, and IEEE 754 floating point are common platform or examination assumptions, not universal guarantees of C. Use the target ABI and the question's stated conventions.

[^csa-191]: **Correction or assumption:** Clarification: A memory cycle is the minimum interval between the starts of independent accesses to the same bank. Whether preparation, internal recovery, and external transfer are separate or overlap depends on the memory and bus timing, so transfer time is not universally excluded.

[^csa-194]: **Correction or assumption:** Correction: DRAM refresh commands are normally issued by the memory controller, with the chip carrying out the refresh internally. In self-refresh mode the chip can schedule refresh itself. Transparency to the CPU does not mean independence from external control, and restoration after a read is normally automatic hardware behavior too.

[^csa-202]: **Correction or assumption:** Clarification: The calculation 1024/20480 additionally assumes a refresh period of 20480ns, which is not given in this excerpt's problem statement. Under that assumption, the result is 5%, not a dimensionless 5.

[^csa-204]: **Correction or assumption:** Clarification: Distinguish a chip organization of “8M locations x 8 bits” from a capacity expressed in MB; the number of locations determines the address count. The stated 31 and 20 count only address and data pins, excluding chip-select, read/write, clock, power, and other pins.

[^csa-211]: **Correction or assumption:** Correction: In n*T_bus, n is the number of bus transfers, or the number of words if the problem specifies one word per transfer, not the number of data bits. A multibit data bus normally transfers multiple bits per transfer cycle.

[^csa-216]: **Correction or assumption:** Correction: Four modules of 4 bits each total 16 bits, not the stated 32-bit bus width, so the example's module width conflicts with its conclusion. If four byte-organized 8-bit modules were intended, one simultaneous group is 00, 01, 10, 11; the opening 00, 10 should be checked against 00, 01.

[^csa-222]: **Correction or assumption:** Clarification: The stated average-time formula requires T_M to represent the complete miss-access time, or an appropriate parallel-access model. If cache is checked first and a miss then incurs an additional main-memory time T_M, the average is T_C+(1-H)*T_M.

[^csa-230]: **Correction or assumption:** Correction: Ordinarily, one cache line corresponds to one main-memory block. After tag comparison selects the line, the block offset selects a word or byte within it. The usual multiplexer selects data within the block, not one of several main-memory blocks stored in a line.

[^csa-242]: **Correction or assumption:** Correction: The PC increment is the instruction size divided by the addressing-unit size. A 16-bit machine need not be word-addressable: with byte addressing and a 2B instruction, the PC increases by 2. An increment of 1 requires conditions such as 2B word addressing.

[^csa-243]: **Correction or assumption:** Clarification: Enabling/disabling interrupts and changing interrupt masks are normally privileged, but a software-interrupt or trap instruction used for a system call may be executable in user mode. Not every interrupt-related instruction is privileged; classification depends on the ISA.

[^csa-248]: **Correction or assumption:** Correction: Distinguish fixed-length versus expanding opcodes from fixed-length versus variable-length instruction words, and distinguish instruction/operation counts from operand counts. Opcode expansion can be implemented within a fixed 20-bit instruction word. The counts 14 and 15 depend on the stated reserved-code schemes, not merely on whether instruction words have fixed length.

[^csa-249]: **Correction or assumption:** Correction: Both fixed-length and variable-length instruction sets need a program counter. Fixed-length instructions merely make sequential PC increments and instruction boundaries simpler to determine.

[^csa-253]: **Correction or assumption:** Correction: A classic 32-bit MIPS J-type target concatenates the high 4 bits of PC+4, the 26-bit target field, and two trailing 0s. Appending 0s is a left shift by 2, not a right shift. PC+4 must not be replaced indiscriminately with the current instruction address, especially at a 256MB boundary.

[^csa-256]: **Correction or assumption:** Clarification: The base PC and displacement unit of relative addressing are ISA-defined and may require scaling. A classic MIPS conditional branch uses PC+4+(sign_extend(imm16)&lt;&lt;2). Indexed addressing generally forms a data effective address and does not justify universally removing a relative branch's scale factor.

[^csa-258]: **Correction or assumption:** Correction: In M[imm], imm is the directly specified effective address, while the operand is the memory content at that address, M[imm]. The operand is not the address itself.

[^csa-262]: **Correction or assumption:** Clarification: A calling convention divides context preservation: the caller saves needed caller-saved registers, and the callee saves callee-saved registers that it will modify. Before returning, Q must restore the registers and stack state it saved; Q does not unconditionally save all of P's context.

[^csa-272]: **Correction or assumption:** Correction: Multicycle and pipelined processors are distinct organizations; a multicycle datapath need not be combined with pipelining. A multicycle PC needs write control because only selected clock cycles of an instruction update it.

[^csa-273]: **Correction or assumption:** Correction: A single-cycle processor still updates the PC, registers, and any written memory at the specified clock boundary. The register file and data memory normally still need write enables. A conventional single-cycle datapath omits an IR because the instruction remains available from instruction memory throughout the cycle, not because no state is written.

[^csa-277]: **Correction or assumption:** Clarification: A fixed instruction-gap rule applies only with specified pipeline stages, register read/write timing, and forwarding. In a classic five-stage pipeline without forwarding but with write-before-read register timing, two independent intervening instructions are ordinarily sufficient; without that timing, a larger gap is needed.

[^csa-296]: **Correction or assumption:** Correction: Transfer rate is the bus clock frequency multiplied by data transferred per cycle, or total data divided by elapsed time. “Number of clock cycles x data per cycle” alone gives an amount of data, not a rate.

[^csa-302]: **Correction or assumption:** Correction: Saving the PSW preserves the interrupted program's flags, processor mode, and interrupt state for restoration. Nested interrupts are controlled by interrupt enables, masks, and priorities; saving the PSW alone does not prevent another interrupt.

[^csa-303]: **Correction or assumption:** Correction: The interrupt-response cycle ordinarily is the phase that executes the implicit interrupt operation and performs the hardware response, not the later interrupt service routine. The ISR then executes as an ordinary instruction sequence, with each instruction having its own instruction cycle.

[^csa-304]: **Correction or assumption:** Clarification: An interrupt vector identifies an ISR, while priority selection chooses which pending interrupt source to service. Hardware arbitration may supply a vector, but the existence of a vector does not itself imply that priority selection is necessarily done in hardware.

[^csa-306]: **Correction or assumption:** Correction: The maximum polling interval is buffer capacity divided by transfer rate, y/x seconds. The original x/y has units of inverse seconds and is inverted.

[^csa-308]: **Correction or assumption:** Clarification: A word counter ordinarily counts transferred bytes, words, or bus transfer units, not necessarily whole data blocks. The total also depends on the initial count and when completion is tested. The bounds 2^n-1 and 2^n assume the corresponding maximum preload and zero/underflow conventions; counter width alone does not determine every DMA transfer's size.

[^csb-319]: **Correction or assumption:** PBR means Partition Boot Record, not partition table. In the traditional MBR scheme, the partition table is part of the disk's master boot record and records partition starts, sizes, types, and related information.

[^csb-325]: **Correction or assumption:** This describes legacy BIOS/MBR booting. The BIOS selects the boot device before reading its MBR; the MBR normally finds the active partition in the partition table and loads its partition boot record. The MBR does not first choose the boot device. UEFI uses a different boot mechanism.

[^csb-332]: **Correction or assumption:** Dynamic relocation does not by itself imply a noncontiguous address space or physical allocation. The single-base-register-plus-offset model addresses a contiguous physical region. Paging and segmentation can support noncontiguous allocation for a process as a whole and use page tables or segment tables for translation.

[^csb-333]: **Correction or assumption:** Kernel entry saves the interrupted user context, commonly in a trap frame on the kernel stack through hardware and software cooperation. Register contents are not simply user-stack contents, and the PCB stores more than kernel-stack context. System calls can block when permitted in process context, but ordinary hard-interrupt handlers cannot sleep. Nested interrupts are not the same as process scheduling.

[^csb-336]: **Correction or assumption:** The const qualifier does not directly determine a storage section. A local const object with automatic storage duration may be on the stack, in registers, or optimized away. String literals and suitable read-only objects with static storage duration are commonly placed in read-only data, depending on language rules, the compiler, and the target platform.

[^csb-337]: **Correction or assumption:** Not all global and static variables occupy the initialized writable data section. Conventionally, writable objects with nonzero initial values use .data, while zero-initialized or implicitly initialized objects with static storage duration use .bss; implementations can arrange these differently.

[^csb-342]: **Correction or assumption:** Each thread maintains its own execution stack and stack pointer, but threads in the same process share an address space. Another thread can generally access a stack object if it has its address. Here, 'private' describes ownership of execution state, not memory protection against other threads.

[^csb-343]: **Correction or assumption:** The CPU instruction set determines the available trap or system-call instructions. The operating-system ABI chooses the mechanism, call numbers, argument registers, and calling convention. Different operating systems on the same architecture need not use identical instructions or argument conventions. Different architectures generally have different encodings and mechanisms; the interface must still be distinguished from the instruction.

[^csb-348]: **Correction or assumption:** The source's 'computational bases' is a typo for CPU-bound processes. Favoring I/O-bound processes generally lets their short CPU bursts initiate the next I/O operation promptly, improving CPU/device overlap. Preventing overwritten data is not a universal basis for assigning process priorities.

[^csb-354]: **Correction or assumption:** These are common teaching heuristics, not a strict ordering shared by all operating systems; the categories can overlap. Interrupt-service priority belongs to the interrupt mechanism and is not directly the same as process scheduling priority. Real-time priorities and the ordering of system and user processes depend on the scheduler's policy.

[^csb-356]: **Correction or assumption:** If the kernel is already executing, it can schedule a newly ready higher-priority process at a permitted preemption point without an additional external interrupt. This does not mean interrupts are never involved: device interrupts can wake such processes, and remote rescheduling on multiprocessors often uses interprocessor interrupts.

[^csb-359]: **Correction or assumption:** Atomic operations are not equivalent to disabling interrupts, and they do not necessarily lock the bus. Modern CPUs commonly implement atomic read-modify-write operations through cache coherence and exclusive access to the relevant cache line; bus locking is used only in particular cases. Disabling relevant interrupts can protect some kernel operations on a uniprocessor, but does not by itself provide multiprocessor mutual exclusion.

[^csb-360]: **Correction or assumption:** Time-quantum selection usually considers response requirements, the number of processes in the ready queue, and processing capacity/context-switch cost. The source's 'number of ready queues' should be 'number of processes in the ready queue,' not the count of distinct queues.

[^csb-363]: **Correction or assumption:** Blocking, wakeups, synchronization, and necessary mutual exclusion for pipe operations are implemented by the operating system's pipe mechanism, not automatically by a special 'pipe process.' An ordinary anonymous pipe has a read end and a write end; bidirectional communication normally uses two pipes.

[^csb-381]: **Correction or assumption:** The statement that a thread switch leaves the page-table register unchanged applies to threads within the same address space. Switching between threads of different processes normally also switches address spaces. Processes sharing an address space and some kernel-thread switches need not change the page-table base, so 'every process switch always changes it' is not universal.

[^csb-382]: **Correction or assumption:** Segmentation is noncontiguous allocation for the process as a whole: different segments can occupy separate physical regions, while each individual segment is normally contiguous. Translation uses the base and limit for each segment in a segment table. A single relocation register is more typical of contiguous partition allocation; segmentation as a whole should not be classified as contiguous allocation.

[^csb-391]: **Correction or assumption:** The MMU is the memory management unit: a hardware component that performs address translation and associated protection checks. It is not the translation process itself.

[^csb-392]: **Correction or assumption:** In demand paging or segmentation, resident pages or segments are in main memory; other contents can reside in backing storage or be created on demand. The program is not entirely 'permanently resident in secondary storage.' The distinction here is whether all relevant pages must already be resident and whether page-fault handling and demand loading are supported.

[^csb-395]: **Correction or assumption:** The entire instruction set and PSW cannot be classified as visible only to system programmers. The instruction set includes user-mode instructions, and status words often contain user-visible condition codes. Privileged instructions, protected control registers, and kernel internals are the system-level details. Application transparency of virtual memory chiefly concerns its translation and management details.

[^csb-396]: **Correction or assumption:** The stated page size is 4 KB, not 8 KB. One page holds 4 KB / 8 B = 512 = 2^9 entries, so each level indexes at most 9 page-number bits. The virtual page number has 36 bits, giving a minimum of 36 / 9 = 4 levels. The original final answer of four levels is unchanged, but its intermediate 8 KB, 1 K, and 10-bit calculation is incorrect.

[^csb-399]: **Correction or assumption:** 0x114802 is the address of the level-two page-table entry at index 1, not the physical address of the target data page or data. Read that 2-byte entry, check its presence and permission bits, extract the upper 12-bit physical frame number, and compute frame number × 1 KB + page offset; the offset here is 0. The stated entries are 2 bytes and must be reconstructed using the appropriate endianness, not treated as 1-byte entries in this example.

[^csb-404]: **Correction or assumption:** POSIX open returns an integer file descriptor; the C library's fopen returns a FILE * stream pointer. fp is not a descriptor, and *fp denotes a FILE object; fileno(fp) can obtain the underlying descriptor. The original code also puts parameter declarations inside a call; an actual call is FILE *fp = fopen(pathname, mode);.

[^csb-405]: **Correction or assumption:** POSIX unlink normally requires write and search/execute permission on the containing directory, with possible sticky-bit and other restrictions; write permission on the target file itself is not required. A zero link count does not immediately free a file while open references remain. Its data and inode are reclaimed after the last relevant reference is released.

[^csb-407]: **Correction or assumption:** An ordinary read locates an open-file object through a file descriptor; pathname lookup normally occurs during open. If disk I/O is needed, the kernel locates the relevant blocks and submits the request before the process waits or blocks. Step 3 must not be interpreted as completing a wait for data before steps 4 and 5 submit the I/O. Access to a memory-mapped file can instead trigger paging through a page fault.

[^csb-413]: **Correction or assumption:** The 39 accesses here and the 21 below assume one record per disk block, uncached required blocks, and exclusion of allocation-metadata costs. Moving the prefix under contiguous allocation also requires free contiguous space before the file. If a block contains several records, count the actual affected blocks rather than substituting the number of records.

[^csb-415]: **Correction or assumption:** The 16 M inodes limit the number of files/inodes, not the number of data clusters available to a single file. One inode can address many clusters through multilevel indexing. Maximum single-file size depends on one inode's addressing capacity, file-size field limits, and available data space. The total inode count given here does not establish a 16 GB limit; the data-region capacity is 512 GiB in binary units.

[^csb-417]: **Correction or assumption:** FCB means File Control Block, not process control block; the latter is PCB. Contiguous allocation records the file's starting physical block and length.

[^csb-418]: **Correction or assumption:** FCB here also means file control block. A next-block pointer consumes space inside each block. Its bit width limits the representable block-number range, the number of addressable blocks, and hence addressable capacity; it does not directly determine the size of an individual disk block.

[^csb-420]: **Correction or assumption:** At 2048 B per cluster, byte 5000 lies in the third cluster (zero-based index 2), whose physical cluster number in the given chain is 4500. Byte 9000 lies in the fifth cluster (zero-based index 4), but only three clusters are given, so the fifth cluster number cannot be determined. If the chain is complete, byte 9000 is beyond the file's allocated space. The original answers 5000 and 4500 are incorrect.

[^csb-421]: **Correction or assumption:** An inode stores direct block addresses and addresses of indirect index blocks, not directory entries. Directory entries associate filenames with inodes; they are distinct from the block-index entries inside an inode.

[^csb-423]: **Correction or assumption:** Hard links share one inode, so content changes are visible through all of them. Unlinking one hard link removes only that directory entry and reduces the link count; it does not simultaneously remove the other hard links, which remain usable.

[^csb-424]: **Correction or assumption:** A symbolic link is a file with its own inode whose contents name a target pathname. References to the target inode do not belong exclusively to the file's owner. An ordinary symbolic link does not implement a network-access protocol: remote access requires a mechanism such as a mounted network filesystem, not merely a host address and pathname.

[^csb-426]: **Correction or assumption:** Multiple paths can cause a link-unaware traversal or copy program to process the same file repeatedly, but this is not inevitable. Tools can identify a file by inode, preserve hard links, choose whether to follow symbolic links, and detect cycles.

[^csb-446]: **Correction or assumption:** Spooling input/output services do not have to run entirely in kernel mode. In real systems, services such as print spoolers commonly use user-mode daemons that access kernel drivers through system calls. Their execution mode, blocking, and concurrency depend on the implementation.

[^csb-452]: **Correction or assumption:** A disk itself is not an internal subdivision comparable to a cylinder or sector. Distinguish surfaces (heads), tracks, cylinders, and sectors. For CHS addressing, the three components are the cylinder number, head number, and sector number.

[^csb-455]: **Correction or assumption:** A primary partition is a category in the MBR partitioning structure, not a partition defined by containing an operating system. Depending on support, an OS can be installed in a primary partition or a logical partition inside an extended partition. The R in MBR/PBR means Record; those records conventionally occupy the corresponding first sectors. This passage uses the legacy BIOS/MBR model.

[^csb-456]: **Correction or assumption:** Legacy BIOS selects a boot device and loads its MBR. Ordinary MBR boot code normally finds the active partition and loads its PBR. User selection of an operating system is generally a boot-manager feature, not a feature of every MBR. A PBR normally continues by loading a bootloader, which then loads the operating system.

[^csb-467]: **Correction or assumption:** Using the finer breakdown of the C toolchain given earlier, producing an executable involves preprocessing, compilation, assembly, and linking. This three-stage list omits assembly unless 'compilation' is being used broadly enough to include it.

[^csb-488]: **Correction or assumption:** time_silce is a misspelling of time_slice; both should refer to the same time-quantum field. The passage describes the teaching model in which a timer interrupt decrements this counter.

[^csb-505]: **Correction or assumption:** In a typical implementation, a process's file-descriptor table entry points to a system-wide open-file object/open file description, which in turn refers to metadata such as an inode or vnode. The open-file object also holds the offset and status flags. A descriptor-table entry therefore should not simply be equated with a direct inode pointer.

[^csb-547]: **Correction or assumption:** For a POSIX pipe, closing every read end causes subsequent writes to raise SIGPIPE or, under the relevant signal-handling conditions, fail with EPIPE. There are no receivers, but open write ends still reference the pipe object. The object need not be destroyed immediately and is generally released after all relevant references are closed.

[^csc-712]: **Correction or assumption:** NFS is a separate network file-system protocol, not a component of FTP.

[^csc-713]: **Correction or assumption:** The 18B is the combined overhead of the 14B untagged Ethernet MAC header and 4B FCS, not the entire MAC frame. The 64–1518B frame length excludes the preamble, start-frame delimiter, and interframe gap.

[^csc-714]: **Correction or assumption:** N is the number of distinguishable states available to each symbol, not the number of symbols transmitted. Each symbol carries log₂N bits, so C = B log₂N.

[^csc-720]: **Correction or assumption:** The 2RTT result is an idealized model excluding DNS, TLS, transmission time, processing time, and similar costs. The client receives SYN+ACK after 1RTT and can carry the request in the third handshake. The HTTP/1.0 estimate normally assumes nonpersistent connections; HTTP/1.1 pipelining combines request waiting, but response size still affects total time.

[^csc-721]: **Correction or assumption:** SYN=0 alone does not prove that a connection is established; a reset segment can also have SYN clear. Determine this from connection state and packet context. A simple request/response over an established connection takes about 1RTT when other delays are ignored.

[^csc-725]: **Correction or assumption:** Communication within a private network does not inherently require NAT. Typical outbound source NAT changes the source address, and replies have their destination address translated according to the existing mapping. Translation depends on mappings and policy, not simply on being a sender or receiver.

[^csc-727]: **Correction or assumption:** IPv4 fragmentation changes fields such as total length, MF, and fragment offset and requires recomputing the header checksum. All fragments of the same original datagram retain the same Identification value. A router may not fragment a datagram with DF=1 or simply clear DF to do so.

[^csc-731]: **Correction or assumption:** Standard PPP does not provide data-frame sequencing, acknowledgment/retransmission, or reliable delivery. Suitable HDLC modes can provide these mechanisms; HDLC's acknowledged modes should not be attributed to PPP.

[^csc-735]: **Correction or assumption:** Store-and-forward can filter frames that fail the FCS check, but this does not guarantee reliable delivery or automatically provide retransmission. The preceding 6B value is the forwarding-start threshold in a simplified cut-through model; the entire frame still has to be forwarded.

[^csc-740]: **Correction or assumption:** Shannon's formula gives the theoretical maximum reliable information rate, or capacity, of a band-limited Gaussian-noise channel, not an arbitrary actual signal rate. W is in Hz, and S/N inside the formula must be a linear ratio, not a dB value.

[^csc-741]: **Correction or assumption:** Under ideal noiseless-channel assumptions, Nyquist's formula gives the maximum bit rate. V is the number of distinguishable states per symbol, not the total number of symbols sent.

[^csc-743]: **Correction or assumption:** A higher signal-to-noise ratio can also result from lower noise power, so it does not by itself imply higher signal power. Energy per symbol depends on signal power and symbol rate; the claim that a lower rate increases symbol energy assumes conditions such as fixed signal power.

[^csc-748]: **Correction or assumption:** W_T &gt; W_R does not necessarily cause frame drops; arrivals and window advancement determine whether frames fall outside the receive window. When W_T &lt; W_R, the sending window limits outstanding frames, so the larger receive window is not automatically fully utilized. With symmetric windows, W_T = W_R ≤ 2^(n−1); the receiving-window maximum should be labeled W_R.

[^csc-750]: **Correction or assumption:** 2τ is the worst-case round-trip propagation and relevant device-delay budget, not a minimum RTT or ordinary transmission delay. At 100Mb/s, a 512-bit slot is 5.12μs. Device delays consume that budget and reduce the permitted cable propagation time; they do not shorten the fixed slot time.

[^csc-751]: **Correction or assumption:** A switch separates collision domains, so A and B connected through it do not share one end-to-end CSMA/CD collision domain. Its cut-through forwarding delay cannot be treated as a repeater delay within a single 2τ budget to establish the 96m result. The arithmetic requires an additional, idealized shared-collision-domain device assumption.

[^csc-752]: **Correction or assumption:** The stated one-way delay is 4.8μs, giving a 9.6μs round trip, which exceeds the 5.12μs, 512-bit slot at 100Mb/s. This topology therefore violates the collision-detection bound for standard minimum-length frames. The following timing is only a hypothetical exercise that omits details such as the jam signal and interframe gap, not a valid standard Ethernet configuration.

[^csc-756]: **Correction or assumption:** The inequality is reversed: 5.12μs &gt; 4.8μs. When B is not transmitting and senses A's signal, it detects a busy channel and defers; it does not detect a collision. Whether B is still backing off also depends on when the backoff interval starts.

[^csc-761]: **Correction or assumption:** Using the decimal units customary for communication rates, 2.5Gb/s × 51.2s = 128×10^9 bits = 16×10^9 bytes, not exactly 2^37 bits or 2^34 bytes. Rounding up still gives a minimum sequence-number field of 34 bits.

[^csc-762]: **Correction or assumption:** Increasing by about 1MSS per acknowledgment is a simplified slow-start rule, and it concerns ACKs that acknowledge new data. Congestion avoidance typically increases the window by about 1MSS per RTT in total, not by one for every ACK.

[^csc-770]: **Correction or assumption:** ARP requests and replies are local to a link; routers do not forward H1's original ARP request hop by hop to H3. H1 resolving its gateway and routers resolving their next hops are independent ARP exchanges. Their replies do not form a return chain from H3 to H1.

[^csc-773]: **Correction or assumption:** The listed addresses constrain the mask but do not uniquely determine it. /24 is a possible choice, and longer prefixes may also be valid. Use the required host counts and any fixed-length or variable-length subnetting conditions.

[^csc-774]: **Correction or assumption:** A forwarding-table destination is generally a prefix and can also be a /32 host route or a /0 default route; it is not restricted to an ordinary subnet address.

[^csc-775]: **Correction or assumption:** Virtual circuits do not inherently guarantee reliable delivery, nor do they necessarily avoid allocating bandwidth. Resource reservation, acknowledgment/retransmission, and other reliability guarantees depend on the particular technology and service. Forwarding identifiers along an established path is distinct from reliability.

[^csc-777]: **Correction or assumption:** DHCP Offer and ACK delivery may be broadcast or unicast depending on client state, the broadcast flag, and related conditions. It is not fixed as MAC unicast with destination IP always 255.255.255.255. Also distinguish the offered client address, server identifier, and configuration options such as gateway and DNS.

[^csc-778]: **Correction or assumption:** The sample string contains slashes and is not a single DNS hostname; DNS resolves a URL's host component. The host directly ARPs for the DNS server only if it is on-link and no valid ARP cache entry exists; otherwise it resolves the next hop. Three queries through root, TLD, and authoritative servers are one uncached model. DNS can also use TCP, and modern full-duplex switched Ethernet does not use CSMA/CD contention.

[^csc-779]: **Correction or assumption:** Actual root DNS servers do not provide recursive resolution for ordinary clients. They normally return referrals, which the recursive resolver follows iteratively. Distinguish a textbook fully recursive illustration from real deployment; query counts depend on caching, delegation levels, aliases, and other factors.

[^csc-780]: **Correction or assumption:** HTTP is normally described as stateless; this does not imply connectionless transport. HTTP/1.x normally uses TCP, while HTTP/3 uses QUIC. A valid ARP cache entry avoids another ARP exchange. The 2RTT result still requires the idealized assumptions, and page-transfer time also depends on congestion and receive windows, resource size, and other limits.

[^csc-781]: **Correction or assumption:** A DNS request asks for the IP address or other DNS records associated with a name, not the web server's MAC address. The DNS server receiving exactly these three frames also depends on conditions such as sharing H's broadcast domain and having empty ARP caches; it is not a universal count.

[^csc-784]: **Correction or assumption:** The UDP checksum covers the pseudo-header, UDP header, and data. Pad one zero byte for calculation when the data length in bytes is odd; do not count the number of zero bits. An all-ones verification sum means no error was detected, not that errors are impossible. In IPv4, a zero UDP checksum field can also indicate that the checksum was omitted.

[^csc-786]: **Correction or assumption:** A directed-broadcast address can identify a remote subnet, but modern routers disable IPv4 directed-broadcast forwarding by default and forward it only when explicitly permitted. Security concerns include broadcast amplification; this does not mean such traffic is necessarily intercepted externally.

[^csc-790]: **Correction or assumption:** The multiple-of-8 requirement applies to the payload length of every nonfinal IPv4 fragment, not the whole IP datagram length. The final fragment's payload need not be a multiple of 8. Fragment offsets use 8-byte units. A source host may also have to fragment a datagram that exceeds its outgoing interface MTU, following the same rule.

[^csc-791]: **Correction or assumption:** One IPv4 multicast address maps to one Ethernet multicast MAC, rather than separate transmissions to multiple unicast MAC addresses. Different IP groups can map to the same MAC. Relevant interfaces on the same link may accept the frame because of this mapping collision and then filter it at IP, but hosts on arbitrary different networks do not all receive it.

[^csc-793]: **Correction or assumption:** In active FTP, the server normally connects from TCP source port 20 to a client-selected data port. In passive FTP, the client connects to a negotiated server port, so data transfers do not universally access destination port 20. The control connection normally uses server port 21; multithreading itself does not guarantee that every transfer can share one control connection.

[^csc-795]: **Correction or assumption:** The second 'may not be used as a source address' should say 'may not be used as a destination address': 0.0.0.0 can be a source in situations such as initial address acquisition. Network and broadcast addresses depend on the complete subnet mask; a final octet of 0 or 255 alone is insufficient, and special cases such as /31 have separate rules.

[^csc-799]: **Correction or assumption:** Private addresses can be routed within private networks, so not every communication requires NAT. Basic NAT translates network-layer IP addresses. Port-multiplexing NAPT/PAT also inspects and translates transport-layer ports; NAT should not be described as operating only at the transport layer.

[^csc-801]: **Correction or assumption:** Sensing a busy channel in CSMA/CA is not collision detection; wireless stations typically infer failure from conditions such as a missing ACK. CWmin and CWmax depend on the PHY and configuration, so the stated range is one parameter model. A busy channel freezes the remaining counter, which resumes after the required idle interval and NAV conditions are satisfied.

[^csc-808]: **Correction or assumption:** The sender, not the receiver, retransmits after receiving three duplicate ACKs. Out-of-order segments normally trigger immediate duplicate ACKs, but normal in-order traffic can use delayed ACKs. Actual TCP ACK values number bytes; the packet numbers here are only a simplified illustration.

[^csc-810]: **Correction or assumption:** Multiple mobile nodes using the same foreign agent can share that agent's care-of address; this does not mean distinct foreign agents may arbitrarily duplicate IP addresses. The agent uses registration/visitor bindings to decapsulate and deliver packets, with MAC addressing potentially used on the final link.

[^csc-813]: **Correction or assumption:** The tunnel's inner packet retains the original IP datagram, so its destination must remain H1's permanent address, 100.0.2.100, not the care-of address 15.0.8.8. The care-of address belongs in this outer destination field. The foreign agent removes the outer encapsulation and delivers the inner packet to H1.

[^csc-815]: **Correction or assumption:** In classic triangular routing, a mobile node can send directly through the visited network with its permanent address as the source, but normal routing processing still occurs. Deployments can also use reverse tunneling through the home agent.

[^csc-816]: **Correction or assumption:** The default route is the prefix 0.0.0.0/0, not the single address 0.0.0.0 equated with the whole Internet. Special rules for an all-zero network number have historical context and do not authorize arbitrary modern host communication with 0.x.x.x addresses.

[^csc-818]: **Correction or assumption:** A VLAN is a logical broadcast domain, not a switch itself. Collision-domain separation comes from switch ports. One collision domain per host assumes a corresponding topology and half-duplex model; modern full-duplex links do not experience CSMA/CD collisions.

[^csc-821]: **Correction or assumption:** These are common default Access/Trunk rules, but allowed-VLAN lists, native-VLAN tagging options, and vendor configuration also matter. PVID primarily classifies incoming untagged frames. Native-VLAN egress tagging can be configurable, so the later 'must strip the tag' statement is not universal.

[^csc-823]: **Correction or assumption:** An Access port forwards only frames permitted for its VLAN. 'Regardless of its VLAN' must not be interpreted as stripping tags from all VLANs and delivering all of them to that host.

[^csc-827]: **Correction or assumption:** FTP active/passive mode determines who initiates the data connection, and the default depends on the client. TIME_WAIT depends on who actively closes that TCP connection, not who opened it. The active closer normally waits 2MSL; active FTP mode alone does not establish that this must be the server.
