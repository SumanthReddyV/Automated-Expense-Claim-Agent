# Automated-Expense-Claim-Agent

## Project Goal
To transform a static chat interface into an actionable assistant capable of processing data and performing real-world tasks-specifically, automating expense reporting.

## What I Built
I developed an action-oriented AI agent using the Microsoft Agent Framework. My objective was to move beyond simple Q&A and create an agent that could handle a business workflow: reading raw expense data, formatting a claim, and "sending" it to the finance department.

The core feature of this project is **Tool Use**. I didn't just give the agent instructions; I gave it a custom Python function that it could invoke autonomously to perform an action.

## How I Implemented It
I built this solution using Python and the agent-framework SDK. The critical implementation detail was bridging the gap between the AI model (GPT-4o) and my local code.

### My Tech Stack & Architecture
- **Language**: Python
- **Core SDK**: Microsoft Agent Framework (agent-framework, azure-identity)
- **Model**: GPT-4o deployed in Microsoft Foundry

### The Code Logic
I defined a custom function called `send_email`. In a production environment, this would connect to an SMTP server, but for this demonstration, I programmed it to print the email details (To, Subject, Body) directly to the console.

```python
# I defined the tool for the agent to use
def send_email(to: str, subject: str, body: str):
    print("\nTo:", to)  # This simulates the email action
    print("Subject:", subject)
    print(body)

# Then I equipped the agent with this tool
async with ChatAgent(
    ...
    instructions="...use the plug-in function to send an email to expenses@contoso.com...",
    tools=send_email, 
) as agent:
```

## Project Structure
I organized the project files to separate the environment configuration from the application logic:

```
ai-agents/
└── Labfiles/
    └── 04-agent-framework/
        └── python/
            ├── .env                 # Stores my Project Endpoint & Model Name
            ├── agent-framework.py   # Main agent logic and tool definition
            └── expenses.json        # Sample data for the agent to process
```

## How to Run My Code
To see this agent in action, you can replicate my deployment steps:

### Clone & Navigate
I used the standard repository and navigated to the framework module:

```bash
rm -r ai-agents -f
git clone https://github.com/MicrosoftLearning/mslearn-ai-agents ai-agents
cd ai-agents/Labfiles/04-agent-framework/python
```

### Install Dependencies
I set up a virtual environment and installed the required SDKs:

```bash
pip install azure-identity agent-framework
```

### Execute the Agent
Because the agent interacts with Azure resources, I authenticate via the CLI first:

```bash
az login
python agent-framework.py
```

### Test It
When the app runs, I submit the prompt:

```
Submit an expense claim
```

### Outcome
The agent parses the loaded expense data and autonomously calls my `send_email` function, outputting a formatted claim email to the console.
