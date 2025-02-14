# Multi-Agent Code Review System
(Work in progress) A multi-agent system for automated and intelligent code review.  Leveraging AI and collaborative agents to improve code quality, streamline workflows, and enhance team collaboration.

The top-level task "Automated Code Review" is decomposed into a hierarchy where the Planner/Interpreter Agent interprets the intent from user input and manages subordinate agents. Each agent focuses on a specific task, making the system modular and scalable. The intermediate output will then be passed to an Aggregation & Review Agent for consolidation

<br>

```mermaid
flowchart TB
    PIA[Planner/Interpreter Agent] --> ICR[Intent Classification & Routing]
    
    ICR --> RCA[Repository Cloning Agent]
    RCA --> CEA[Code Change Extraction Agent]
    RCA --> SAA[Static Analysis Agent]
    RCA --> DAA[Dynamic Analysis Agent]
    RCA --> DSA[Documentation & Security Agent]
    CEA --> ALA[Aggregation & Review Agent]
    SAA --> ALA
    DAA --> ALA
    DSA --> ALA

    
    subgraph RCA[Repository Cloning]
        RCA1[Clone GitHub repository to temp]
        RCA2[Prepare codebase for analysis]
    end
    
    subgraph CEA[Code Change Extraction]
        CEA1[Extract commit metadata]
        CEA2[Parse Git history]
    end
    
    subgraph SAA[Static Analysis]
        SAA1[Run linters]
        SAA2[Evaluate code quality]
    end
    
    subgraph DAA[Dynamic Analysis]
        DAA1[Execute profiling]
        DAA2[Verify correctness]
    end
    
    subgraph DSA[Documentation & Security]
        DSA1[Validate documentation]
        DSA2[Scan vulnerabilities]
    end
    
    subgraph ALA[Aggregation & Review]
        ALA1[Aggregate reports]
        ALA2[Generate insights]
    end
```
<br>

We can use TypedDict for managing state/memory of the workflow
```python
##  states.py
from typing import TypedDict, List, Dict
from datetime import datetime

class RepositoryState(TypedDict):
    temp_path: str
    branch: str
    is_prepared: bool

class ChangesetState(TypedDict):
    changed_files: List[str]
    commit_data: Dict[str, any]
    diff_stats: Dict[str, int]

class TaskState(TypedDict):
    repository_url: str
    commit_hash: str
    pending_agents: List[str]
    completed_agents: List[str]

class AgentCoordinator(TypedDict):
    workflow_id: str
    status: str
    task_queue: Dict[str, List[str]]
    current_state: TaskState
    agent_states: Dict[str, Dict]
```
