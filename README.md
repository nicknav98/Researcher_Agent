# Original Xbox research workflow

This repository contains a Jupyter notebook that runs three research stages in Python:

1. **Researcher** searches the web for sources about the required Xbox topics.
2. **Fact Checker** checks those sources and builds an outline with at least three relevant sources per topic.
3. **Writer** drafts the article from the checked outline without web access.

The notebook reads [outcomes.md](outcomes.md) and passes its acceptance criteria to each stage. Before saving a draft, Python checks the required headings, a Summary section, and at least three distinct checked citations in each section. It then converts source IDs to numbered inline citations and builds the reference list. These checks cannot establish whether every claim is accurate or whether two differently worded passages repeat the same point, so review the finished draft and its sources.

The five topic sections are defined in `REQUIRED_SECTIONS` inside [multi_agent_research_workflow.ipynb](multi_agent_research_workflow.ipynb). If you change the research topic substantially, update those sections and [outcomes.md](outcomes.md) together.

## Run locally

Use Python 3.13, the version recorded in the notebook, and run these commands from the repository root:

```sh
python3 -m venv .ven
source .ven/bin/activate
python -m pip install -r requirements.txt jupyterlab ipykernel
python -m jupyter lab
```

Open `multi_agent_research_workflow.ipynb` in JupyterLab and select the kernel from `.ven`. Run the cells in order. The first code cell installs or upgrades `openai` and `python-dotenv`; you can skip that cell when the packages were already installed from `requirements.txt`.

Create a `.env` file in the repository root before running the configuration cell:

```dotenv
OPENAI_API_KEY=your_openai_api_key
RESEARCH_TOPIC=Softmodded original Xbox capabilities, video output modes, game-region switching, burned discs, and automatic region handling
```

`OPENAI_API_KEY` is required. `RESEARCH_TOPIC` is optional; the notebook defaults to the topic shown above. The API key must have access to the notebook's fixed model, `gpt-6-luna`, and its web-search tool. The `.env` file is ignored by Git. Keep the notebook's working directory at the repository root so it can find `.env` and `outcomes.md` and write to `outputs/`.

The notebook estimates a $0.10 run budget and checks usage after each API stage. This is an estimate, not a prepaid spending limit. Research and fact checking each allow up to three web-search tool calls; writing uses no tools. A run stops before saving the final draft if its required source coverage or draft checks fail.

## Outputs

A successful run writes these files under `outputs/`:

| File | Contents |
| --- | --- |
| `final_draft.md` | Article with a Summary and numbered references |
| `citations.json` | Topic, checked source records, outline, and reference numbers |
| `workflow_events.json` | Stage status, token usage, web-search calls, and estimated cost |
| `research_outputs.zip` | Bundle of the three files above |

Running the notebook again overwrites those files. The committed output bundle includes a manual source review of an earlier run, recorded in its metadata; it is not an execution result from the current notebook version.
