---
title: "Graph-based Agent Memory: Reading Notes"
date: 2026-10-20T00:00:00+08:00
lastmod: 2026-10-22T22:36:04+08:00
description: "The agent memory lifecycle and taxonomy, with graph-based memory storage, retrieval, and continual evolution."
draft: false
---

<style>
.prose mjx-container[display="true"] { max-width: 100%; overflow-x: auto; overflow-y: hidden; padding: .4em 0; }
</style>

## The Agent Memory Lifecycle

The lifecycle consists of four stages: memory extraction, storage, retrieval, and evolution.

1. Memory extraction: transforming raw, unstructured observations $o_t$ into structured memory units $m$.
2. Memory storage: indexing and organizing the extracted units within a memory structure $\mathcal{M}$.
3. Memory retrieval: retrieving relevant stored information $\mathcal{M}_{\text{rel}}\subset\mathcal{M}$ in response to a query $q$.
4. Memory evolution: refining memory through post-processing, including internal self-evolution, such as consolidation and abstraction, and external self-exploration, such as new environmental feedback.

<figure class="fig">
  <img src="/blog/graph-based-agent-memory/figures/F001.png" width="2590" height="910" alt="Figure 1. Workflow of an AI agent system and detailed implementations of the memory system." loading="lazy" decoding="async">
  <figcaption>Figure 1. Workflow of an AI agent system and detailed implementations of the memory system.</figcaption>
</figure>

## A Multidimensional Taxonomy of Agent Memory

Agent memory can be classified along several dimensions, including temporal scope, functional role, and representational structure.

### Temporal Scope

- **Short-term memory**: retains recent, immediately relevant information, offering fast access but limited capacity. It includes the current conversation context, ongoing reasoning traces, and transient state variables. It is usually discarded once the immediate task ends, although important elements may be consolidated into long-term storage.
- **Long-term memory**: persists across sessions, including accumulated knowledge, past interactions, learned patterns, and user preferences. It supports continuity across interaction episodes, transfer learning, and personalized, adaptive behavior over longer periods.

### Cognitive Structure of Memory

- **Semantic memory**: stores general, context-independent world knowledge and factual information, such as "Paris is the capital of France." In a graph structure, it forms a stable ontological layer. Common sense and domain-specific facts provide a reliable foundation for agent reasoning.
- **Procedural memory**: encodes skills, routines, and fixed rules, representing how-to knowledge such as standard operating procedures or game rules. In practice, this allows agents to execute complex, established tasks automatically under standard conditions.
- **Associative memory**: establishes latent connections between concepts or data points within a knowledge base. Connecting related information, such as symptoms and diagnoses or products and categories, supports creative reasoning and analogy. It serves as connective tissue within a knowledge graph.
- **Working memory**: acts as the agent's mental scratchpad for processing immediate experience. It temporarily holds the current dialogue turn, intermediate reasoning steps, and transient variables. Despite its short lifespan, it is the entry point for all experience memory. It offers very fast access and directly shapes the agent's next action.
- **Episodic memory**: records past interaction sessions in chronological order, turning transient working memory into a persistent, queryable autobiographical history. For example, it may record that a user requested vegetarian options or cancelled an order. It lets the agent recall what happened and when, providing explicit temporal anchors.
- **Affective memory**: captures emotional tone or sentiment arising during interactions. Recording user feedback or frustration allows the agent to adjust its empathy and communication style in later turns. This adds a qualitative emotional dimension to raw interaction logs.

### Two Main Types

Static knowledge memory and dynamic experience memory work together to support understanding the world's rules and adapting to personalized human–agent interactions.

- **Knowledge memory (passive and static)**: is the agent's passive, static repository of objective, global, verifiable information. It resembles an internal reference library or textbook, containing canonical facts, general rules, established procedures, and general truths about the world. This memory is usually preloaded, rarely updated, and independent of specific contexts. Its purpose is to provide a stable, reliable foundation for reasoning and action. Examples include factual definitions, fixed game rules, and standard scientific principles. A shopping agent's product database or a robot's general affordance model also falls into this category. Overall, it acts as a passive, reliable factual backbone for reasoning.
- **Experience memory (proactive and dynamic)**: is the agent's personal journal, actively recording its interactions, environmental observations, and action outcomes. It includes conversation histories, execution logs, trial-and-error trajectories, and feedback signals. For example, it may record a particular user's request for vegetarian lasagna, a loss from a mean-reversion trade, or a failed grasp on a wet mug. This memory is dynamic and highly personalized, supporting learning from practice and adapting to particular situations.

<figure class="fig">
  <img src="/blog/graph-based-agent-memory/figures/F002.png" width="2590" height="1170" alt="Figure 2. Knowledge memory and experience memory, with applications across agent scenarios. Static knowledge and dynamic experience together support understanding the world&#x27;s rules and adapting to personal interactions." loading="lazy" decoding="async">
  <figcaption>Figure 2. Knowledge memory and experience memory, with applications across agent scenarios. Static knowledge and dynamic experience together support understanding the world&#x27;s rules and adapting to personal interactions.</figcaption>
</figure>

### Unstructured and Structured Memory

Traditional agent memory systems often use simplified storage paradigms, mainly including:

- **Linear or buffer-based memory**: fixed-length token windows or conversation histories retain recent interactions, but lose information and lack relational context.
- **Vector-based memory**: encodes interactions as dense embeddings in a vector database, supporting efficient similarity-based retrieval. However, it has limitations in structured reasoning and modeling hierarchical relationships.
- **Key–value or log-based memory**: records events in temporal logs or attribute–value pairs, supporting direct lookup. Its expressiveness is limited for complex queries and dynamic updates.

## Memory Storage

<figure class="fig">
  <img src="/blog/graph-based-agent-memory/figures/F006.png" width="2590" height="1055" alt="Figure 3. Taxonomy of graph construction paradigms for agent memory, comparing construction methods, corresponding memory functions, advantages, and limitations." loading="lazy" decoding="async">
  <figcaption>Figure 3. Taxonomy of graph construction paradigms for agent memory, comparing construction methods, corresponding memory functions, advantages, and limitations.</figcaption>
</figure>

### Knowledge Graph Structures

#### Knowledge Graph Modeling

Building a knowledge graph for an agent involves continuously extracting and integrating structured triples from unstructured interaction streams. Mainstream approaches rely heavily on large language models (LLMs) as open-vocabulary parsing engines.

In the AriGraph world model, an LLM parses each textual observation from the environment, identifies relevant objects, and extracts their relationships as triples. Similarly, systems such as Mem0 transform conversational messages into entity–relation–entity triples during extraction. This offers substantial advantages over traditional named entity recognition (NER) and relation-classification pipelines. LLMs can generalize to unseen, fine-grained entity types and uncover implicit semantic relationships through deeper contextual understanding.

#### Corresponding Memory Types

The triple-based structure of knowledge graphs is particularly suitable for long-term, static memory. This includes general world knowledge, such as _(Paris, capital, France)_, and fixed domain-specific concepts. Explicit relations support efficient storage and complex multi-hop queries, which are difficult for vector-based retrievers. Adding temporal metadata to triples or linking them to episodic event nodes also supports episodic memory. For example, an episodic vertex can connect to triples extracted from the same observation.

Knowledge graphs also support dynamic construction and modification, since knowledge, manuals, and similar information can change.

### Hierarchical Memory Structures

Hierarchical memory organizes information into a multilevel tree, compressing large collections of experience into readily accessible knowledge schemas. It supports top-down traversal to retrieve abstract topics and bottom-up aggregation of summaries to maintain coherence across a large memory repository.

Systems such as MemTree dynamically route new information through the tree. They cluster similar content under existing nodes, create new branches for novel information, and recursively update ancestor summaries to reflect newly absorbed knowledge.

Construction relies on two core processes: **semantic clustering** for organization and **recursive summarization** for conceptual abstraction. To preserve the hierarchy's value as a compressed knowledge schema, each parent dynamically generates a concise natural-language summary of all information in its subtree.

Beyond static factual knowledge, this structure can also represent conversational or sequential memory. Individual statements or dialogue turns are linked primarily by their order within a session or by the conversational context. This forms a timeline or narrative chain, supporting dialogue coherence and tracking changes in user intent within an interaction episode.

### Temporal Graph Structures

#### Modeling Two Temporal Dimensions

A key distinction is between when an event actually occurred (_valid time_) and when the system recorded it (_transaction time_). **Graphiti** implements a bi-temporal model that tracks both dimensions simultaneously.

Relative expressions such as "last Friday" can easily become ambiguous across conversation sessions. **TReMu** uses time-aware memory to separate mention time, the session timestamp, from the inferred time at which the event occurred.

#### Hierarchical Temporal Constraints

**MemoTime** aims to prevent temporal inconsistencies in reasoning chains, such as retrieving an effect that occurred before its cause. It organizes temporal knowledge graph (TKG) reasoning into a hierarchical **Tree of Time**. Any retrieved reasoning path $e_1\to e_2\to e_3$ must respect chronological order, $t_1\leq t_2\leq t_3$. This operator-aware structure prunes evidence that appears semantically relevant but is temporally invalid.

| Date | Event |
| --- | --- |
| October 14 | Tests pass in the old environment |
| October 15 | Python is upgraded |
| October 16 | Dependency issues following the upgrade are fixed |
| October 17 | Tests pass in the new environment |

The subquestion tree, or Tree of Time generated by an LLM, can be understood as follows:

```text
Main question: After upgrading Python and fixing dependencies, when did tests first pass?
├─ Subquestion 1: When was Python upgraded? → October 15
├─ Subquestion 2: When were the post-upgrade dependency issues fixed? → October 16
└─ Subquestion 3: What is the earliest passing test record after the fix? → October 17
```

The time found in one step becomes a retrieval constraint for the next:

```text
Find the upgrade date
→ Restrict dependency-fix records to those after the upgrade
→ Restrict test records to those after the fix
→ Select the earliest qualifying record
```

This is a **planning tree constructed temporarily for the current question**, distinct from the memory graph that stores historical knowledge.

### Hypergraph Structures

Binary graphs connecting two entities are efficient, but lose information when representing complex interactions among several entities. An example is three drugs acting together to cause a particular side effect. Hypergraphs address this through hyperedges that connect any number of nodes, preserving the integrity of $n$-ary relations.

#### Information Integrity in N-ary Relations

Hypergraph memory aims to avoid the sparsity and semantic fragmentation caused by decomposing complex facts into multiple binary edges. **HyperGraphRAG** demonstrates, from an information-theoretic perspective, that hypergraph representations preserve more complete information than equivalent binary graphs.

A natural-language knowledge fragment and all its associated entities are represented as one hyperedge, $e_i=(e_i^{\text{text}},\{v_1,\dots,v_n\})$. This supports **dual retrieval**, retrieving both relevant entities and the hyperedges that bind them together. The result provides complete factual context for downstream generation.

#### Structural Dependencies in Tabular Data

Hypergraphs are particularly effective for structured tabular data whose relationships are inherently group-based. **HyperG** models tabular knowledge using hypergraphs, constructing separate hyperedges for rows, columns, and the entire table. These capture higher-order dependencies, including semantic consistency within columns and hierarchical relationships between table captions and data cells.

### Hybrid Graph Architectures

Graph structures excel in precision and multi-hop reasoning, but may lack the breadth of vector retrieval or the flexibility of unstructured buffers. Hybrid architectures integrate graphs with other data structures to balance their strengths and limitations. A common design explicitly separates static knowledge from dynamic experience.

##### External Graphs with Internal Working Memory

One hybrid approach focuses on interaction between a large external knowledge graph and a lightweight internal working state. **KG-Agent** combines an external knowledge graph with internal _Knowledge Memory_. Unlike purely graph-based agents, it maintains a structured scratchpad, iteratively updating reasoning history, tool definitions, and intermediate observations retrieved from the external graph. This design lets the agent navigate large static graphs using a flexible, evolving internal context, connecting symbolic storage with neural reasoning.

### Summary

Different memory types often require different graph construction methods; no single approach fits every scenario. Building graph-based memory therefore requires careful design around the characteristics of each memory type.

Knowledge memory is usually stable, structured, and context-independent, making it well suited to graphs emphasizing relations or hierarchies, such as knowledge graphs. These capture explicit relationships between entities and support efficient reasoning over general information.

Experience memory is dynamic, personalized, and context-specific. It therefore often benefits from graphs emphasizing temporal sequences, interaction trajectories, or user–action relationships. Such graphs record how the agent's interactions and preferences evolve over time.

## Memory Retrieval

### Similarity-Based Retrieval Operators

Similarity-based retrieval is a coarse-grained operator that encodes a user query as a vector. It retrieves the top-$k$ most similar memory entries in the embedding space.

Direct vector similarity search often works well in relatively simple applications. It can also serve a supporting role, such as filtering irrelevant memories through query–query similarity.

However, it has several limitations: **lexical or semantic similarity does not necessarily imply relevance; complex queries often depend on unmentioned related entities and require multiple retrieval steps; and temporal awareness is lacking**.

### Graph- and Rule-Based Retrieval Operators

Rule-based operators in graph memory use symbolic rules and filters to identify relevant memories.

For knowledge memory, they can select candidates in the initial retrieval stage for downstream processing. They can also validate retrieved candidates and remove memories that violate the rules. Experience memory is inherently noisy and continually evolving. These operators can restrict candidates to compatible entity or relation types and prioritize high-confidence triples. Two common advanced implementations are outlined below.

#### The Retrieval Process

Consider a memory graph $G_t=(V_t,E_t,X_t)$. The node set $V_t$ contains entities, events, or text chunks, while $E_t\subseteq V_t\times V_t$ is the set of relational edges. Node and edge attributes or features are represented by $X_t$, including textual descriptions and vector embeddings $\mathbf{h}_i\in\mathbb{R}^d$. Given a user query $q$, the operator aims to return **a compact subgraph $\mathcal{S}_t\subseteq G_t$ that is highly relevant to the task**.

1. Identify key entities as anchors, using NER or similarity retrieval to generate candidate anchors:

   $$
   \begin{aligned}
   V_{\text{anchor}} &= \text{TopK}_{v_i \in V_t} \big( \text{sim}(\mathbf{q}, \mathbf{h}_i) \big),\\
   \text{where } \text{sim}(\mathbf{q}, \mathbf{h}_i) &= \frac{\mathbf{q}^\top \mathbf{h}_i}{\Vert{}\mathbf{q}\Vert{} \Vert{}\mathbf{h}_i\Vert{}}
   \end{aligned}
   $$
2. Expand from the anchor set, moving outward by $n$ steps. This can use breadth-first search (BFS) or relation-constrained traversal that selects only relevant neighboring nodes.
3. Score and prune nodes or paths; the implementation details are not discussed here.
4. Cross-graph traversal changes the procedure in step 2. G-Memory uses bidirectional traversal across three layers: an insight graph at the top, a query graph in the middle, and an interaction graph below. It first retrieves candidates from the middle layer by similarity. It then searches upward for similar tasks and traverses downward to find the corresponding original interaction graph.

### Reinforcement Learning-Based Retrieval Operators

Reinforcement learning (RL) is increasingly used to train operators that autonomously learn adaptive retrieval policies by optimizing downstream task rewards defined over the underlying memory.

Knowledge memory provides a stable foundation and hard constraints for RL operators, while experience memory supports context-aware adaptation. This means recalling and ranking historical state–action–reward fragments that match the current state, adapting retrieval actions to the agent's current task state. Both aim to retrieve consistently useful, high-value evidence while suppressing semantically similar but systematically misleading candidates.

A common implementation augments embedding-based candidate generation with a learned action-value function $Q(s,a)$. The state $s$ summarizes the current query and, optionally, the agent's internal state. The action $a$ represents a retrieval decision, such as selecting a memory unit, choosing a graph navigation step, or invoking an external tool. Some recent work formulates memory retrieval as sequential decision-making and trains dedicated memory agents using policy optimization algorithms such as PPO/GRPO.

### Agent-Based Retrieval Operators

Agent-based operators treat retrieval as an open planning–feedback loop in which an agent can act beyond its internal memory graph. The key difference from RL-based approaches is the ability to call external tools and APIs to supplement internal memory. Verified results can then be written back for reuse. Retrieval therefore extends beyond passive filtering within a static, closed knowledge base.

For knowledge memory, the agent plans graph traversals to gather supporting facts. Traversal finds reasoning chains in structured memory. When graph coverage is incomplete, the agent can use the model's parametric knowledge for supplementary checking.

For experience memory, the agent follows action chains across interaction episodes to recover situational context, rather than traversing a static semantic graph. It reconstructs event sequences from timestamped logs to guide current decisions, rather than retrieving through structured semantic relations.

## Memory Evolution and Continual Learning

As agents interact with dynamic environments over long periods, their internal memory must evolve. It needs to absorb new information, resolve factual inconsistencies, and adapt to changing contexts. Graph-based memory naturally supports these needs through explicit representations of relationships, temporal dependencies, and factual validity. Direct operations on nodes, edges, and subgraphs allow flexible updates.

Memory evolution can be divided into two complementary paradigms. **Internal self-evolution** maintains logical consistency through intrinsic graph operations without external environmental input. **External self-exploration** grounds and refines memory through active interaction with physical or digital environments. These mechanisms turn passive knowledge repositories into adaptive systems capable of proactive learning.

<figure class="fig">
  <img src="/blog/graph-based-agent-memory/figures/memory-evolution.png" width="2590" height="815" alt="Figure 4. Memory evolution mechanisms. Left: internal self-evolution through memory consolidation, graph reasoning, and reorganization. Right: external self-exploration through environmental interaction, including feedback-driven adaptation and active inquiry." loading="lazy" decoding="async">
  <figcaption>Figure 4. Memory evolution mechanisms. Left: internal self-evolution through memory consolidation, graph reasoning, and reorganization. Right: external self-exploration through environmental interaction, including feedback-driven adaptation and active inquiry.</figcaption>
</figure>

### Internal Self-Evolution

During interaction, agents accumulate large amounts of experience memory as concrete interaction trajectories and execution logs. Current mainstream approaches include **generalizing memories into skills or facts through graph merging to create standardized representations, and identifying implicit contradictions through internal reasoning, such as logical deduction or graph traversal**.

#### Graph Reasoning

Beyond consolidating existing nodes, internal evolution discovers latent connections to address memory sparsity. The agent scans its memory graph for missing edges or new relationships that were never explicitly observed but can be deduced from existing facts. This transforms isolated, sparse trajectories into a densely connected knowledge network.

**Latent Link Prediction**: using an LLM's semantic understanding, the agent can predict relationships between disconnected subgraphs. For example, given $(A\xrightarrow{cause}B)$ and $(B\xrightarrow{cause}C)$, it can infer and insert a transitive edge $(A\xrightarrow{leads\_to}C)$. This computationally models associative thinking, linking events separated in time without new external input.

**Inductive Knowledge Enrichment**: the agent can derive new attributes or facts through path-based reasoning. Multi-hop traversal identifies higher-order association patterns, enriching the graph with inferred covariates or higher-order relationships.

### External Self-Exploration

Internal self-evolution can refine memory through internal consistency, but it cannot directly verify the *objective validity* of knowledge against the real world.

#### Feedback-Driven Adaptation

This approach optimizes memory content and management policies using the actual outcomes of past actions. Raw interaction logs in open, dynamic environments often contain substantial noise. Agents therefore use task success and failure as strong supervisory signals to extract useful action knowledge.

Rather than relying on rigid, hardcoded rules, systems such as Memory-R1 and Inside Out treat memory operations as learnable discrete actions within an RL framework. These operations include insertion, modification, and deletion. End-to-end rewards reflecting downstream task accuracy allow the agent to learn optimal policies for maintaining graph health.

#### Active Inquiry

Recent frameworks allow agents to identify uncertainty in their memory graphs and set goals autonomously to resolve these blind spots. ProMem supports reflection on missing nodes or ambiguous edges, generating targeted queries to fill knowledge gaps. AgentEvolver extends this capability to task generation. It synthesizes new subtasks that encourage exploration of unknown states, adding diverse experience to memory proactively.

KIMI K2.5 introduces a scalable swarm mechanism, spawning parallel subagents to explore disjoint branches of a problem space. The central memory repository can rapidly gather diverse perspectives and edge cases. This transforms the system from a passive container of experience into an active engine for knowledge construction.

Despite progress in feedback-driven adaptation and active inquiry, explicitly using graph topology to guide external exploration remains an open research direction.

Paper and figures: [Graph-based Agent Memory: Taxonomy, Techniques, and Applications](https://arxiv.org/abs/2602.05665v1). Figures 1–4 follow the order in the note and correspond to Figures 1, 2, 6, and 8 in the paper. Captions are adapted from the corresponding paper descriptions.
