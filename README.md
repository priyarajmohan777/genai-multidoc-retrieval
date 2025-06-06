## Design and Implementation of a Multidocument Retrieval Agent Using LlamaIndex

### AIM:
To design and implement a multidocument retrieval agent using LlamaIndex to extract and synthesize information from multiple research articles, and to evaluate its performance by testing it with diverse queries, analyzing its ability to deliver concise, relevant, and accurate responses.

### PROBLEM STATEMENT:
Accessing and synthesizing information from multiple documents is crucial for research, but manual analysis is time-consuming. A multidocument retrieval agent can automate this process by:

Parsing and indexing multiple research articles.
Enabling users to ask queries in natural language.
Providing synthesized, concise, and accurate responses from the indexed documents.
The effectiveness of the system will be evaluated through diverse queries to test its accuracy and relevance.

### DESIGN STEPS:

#### STEP 1:

Use LlamaIndex's document loaders to read and parse multiple research articles in PDF or text format.

#### STEP 2: Create a Unified Index
Combine and index content from all documents using LlamaIndex to enable cross-document retrieval.

#### STEP 3: Set Up a Query Engine
Configure a query engine to allow natural language questions and retrieve relevant content.

#### STEP 4: Implement the Retrieval Agent
Build a retrieval agent that extracts and synthesizes information from the index.

#### STEP 5: Evaluate the Agent
Test the agent with diverse queries to evaluate the quality of its responses.

### PROGRAM:

```python
urls = [
    "https://openreview.net/pdf?id=VtmBAGCN7o",
    "https://openreview.net/pdf?id=6PmJoRfdaK",
    "https://openreview.net/pdf?id=hSyW5go0v8",
]

papers = [
    "metagpt.pdf",
    "longlora.pdf",
    "selfrag.pdf",
]

from utils import get_doc_tools
from pathlib import Path

paper_to_tools_dict = {}
for paper in papers:
    print(f"Getting tools for paper: {paper}")
    vector_tool, summary_tool = get_doc_tools(paper, Path(paper).stem)
    paper_to_tools_dict[paper] = [vector_tool, summary_tool]

initial_tools = [t for paper in papers for t in paper_to_tools_dict[paper]]

from llama_index.llms.openai import OpenAI
llm = OpenAI(model="gpt-3.5-turbo")

from llama_index.core.agent import FunctionCallingAgentWorker
from llama_index.core.agent import AgentRunner

agent_worker = FunctionCallingAgentWorker.from_tools(
    initial_tools, 
    llm=llm, 
    verbose=True
)
agent = AgentRunner(agent_worker)

response = agent.query(
    "Tell me about the evaluation dataset used in LongLoRA, "
    "and then tell me about the evaluation results"
)

response = agent.query("Give me a summary of both Self-RAG and LongLoRA")
print(str(response))

response = agent.query(
    "Tell me about the evaluation dataset used "
    "in MetaGPT and compare it against SWE-Bench"
)
print(str(response))
```

### OUTPUT:
![image](https://github.com/user-attachments/assets/7601e7b4-477c-4cfa-9e6a-8640845cdbf3)
![image](https://github.com/user-attachments/assets/2baab468-ab19-4081-ba49-ec043fb3ee99)
![image](https://github.com/user-attachments/assets/408f8a69-af72-4608-a8d7-0ba853cb2a2d)










### RESULT:
Thus, a multidocument retrieval agent using LlamaIndex to extract and synthesize information from multiple research articles is designed and implemented successfully.
