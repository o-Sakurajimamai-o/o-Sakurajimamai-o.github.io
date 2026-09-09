---
title: "408 Exam Notes: Data Structures, Computer Organization, OS, and Networks"
date: 2026-09-09
description: "Revision notes for China’s 408 computer science entrance exam, covering data structures, computer organization, operating systems, and computer networks."
translationKey: kaoyan-408
draft: false
---

These notes come from my preparation for China’s 408 computer science postgraduate entrance exam. They preserve the calculations, revision reminders, and original figure, organized around its four subjects. Clear errors and assumptions that depend on the exercise are explained in the text.

For the mathematics subjects, see my [calculus and linear algebra notes](../kaoyan-math/).

These notes collect common mistakes, calculation details, and items to memorize while preparing for the 408 examination. They are organized into data structures, computer organization, operating systems, and computer networks. Apply each formula and numerical result together with the hardware, protocol, and timing assumptions stated in the question.

## Data Structures {#data-structures}

<!-- source: cs408:L1-L3 -->
### Tree Shapes, Degrees, and Traversals {#tree-shapes-degrees-traversals}

- In a binary search tree with distinct keys, an inorder traversal produces a strictly increasing sequence. For a fixed set of keys, this sequence is fixed, but the inorder sequence alone does not uniquely determine the tree's shape.
- The number of distinct shapes of an unlabeled binary tree with $n$ nodes is the Catalan number

  $$
  C_n=\frac{1}{n+1}\binom{2n}{n}.
  $$

  If all nodes have distinct labels and different label assignments count as different trees, the total is $C_n\times n!$. These formulas count tree shapes or labeled trees; they are not automatically the number of different permutations produced by a particular traversal. When node identifiers are distinct, **preorder plus inorder**, or **postorder plus inorder**, uniquely determines a binary tree.
- Let $d_{\max}$ be the maximum node degree in a rooted tree, and let $n_i$ count nodes with $i$ children. Then

  $$
  n=\sum_{i=0}^{d_{\max}}n_i
   =1+\sum_{i=1}^{d_{\max}}i\,n_i.
  $$

  **The two sums start at different indices, and the second must include the factor $i$.** The first counts nodes; the second counts edges and uses the fact that a tree has $n-1$ edges.

<!-- source: cs408:L5-L8 -->
- Converting a tree with $n$ nodes to its left-child, right-sibling binary representation produces $n-1$ non-null pointers. A left pointer refers to the first child; a right pointer refers to the next sibling. **The number of left pointers equals the number of non-leaf nodes in the original tree**; the remaining non-null pointers are right pointers.
- An AVL tree's balance factor is **left-subtree height minus right-subtree height**. Taking an empty tree to have height $0$, let $N_h$ be the minimum number of nodes in an AVL tree of height $h$. Then

  $$
  N_0=0,\qquad N_1=1,\qquad
  N_h=N_{h-1}+N_{h-2}+1\quad(h\geq2).
  $$

  A minimum-size tree uses subtrees of heights $h-1$ and $h-2$, in either left-right order. The balance factor need not be positive $1$ at every non-leaf node. At the same height, the maximum is attained when every level is full, giving $2^h-1$ nodes.

<!-- source: cs408:L4-L4 -->
### Hashing and Merging Sorted Sequences {#hashing-and-merging}

- The average search length for an unsuccessful hash-table lookup depends on the **initial addresses** the hash function can produce. For example, with an array of length $10$ and a hash function modulo $7$, assume the initial addresses are equally likely: calculate the unsuccessful search length starting at each index from $0$ through $6$, then average them. If a probe sequence reaches indices $7$ through $9$, those probes still count toward that search's length.

<!-- source: cs408:L9-L9 -->
- Merging two nonempty sorted sequences of lengths $m$ and $n$ requires at most $m+n-1$ key comparisons in the worst case, with time complexity $O(m+n)$.

<!-- source: cs408:L11-L13 -->
### Heap Insertion and Deletion {#heap-insertion-deletion}

- **Insertion:** Put the new element in the next available position of the complete binary tree, at the end of the heap array. For a max-heap, compare it with its parent and swap if it is larger, continuing upward. The existing subtrees still satisfy the heap property, so additional sibling comparisons are unnecessary.
- **Deleting the root:** Remove the root and replace it with the last node in the heap. Then sift down: compare the two children, choose the larger one, compare it with the current node, and swap if necessary. Stop when no further swap is needed. If there is only one child, compare with that child alone. For a min-heap, reverse the comparison direction.

<!-- source: cs408:L10-L10 -->
### Graph Degrees and Adjacency Matrices {#graph-degrees-adjacency-matrices}

- In an undirected graph, the sum of all vertex degrees is twice the number of edges; a self-loop contributes $2$ to the degree. In a directed graph, a vertex's degree is its indegree plus its outdegree. The total indegree and the total outdegree each equal the number of edges.

<!-- source: cs408:L14-L14 -->
- For an adjacency matrix $A$ used to count connections, $A^n[i][j]$ gives the number of **walks** of length $n$ from vertex $i$ to vertex $j$. A walk may repeat vertices or edges, so this cannot generally be interpreted as the number of simple paths.

## Computer Organization {#computer-organization}

<!-- source: cs408:L15-L16 -->
### Performance Measures and Instruction Execution {#performance-instruction-execution}

- CPI is the average number of clock cycles per instruction: it measures how many cycles an instruction requires on the given hardware. CPI and clock-period duration are different quantities:

  $$
  T_{\mathrm{CPU}}=\text{instruction count}\times\mathrm{CPI}\times\text{clock period}.
  $$

  Which micro-operations a datapath can complete within one cycle depends on its design; this does not mean that every register can be used only once per cycle. The definition of CPI does not contain the clock-period duration, but a processor design change can affect both.

<!-- source: cs408:L18-L18 -->
- Count reads and writes separately when calculating memory accesses. For example, `a[x] = a[x] + 1;` accesses `a[x]` twice in the usual model that first reads the value and then writes it back. Account separately for instruction fetches, accesses needed for address calculation, or any register and cache optimizations allowed by the question.

<!-- source: cs408:L20-L22 -->
- The PC increment depends on **instruction length and the addressable unit**, not merely on a machine being described as “16-bit.” For sequential execution, divide the instruction length in bytes by the number of bytes in one addressable unit. For a $2\,\mathrm{B}$ instruction, the PC increases by $1$ if memory is addressed in $16$-bit words, but by $2$ if memory is byte-addressed.
- General-purpose registers can hold operands and addresses. Their width is usually related to machine word size and datapath width; determine the actual address width from the architecture and the question.
- In a microprogrammed processor, a machine instruction is normally implemented by its corresponding microprogram, possibly sharing routines such as instruction fetch. This describes **the control process within an instruction cycle**; it does not mean that a machine instruction or an entire microprogram takes only one clock cycle.

<!-- source: cs408:L35-L35 -->
- A single-cycle processor completes every instruction in one clock cycle, so its CPI is $1$. The clock period must accommodate the complete execution of the slowest instruction. **Single-cycle does not mean single-bus:** a traditional single-bus datapath supports only one bus data transfer at a time, while an instruction often requires several internal transfers, so this organization cannot simply be treated as a single-cycle implementation.

<!-- source: cs408:L48-L48 -->
- If the start and end addresses identify bytes, and the end address is the last byte actually occupied, code length is **end address $-$ start address $+1$**. If the end address instead points immediately past the code, omit the $1$. Convert units if memory is addressed in something other than bytes.

<!-- source: cs408:L17-L17 -->
### Number Representations, Flags, and Alignment {#numbers-flags-alignment}

- With the same number of bits and the conventional excess-code bias of $2^{n-1}$, biased representation and two's complement have the same range: $[-2^{n-1},\,2^{n-1}-1]$. Recalculate the range if a different bias is used.

<!-- source: cs408:L31-L32 -->
- For unsigned comparisons, SF and OF do not directly determine the comparison result; use ZF and CF. Under the convention that subtraction sets CF to $1$ when a borrow occurs, a minuend strictly greater than the subtrahend gives $\mathrm{ZF}=0$ and $\mathrm{CF}=0$, or

  $$
  \overline{\mathrm{ZF}+\mathrm{CF}}=1.
  $$

  Here $+$ means logical OR. This condition expresses unsigned “strictly greater,” not merely “no borrow”: equal operands also produce no borrow, but set ZF to $1$.
- Under natural alignment, a value is normally stored at a byte address that is an integer multiple of its type's size. For example, if the question specifies a $2\,\mathrm{B}$ `short` and a $4\,\mathrm{B}$ `int`, their addresses must be multiples of $2$ and $4$, respectively. Insert padding bytes when necessary. Actual type sizes and alignment rules depend on the question or ABI.

<!-- source: cs408:L30-L30 -->
### Memory Organization, Address Widths, and Cache {#memory-organization-address-widths}

- **The width of the memory address register, MAR, depends on the number of addressable memory units.** Consider both total address space and addressing granularity. For example, a capacity of $128\,\mathrm{KiB}$ addressed in $16$-bit words has $2\,\mathrm{B}$ per addressable unit:

  $$
  \frac{128\,\mathrm{KiB}}{2\,\mathrm{B}}=2^{16}\text{ units}.
  $$

  The MAR therefore needs $16$ bits. The same capacity with byte addressing needs $17$ bits.

<!-- source: cs408:L34-L34 -->
- Distinguish a chip's number of storage locations from its capacity in bytes. An **$8\mathrm{M}\times8$-bit** chip has $8\mathrm{M}=2^{23}$ locations, each $8$ bits wide, requiring $23$ address bits and $8$ data lines. Counting only address and data pins:
  - An SRAM chip without address multiplexing needs $23+8=31$ pins.
  - A DRAM chip that multiplexes row and column addresses over two phases, splitting the address as evenly as possible into $12$ and $11$ bits, needs $\lceil23/2\rceil=12$ address pins plus $8$ data pins, for a total of $20$.

  **Neither figure is the chip's total physical pin count:** control, power, ground, and other pins have not been included. A DRAM chip may also use a different row-column organization. The original `8MB × 8` notation is interpreted here as “$8\mathrm{M}$ locations, each $8$ bits wide.”

<!-- source: cs408:L37-L37 -->
- A main-memory address can be divided into “memory block number $+$ offset within the block.” In a set-associative cache, the block number is further divided into **tag $+$ cache set index**. The block offset is unchanged.

<!-- source: cs408:L19-L19 -->
- A write-through operation must eventually write to main memory. Unless the question says otherwise, count that memory write even if it first enters a write-buffer queue. Whether the CPU must wait is a separate question determined by the buffering and timing assumptions.

<!-- source: cs408:L47-L47 -->
- In the usual textbook model, main memory is implemented using RAM and ROM, and microprogram control memory is typically ROM. Follow the stated design if the question instead discusses writable control memory or another special case.

<!-- source: cs408:L23-L24 -->
### Buses, Memory Cycles, and Interleaving {#buses-memory-cycles-interleaving}

- An asynchronous bus does not use a shared clock to divide communication into fixed transfer beats. Instead, mutually dependent handshake signals coordinate the participants, accommodating devices of different speeds and allocating transfer time as needed.
- On an I/O bus, both data-buffer registers and command/status registers transfer their **contents over data lines**. Address lines identify the port exchanging data with the CPU; read/write control lines request a read or write at that port. Distinguish the command value written into a command register from the bus control signal that requests the write.

<!-- source: cs408:L36-L36 -->
- In asynchronous serial communication, the two clocks need not remain strictly synchronized, so **start and stop bits** mark the beginning and end of each character.

<!-- source: cs408:L38-L43 -->
- The **memory cycle time** is the minimum interval from the start of one access to a memory bank until that bank can begin another independent access. It reflects access and recovery constraints; it cannot generally be assumed to exclude all data-transfer time. Use the question's division between memory-bank activity and bus transfers to avoid double-counting.
- Analyze a bus transaction in stages: send the initial address and command; allow memory to prepare or read the data; then transfer the data over the data bus.
- For interleaved memory, suppose the question assigns one bus cycle to the address and command, time $T$ to preparation in the first bank, pipelined preparation in subsequent banks, and time $T_{\mathrm{bus}}$ to each data-word transfer. Transferring $n$ consecutive words then takes

  $$
  T_{\mathrm{transaction}}=T_{\mathrm{bus}}+T+nT_{\mathrm{bus}}.
  $$

  **The first bank must finish its preparation before the subsequent banks produce data as a pipeline.** Here $n$ counts data words or bus transfers, not individual bits. The formula also assumes that the stages are arranged as in the diagram.

{{< fig src="figures/interleaved-memory-timing.png" alt="Address, preparation, and pipelined-transfer timing for interleaved memory" caption="Address, preparation, and pipelined-transfer timing for interleaved memory" >}}

In the diagram, rows identify memory banks and columns identify time units. Red means sending the initial address, yellow means preparing data, and blue means transferring data. The example occupies $1+8+8=17$ time units.

<!-- source: cs408:L44-L46 -->
- Distinguish two ways to activate low-order interleaved memory:
  - **Staggered activation:** Typically, each bank's data width equals the bus width, and banks start in sequence. With memory cycle time $T$ and bus cycle time $r$, sustaining one data item every $r$ requires $m\geq\lceil T/r\rceil$ banks. If $m=T/r$, the interval between starts is $T/m=r$, or $1/m$ of a memory cycle. Other address-access patterns can still cause bank conflicts.
  - **Simultaneous activation:** Typically, the combined widths of all banks equal the bus width, so they jointly supply one bus-wide value. A transfer may include bytes that the current operation does not need.
- The original simultaneous-activation example specifies “four $64\mathrm{KB}\times4$-bit DRAM chips, interleaved addressing, a maximum memory transfer of $32$ bits, and an $8\,\mathrm{B}$ `double` read starting at an address ending in `AH`.” **The chip-width conditions must be checked against the original question:** $4\times4=16$ bits, not the stated $32$-bit bus width.
- Consider the address example separately under the assumptions of byte addressing and reads of aligned $4\,\mathrm{B}$ blocks. The low two bits of `AH` are `10`; an $8\,\mathrm{B}$ read starting there crosses three aligned blocks. Each group below lists the low two address bits within a block; bold entries are the bytes actually needed:
  - First block: `00`, `01`, **`10`, `11`**.
  - Second block: **`00`, `01`, `10`, `11`**.
  - Third block: **`00`, `01`**, `10`, `11`.

  Thus $8$ bytes are useful, while another $4$ arrive with the blocks but are unused by this operation. This alignment illustration does not resolve the inconsistency in the chip widths above.

<!-- source: cs408:L25-L29 -->
### Stored Programs and Execution Context {#stored-program-execution-context}

- The von Neumann model stores instructions and data in the same logical memory space. Both are represented in binary; their bit patterns do not inherently identify them as instructions or data. The CPU fetches instructions from addresses specified by the PC, then accesses operands using address information in the instruction and other addressing mechanisms. The basic textbook distinction is that **the fetch phase interprets the retrieved content as an instruction, while the execution phase processes data according to that instruction**.

<!-- source: cs408:L33-L33 -->
- Both interrupt handling and subroutine calls must preserve a return location. In the textbook stack model, a return address associated with the PC is pushed so that execution can resume at the interrupted position or return to the caller. Some architectures initially preserve the return address in a dedicated register instead.
- Because an interrupt can arrive asynchronously during program execution, the necessary processor state, including the PSW or equivalent status information, must be preserved so the original environment can be restored. An ordinary subroutine call usually does not automatically change privilege level or interrupt-enable state, and normally does not automatically save the entire PSW. Determine which registers and flags need preservation from the calling convention and architecture.

## Operating Systems {#operating-systems}

<!-- source: cs408:L49-L51 -->
> **Memorize:** The advantages, disadvantages, and suitable applications of different operating systems. Compare single-job processing with batch processing, following the textbook's terminology.

<!-- source: cs408:L52-L55 -->
### Address Translation and Memory Access {#address-translation-memory-access}

- For a process's memory access, first consult the TLB; on a miss, consult the page table in main memory. Whether these lookups may occur simultaneously depends on the question. If the address is valid but its page is absent from main memory, a page fault requires loading the page from secondary storage, updating the page table and relevant TLB state, and restarting the interrupted access. An invalid address cannot automatically be treated as an ordinary page fault that will be satisfied by loading a page.
- If the question's model restarts the TLB lookup after handling the fault, that access involves two TLB lookups. Include the retry in timing calculations. **Completing address translation only produces a physical address; it does not mean that the target data has already been read or written.**
- After obtaining the physical address, consult the cache under the physically addressed cache model used by the question. On a miss, access main memory and fill the cache according to its policy. Follow the stated assumptions for overlapping TLB, cache, and memory access, and for restarting after a page fault.
- A C program passes through preprocessing $\to$ compilation $\to$ assembly $\to$ linking $\to$ loading. The linker combines object modules from different files, resolves symbols, and determines the executable's address layout. The loader places the program in memory and performs the required loading and relocation work. With virtual memory or dynamic relocation, mechanisms such as the MMU still translate logical or virtual addresses into physical addresses at runtime; this work is not all completed during linking or a single loading step.

<!-- source: cs408:L78-L79 -->
- Do not discard a small number of cache or TLB misses when calculating a hit rate. One miss in $1000$ accesses gives $999/1000=99.9\%$, not $100\%$.
- A disk buffer acts like a cache for secondary storage. If the required page or data is already buffered, fewer actual disk I/O operations are needed.

<!-- source: cs408:L56-L56 -->
### Scheduling and Atomic Operations {#scheduling-atomic-operations}

- Entering a critical section does not automatically disable scheduling or preemption. Preemptibility depends on the kernel and synchronization mechanism; certain protected kernel operations may temporarily prevent preemption. For example, some kernel primitives temporarily disable preemption while executing, depending on their implementation. Mutual exclusion on a resource and the ability to reschedule a process are separate properties.

<!-- source: cs408:L71-L74 -->
- Common priority guidelines are: system processes above user processes; interactive processes above noninteractive ones; and I/O-bound processes above CPU-bound ones. Starting I/O early helps overlap device activity with CPU work, and prompt handling of device data can help prevent issues such as particular device buffers overflowing. These are common policies, subject to the system's scheduler and real-time requirements.

<!-- source: cs408:L80-L80 -->
- **An atomic operation is neither simply “disable interrupts, then enable them” nor necessarily a bus lock.** Disabling interrupts can protect certain kernel operations on a single CPU, but disabling interrupts on one core does not stop other CPUs from accessing memory. Hardware atomic instructions often use cache coherence and exclusive access to the relevant cache line; bus locking is used in some circumstances. The common purpose is to keep concurrent accesses from observing the protected operation as separately interleavable steps.

<!-- source: cs408:L57-L57 -->
### Opening Files and Allocating Disk Blocks {#file-opening-allocation}

- `open` opens a path and, on success, returns an **integer file descriptor**. The C library function `fopen` instead returns a **`FILE *` stream pointer**:

  ```c
  int fd = open(pathname, flags);       /* POSIX file descriptor */
  FILE *fp = fopen(pathname, mode);     /* C standard-library stream */
  ```

  Subsequent `read(fd, ...)` calls use the descriptor to access the open file; standard I/O functions such as `fread(..., fp)` use the stream. They normally avoid walking the directory path again for every operation, making access more direct than repeatedly locating and opening the file.

<!-- source: cs408:L75-L77 -->
- When inserting a file record, disk-block access counts depend on the allocation method. Retain the example of inserting at position $20$ among $1000$ records, **assuming one record per block, no cache hits, and no additional allocation-table or directory-metadata accesses**:
  - **Contiguous allocation:** If space is available immediately before the file and its start position may be changed, move the first $19$ records toward lower block addresses instead of shifting the much larger trailing portion toward higher addresses. Each moved block is read once and written once; write the new record into the vacated block. The total is $19\times2+1=39$ accesses. This minimum does not apply if there is no space before the file or its start cannot be changed.
  - **Linked allocation:** Follow links from the beginning, reading the first $19$ blocks. Write a new block whose link points to the former successor of block $19$, then rewrite block $19$ to point to the new block. This requires $19$ reads and $2$ writes, totaling $19+2=21$ accesses.

<!-- source: cs408:L58-L62 -->
### Disk Initialization and System Booting {#disk-initialization-booting}

- Remember these levels of traditional disk preparation before installing an operating system:
  1. **Low-level formatting:** Establish physical sectors and their structures, including headers, data areas, and trailers. Traditional CHS descriptions identify locations using cylinder/track, head, and sector numbers. Modern hard drives are normally low-level formatted at the factory, and hosts generally use LBA.
  2. **Partitioning:** Divide the disk into partitions for the operating system, which might later receive drive letters such as C and D. In the traditional MBR scheme, the partition table is part of the master boot record, **MBR**, and records information such as partition start locations. It is not the PBR. GPT uses a different partition structure.
  3. **Logical or high-level formatting:** Write initial file-system structures, create the root directory, and initialize free-block management information.

<!-- source: cs408:L63-L70 -->
- A traditional **BIOS + MBR** boot sequence can be organized as follows:
  1. After reset, the CPU executes firmware startup code in ROM or flash and begins BIOS initialization.
  2. Firmware performs hardware self-tests and establishes the necessary execution environment. In the traditional real-mode model, this also includes an interrupt vector table in low memory. The entire self-test process should not be reduced to detecting faults through interrupts alone.
  3. BIOS chooses a boot device according to the boot order, loads the relevant boot sector into memory, and transfers control to it.
  4. For a disk using this boot chain, the MBR boot code examines the partition table, locates the active partition, and loads that partition's boot sector.
  5. The active partition's first sector is the partition boot record, **PBR**. Its code finds and loads the next-stage boot program or operating-system boot manager. Some textbook examples place the relevant files in the partition's root directory.
  6. The boot program or manager continues loading the operating-system kernel and related components into RAM, then starts the operating system.
- Distinguish firmware from working memory: ROM or flash holds the firmware startup code that initializes hardware and begins the boot chain. RAM holds the main working data, program contents, and file caches used by the running operating system. Systems using UEFI, GPT, or other arrangements need not follow this traditional chain exactly.

## Computer Networks {#computer-networks}

<!-- source: cs408:L81-L84 -->
> **Memorize:**
>
> 1. The 802.11 data-frame format and address meanings involving an AP; CSMA/CA interframe spaces, channel reservation, and NAV values; DHCP message types, ICMP message types, OSPF packet types, and BGP message types.
> 2. The PPP frame format and the basic concepts of SDN.

<!-- source: cs408:L85-L85 -->
### Ethernet Frames, Link Services, and Switching {#ethernet-link-services-switching}

- A standard Ethernet MAC frame without a VLAN tag is between $64\,\mathrm{B}$ and $1518\,\mathrm{B}$, measured from the destination address through the FCS. Its data field is $46$ to $1500\,\mathrm{B}$, and the remaining fields total $18\,\mathrm{B}$. These lengths exclude the preamble, start-of-frame delimiter, and interframe gap.

<!-- source: cs408:L95-L98 -->
- **The data-link layer can provide either reliable or unreliable service**, depending on the protocol and operating mode:
  - Typical Ethernet MAC service is connectionless and unreliable. CRC detects errors, corrupt frames are discarded, and the MAC layer does not acknowledge and retransmit them.
  - Certain HDLC modes with sequence numbers, acknowledgments, and ARQ can provide reliable link transfer. **Standard PPP itself does not provide this acknowledgment-and-retransmission reliability**, so it should not be grouped with HDLC's reliable modes.
  - The 802.11 wireless MAC uses frame-level ACKs and retransmissions for ordinary unicast frames, improving link reliability. Finite retries do not guarantee delivery, nor do they provide end-to-end reliability.

<!-- source: cs408:L99-L101 -->
- Two Ethernet switch forwarding methods:
  - **Cut-through:** Begin forwarding after enough header information has arrived, reducing latency but preventing full-frame FCS validation before forwarding begins. Textbook estimates may assume the $6\,\mathrm{B}$ destination MAC address is enough to select the basic forwarding direction. This does not mean the switch transfers or processes only those $6\,\mathrm{B}$; an actual device may require more header information.
  - **Store-and-forward:** Buffer and check the complete frame before forwarding. This introduces a longer initial wait but allows corrupt frames to be filtered. For the shortest standard Ethernet frame, at least $64\,\mathrm{B}$ must arrive. Error detection alone does not supply retransmission or guaranteed delivery.

<!-- source: cs408:L86-L86 -->
### Symbols, Channel Capacity, and Propagation Delay {#symbols-capacity-propagation}

- If the baud rate is $B$ and each symbol can take $N$ discrete states, the data rate under the corresponding coding assumptions is

  $$
  C=B\log_2N.
  $$

  Each symbol carries $\log_2N$ bits of information. The $N$ states can be viewed as a base-$N$ symbol alphabet; distinguish the number of possible states from the number of symbols sent per unit time.

<!-- source: cs408:L104-L104 -->
- **Shannon's theorem** gives the capacity limit of a noisy channel:

  $$
  C=W\log_2(1+S/N).
  $$

  $W$ is bandwidth, and $S/N$ is the linear ratio of signal power to noise power. In decibels it is $10\log_{10}(S/N)$; for example, $S/N=1000$ corresponds to $30\,\mathrm{dB}$. Use the linear ratio $1000$, not the decibel value $30$, in the capacity formula.
- **Nyquist's theorem**, for an ideal noiseless channel, gives

  $$
  C=2W\log_2V,
  $$

  where $V$ is the number of possible symbol states. Distinguish **transmission rate** from **propagation speed**: the former is normally measured in bit/s and determines how long sending the data takes; the latter is normally measured in m/s and determines how long a signal takes to cross a distance.

<!-- source: cs408:L110-L110 -->
- In the half-duplex CSMA/CD model, $\tau$ is the one-way propagation time between the most distant stations. Collision detection must allow for the round trip, so analyze the contention interval as $2\tau$. The minimum frame's transmission time must cover the worst-case round-trip collision-detection time. This is not a general “minimum RTT” for TCP or other protocols.
- For traditional half-duplex 100BASE-T Ethernet at $100\,\mathrm{Mbit/s}$, the $64\,\mathrm{B}$ minimum frame corresponds to a slot time of

  $$
  \frac{64\times8}{100\times10^6}=5.12\,\mu\mathrm{s}.
  $$

  If intermediate equipment contributes an additional **one-way** delay of $\Delta x\,\mu\mathrm{s}$, the one-way cable-propagation budget is $5.12/2-\Delta x\,\mu\mathrm{s}$. Multiply it by propagation speed to obtain the distance limit. Equipment delay consumes the available propagation budget; it does not shorten the protocol's slot time. If the specified equipment delay is instead a round-trip value, subtract it from $5.12\,\mu\mathrm{s}$ before dividing by $2$. **The minimum frame covers a round trip, so convert to a one-way delay before calculating distance.**

<!-- source: cs408:L105-L109 -->
### ARQ Windows and Piggybacked Acknowledgments {#arq-windows-piggybacked-acks}

- Stop-and-wait, S-W, is a single-frame sliding-window protocol: both the sending and receiving windows have size $1$.
- Go-Back-N, GBN: with an $n$-bit sequence number, the receiving window always has size $1$. The usual multi-frame sending window satisfies $1\lt W_T\leq2^n-1$.
- Selective Repeat, SR: let the sending and receiving window sizes be $W_T$ and $W_R$. With $n$-bit sequence numbers, the common window-constraint model gives

  $$
  1\lt W_T,\qquad1\lt W_R,\qquad W_T+W_R\leq2^n.
  $$

  Usually $W_T=W_R$. **With this equality assumed**, the constraint implies $W_T=W_R\leq2^{n-1}$.
- Identify each symbol's meaning when calculating channel utilization. In some formulas, the numerator's $n$ counts **packets sent within a window**, which equals $W_T$ when the window is full. It is not the sequence-number field's bit width. Include the packet length, transmission time, and acknowledgment delay as required.

<!-- source: cs408:L115-L115 -->
- A GBN receiver accepts data frames only in order. Write a frame with a piggybacked acknowledgment as $R_{x,y}$, where $x$ is the frame's own sequence number and $y$ is the next sequence number expected from its peer. Suppose the receiver initially expects frame $2$ and accepts $R_{2,2}$ in order, then $R_{4,3}$ arrives, but the peer's frame $3$ has not yet been received in order. The acknowledgment must still request frame $3$, not frame $5$ merely because frame $4$ was seen. If the next outgoing frame's sequence number is $3$, send **$R_{3,3}$**.

<!-- source: cs408:L91-L94 -->
### IPv4 Fields and Routing {#ipv4-fields-routing}

- Analyze changes to IPv4 fields according to the actual operation:
  - Outbound source NAT changes the source IP address when that translation actually occurs. Not all private-network traffic must pass through NAT. Other translation directions or types may change the destination address.
  - Ordinary router forwarding decrements TTL and updates the IPv4 header checksum.
  - Different link MTUs may cause an IPv4 datagram that permits fragmentation to be fragmented, changing the individual fragments' total lengths, flags, and fragment offsets. **All fragments of the same original datagram retain the same identification value**; identification does not normally change at every fragmentation step.

<!-- source: cs408:L102-L103 -->
- A routing table can contain both `128.20.96.0/23` and `128.20.96.128/25`. Forwarding uses **longest-prefix matching**: if a destination matches both entries, the more specific `/25` entry wins.
- Under the protocol-layer classification commonly used in 408 questions, RIP runs over UDP and is usually classified as an application-layer protocol; OSPF runs directly over IP and is classified at the network layer; BGP runs over TCP and is usually classified at the application layer. All three exchange information used for routing.

<!-- source: cs408:L87-L90 -->
### HTTP Round Trips and DNS Caches {#http-round-trips-dns-cache}

- For HTTP RTT calculations, first establish whether a TCP connection already exists. In the common simplified model, ignore DNS, TLS, data-transmission time, and server processing time. After sending SYN, the client receives SYN+ACK at about $1$ RTT. The third handshake ACK takes another half RTT to reach the server and may carry the HTTP request. The first response then takes half an RTT to return, giving about **$2$ RTTs** from a new connection's start to its first response. Counting only until the server receives the third handshake message gives $1.5$ RTTs.
- Inspect connection state using the surrounding exchange as well as individual segment flags. SYN is used to establish connections, but **SYN being unset does not by itself prove that a connection is already established**. Over an established, usable TCP connection, one request/response exchange takes about $1$ RTT under the simplified model.
- In HTTP/1.0's typical nonpersistent-connection model, each object needs a new TCP connection and about $2$ RTTs: $1$ RTT for connection setup and $1$ RTT for the request and first response.
- HTTP/1.1 persistent connections reuse an established TCP connection. Without pipelining, requesting $k$ objects generally takes $k$ request/response RTTs. With pipelining, a batch of known requests can be sent consecutively, and an idealized exam model may count $1$ RTT of round-trip overhead for that batch. Still count initial connection setup, object transmission time, and other specified costs separately.

<!-- source: cs408:L114-L114 -->
- **DNS servers also cache information.** Valid cached records or delegation information can reduce subsequent queries; a lookup need not begin at a root name server every time.

<!-- source: cs408:L111-L113 -->
### TCP Sequence Numbers and the Congestion Window {#tcp-sequence-numbers-congestion-window}

- TCP sequence numbers count **bytes**, not segments. For example, under the exam convention $1\,\mathrm{KB}=1024\,\mathrm{B}$, an MSS of $1\,\mathrm{KB}$ means sending $2$ MSS, or $2\,\mathrm{KB}$, advances the next data sequence number by $2048$. SYN and FIN each consume one position in sequence space; include them separately when relevant.
- Do not memorize congestion-window growth as an unconditional “add one for every ACK.” In classic TCP **slow start**, an ACK acknowledging new data normally increases the window by roughly $1$ MSS; under ideal conditions, the window approximately doubles per RTT. In **congestion avoidance**, the increase per ACK is approximately $\mathrm{MSS}^2/\mathrm{cwnd}$, totaling about $1$ MSS per RTT. Follow the question's congestion-control algorithm, ACK behavior, and window units.
