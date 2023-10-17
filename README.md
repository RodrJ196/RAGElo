# RAGeval


<img  align="left" src="RAGEval_logo_DallE.png" width="200">
<p  align="center" ><em> For when you are too lazy to annotate everything manually</em></p>


---

**Documentation**: soon

**Source Code**: [github.com/zetaalphavector/auto-eval](github.com/zetaalphavector/auto-eval)

---

RAGeval is a toolkit for evaluating answers generated by LLM-based agents using Retrieval Augmented Generation (RAG), based on the Elo ranking system.


The main features are:

- **Easy**: A simple and pretty CLI for comparing answers from multiple agents and annotating them.
- **Modular**: Easily change Evaluators, Agents and the ranking system itself (TODO)
- **Agnostic**: Evaluate answers generated with any agent, chain, model or LLM.

What RAGeval is **not**:

- A RAG system itself. It does not generate answers, it only evaluates them.
- A substitute for human evaluation. It is meant to be used as a tool to help with the annotation process, not to replace it.

## Requirements

Python 3.10+

RAGeval is built with [Typer](https://github.com/tiangolo/typer) for pretty CLIs.

## Installation

```bash
pip install rageval
```

## Naming

RAGeval deals with the following entities:

- **Agent**: A RAG system capable of generating answers for a query based on a list of documents retrieved by a search system.
- **Query**: A query or question answered by the *Agent*.
- **Document**: A passage or document that was used by the RAG system to generate an answer for the *query*.
- **Answer**: The answer generated by an *agent* for the *query* based on a set of *documents*. It should cite the *documents* used to generate it using square brackets (`[]`).
- **Evaluator**: The LLM to be used when evaluating the *answers* generated by the *agent*. It can be also used to annotate individual documents for relevance. It contains a *prompt* to be used when evaluating the *answers*.

## Example

For evaluating a set of answers, we can use the `run-all` command like this:
```bash
$ python -m rageval run-all queries.csv documents.csv answers.csv

---------- Agent Scores by Elo ranking ----------
 agent1        : 1026.7
 agent2        : 973.3
```

The queries.csv file has the following format:
```csv
query_id,query
0, What is the capital of Brazil?
1, What is the capital of France?
```
The documents.csv file has the following format:
```csv
query_id,doc_id,document_text
0,0, Brasília is the capital of Brazil.
0,1, Rio de Janeiro used to be the capital of Brazil.
1,2, Paris is the capital of France.
1,3, Lyon is the second largest city in France.
```

The answers.csv file has the following format:
```csv
query_id,agent,answer
0, agent1, Brasília is the capital of Brazil, according to [0].
0, agent2, Accodring to [1], Rio de Janeiro used to be the capital of Brazil, until 1960."
1, agent1, Paris is the capital of France, according to [2].
1, agent2, Lyon is the second largest city in France, according to [3].
```


### Individual Commands
It is also possible to run just either of the Evaluators individually. To do so, we use the `annotate-documents`, `annotate-answers` and `rank-agents` commands:

```bash
$ python -m rageval annotate-documents queries.csv documents.csv reasoner reasonings.csv 
```

```bash
$ python -m rageval annotate-answers queries.csv answers.csv reasonings.csv answers_eval.jsonl
```

```bash
$ python -m rageval rank-agents answers_eval.jsonl 
```

For additional information on the commands, use the `--help` flag.
---
The RAGeval logo was created using Dall-E 3 with the following prompt:
```
Vector design of a minimalistic command line interface (CLI) window. Inside the window, there's a concise Python code snippet symbolizing retrieval and augmentation. Above the window, a single performance bar glows in green, indicating success.

```
### TODO
- [ ] Publish on PyPi
- [ ] Add custom types
- [ ] Add more document evaluators (Microsoft)
- [x] Split Elo evaluator
- [x] Install as standalone CLI
