<img src="https://images.pexels.com/photos/3862634/pexels-photo-3862634.jpeg" />

# VoltMotors ⚡

**VoltMotors** is a European electric vehicle startup that sells three models: the Volt City (urban hatchback), the Volt Touring (family SUV), and the Volt Sport (performance coupe). The company operates showrooms in six cities and handles customer inquiries through an AI assistant called **VoltBot**.

VoltBot uses the **OpenAI Responses API** with built-in tools: **web search** to find real-time data and **code interpreter** to run calculations. No custom tool code is needed. The API provides and executes the tools for you.

In this review, you run pre-written code, observe the outputs, and answer questions that test your understanding.

Run every code cell in order. Read the outputs carefully. Then answer the questions that follow each Task.

## Setup


```python
import os
import json
from google.colab import userdata
from openai import OpenAI

# Read the secret and store it where the OpenAI library expects to find it
os.environ["OPENAI_API_KEY"] = userdata.get("OPENAI_API_KEY")

# The client automatically picks up the key from the environment
client = OpenAI()
MODEL = 'gpt-4o-mini'
```

## Task 1: Single call vs built-in tools

Run the two cells below. The first sends a customer question **without** tools. The second sends the same question **with** web search enabled.


```python
# WITHOUT tools: the model uses only its training data
response_no_tools = client.responses.create(
    model=MODEL,
    instructions='You are VoltBot, the AI assistant for VoltMotors.',
    input='What is the price of the Volt Touring SUV? Is it available right now?'
)

print('=== WITHOUT TOOLS ===')
print(response_no_tools.output_text)
```


```python
# WITH web search: the model searches the internet for real data
response_with_tools = client.responses.create(
    model=MODEL,
    tools=[{"type": "web_search_preview"}],
    instructions=(
        'You are VoltBot, the AI assistant for VoltMotors. '
        'Use web search to find real, current information about VoltMotors vehicles. '
        'If you cannot find VoltMotors data, say so honestly.'
    ),
    input='What is the price of the Volt Touring SUV? Is it available right now?'
)

print('=== WITH WEB SEARCH ===')
print(response_with_tools.output_text)
```

### Answer the questions below

<AITextGrader
  question="Compare the two outputs. The first response (without tools) sounds confident. The second (with web search) may say it could not find VoltMotors data. Which response is more trustworthy, and why?"
  templateAnswer="The second response is more trustworthy because it is grounded in real data. VoltMotors is a fictional company, so the web search correctly finds nothing. The first response invents plausible-sounding but completely fake information. A model that admits it cannot find data is safer than one that confidently makes things up."
/>

<AITextGrader
  question="For a real company like Tesla or Renault, which approach (with or without web search) would give more useful results? Why?"
  templateAnswer="With web search. The model can find real, current prices, availability, and specifications from the company's website and reviews. Without web search, the model only has its training data, which may be outdated or incomplete. Web search gives access to live information."
/>

## Task 2: Inspecting the agent's steps

When the model uses built-in tools, the `response.output` list contains every step the model took. Run the cell below and study the output.


```python
# Search for real EV data and inspect every step
response = client.responses.create(
    model=MODEL,
    tools=[{"type": "web_search_preview"}],
    instructions='You are VoltBot. Use web search to find real electric vehicle data.',
    input='What are the cheapest electric SUVs available in Europe in 2026? Compare three models.'
)

print('=== AGENT STEPS ===')
step_number = 0
for item in response.output:
    step_number = step_number + 1
    print(f'Step {step_number}: {item.type}')

    # For messages, show the sources used
    if item.type == 'message':
        for content_block in item.content:
            if hasattr(content_block, 'annotations'):
                source_count = 0
                for annotation in content_block.annotations:
                    if annotation.type == 'url_citation':
                        source_count = source_count + 1
                print(f'  Sources cited: {source_count}')

print('\n=== FINAL ANSWER ===')
print(response.output_text)
```

### Answer the questions below

<AITextGrader
  question="You see items with type 'web_search_call' and 'message' in the output. The 'web_search_call' item only shows an ID and a status. Where do the actual web sources appear?"
  templateAnswer="The actual web sources appear as url_citation annotations inside the message item's content. Each annotation contains the URL, the title of the source, and the position in the text where it is cited. The web_search_call item only tells you that a search happened and whether it completed."
/>

<AITextGrader
  question="The Responses API ran the entire agent loop (search, read results, write answer) in a single API call. In the previous lecture, you learned that an agent loop has four steps: send to LLM, check for tool calls, execute tools, append results. Who runs these steps when you use the Responses API?"
  templateAnswer="The API runs all four steps internally. You do not write a while loop, execute tools, or manage a message list. The Responses API is agentic by default. It handles tool execution, result collection, and the decision of whether to search again or produce a final answer. You just call client.responses.create() once."
/>

## Task 3: Code interpreter for calculations

VoltBot needs to help customers compare costs. Run the cell below. It uses the code interpreter tool to do real calculations.


```python
# Code interpreter calculates total cost of ownership
response = client.responses.create(
    model=MODEL,
    tools=[
        {
            "type": "code_interpreter",
            "container": {"type": "auto"}
        }
    ],
    instructions='You are VoltBot. Use Python code for all calculations. Show the breakdown clearly.',
    input=(
        'A customer is comparing two options for a 3-year period: '
        'Option A: Buy a Volt Touring at 47500 euros, electricity cost 5 euros per 100km, '
        'insurance 1200 euros per year, drives 15000 km per year. '
        'Option B: Lease a competitor SUV at 650 euros per month, fuel cost 10 euros per 100km, '
        'insurance included, drives 15000 km per year. '
        'Which option is cheaper over 3 years? Show the math.'
    )
)

print(response.output_text)
```


```python
# Show the Python code the model wrote
for item in response.output:
    if item.type == 'code_interpreter_call':
        print('=== CODE THE MODEL WROTE ===')
        print(item.code)
        print()
```

### Answer the questions below

<AITextGrader
  question="The model wrote and ran Python code to answer the question. You can see the exact code in the output above. Why is this better than letting the model do the math in its head (without code interpreter)?"
  templateAnswer="LLMs frequently make arithmetic errors when calculating in text. Code interpreter runs actual Python code that produces exact results. You can also inspect the code to verify the logic is correct. This is especially important for financial calculations where accuracy matters."
/>

## Task 4: Combining multiple tools

VoltBot can use web search and code interpreter together. The model decides which tool to use at each step. Run the cell below.


```python
# Combine web search + code interpreter
response = client.responses.create(
    model=MODEL,
    tools=[
        {"type": "web_search_preview"},
        {
            "type": "code_interpreter",
            "container": {"type": "auto"}
        }
    ],
    instructions=(
        'You are VoltBot. Search the web for real data. '
        'Use code interpreter for calculations. Never invent numbers.'
    ),
    input=(
        'Search for the average electricity price in France in 2026 (per kWh). '
        'Then calculate the annual energy cost for a car that drives 15000 km '
        'and consumes 18 kWh per 100 km.'
    )
)

# Show every step
print('=== STEPS ===')
for item in response.output:
    print(f'  {item.type}')

print('\n=== ANSWER ===')
print(response.output_text)
```

### Answer the questions below

<AITextGrader
  question="The model used two different tools to answer one question. It searched the web for the electricity price, then used code interpreter to calculate the annual cost. Could it have answered this question with only one tool? Explain."
  templateAnswer="It needed both tools. Web search was necessary to find the current electricity price in France, which changes over time. Code interpreter was necessary to calculate the annual cost accurately. Without web search, the price would be a guess. Without code interpreter, the math might contain errors."
/>

## Task 5: Multi-turn conversations

A real customer conversation has multiple turns. The Responses API remembers context using `previous_response_id`. Run the two cells below.


```python
# Turn 1
turn_1 = client.responses.create(
    model=MODEL,
    tools=[{"type": "web_search_preview"}],
    instructions='You are VoltBot. Help customers find the right electric vehicle.',
    input='I am looking for a family electric SUV with at least 400 km range. What options exist in Europe?'
)

print('=== TURN 1 ===')
print(turn_1.output_text)
```


```python
# Turn 2: follow-up using previous context
turn_2 = client.responses.create(
    model=MODEL,
    tools=[{"type": "web_search_preview"}],
    input='Which of those is the cheapest? Can I find a showroom in Berlin?',
    previous_response_id=turn_1.id
)

print('=== TURN 2 ===')
print(turn_2.output_text)
```

### Answer the questions below

<AITextGrader
  question="In Turn 2, the customer says 'Which of those'. The model understands this refers to the SUVs from Turn 1. How does the API know what 'those' means? What parameter enables this?"
  templateAnswer="The previous_response_id parameter connects Turn 2 to Turn 1. The API stores the full conversation history from Turn 1 (including the web search results). When Turn 2 arrives, the model sees the entire previous context. This is how it knows 'those' refers to the SUVs mentioned in Turn 1."
/>

<AITextGrader
  question="Compared to the Chat Completions API where you manage a messages list manually, what advantage does previous_response_id give you?"
  templateAnswer="You do not need to manage a messages list. The API stores the conversation state on its servers. You only pass the previous_response_id. This is simpler, less error-prone, and means you do not need to track tool call IDs or append tool results manually."
/>

## Task 6: Workflows vs agents

Run the cell below. It shows a **prompt chaining workflow** for processing a customer complaint. This is NOT an agent. The code defines the steps and their order. The LLM handles each step.


```python
# Prompt chaining: fixed three-step complaint processing

complaint = (
    'I ordered a Volt Touring three months ago. The delivery has been delayed twice. '
    'Nobody from VoltMotors contacted me to explain. I found out about the delays '
    'through the tracking website. This is unacceptable for a 47500 euro purchase.'
)

# Step 1: Extract the issue
step1 = client.responses.create(
    model=MODEL,
    instructions=(
        'Extract the core issue from this customer complaint. '
        'Return JSON with: issue_type, financial_impact, emotional_impact. '
        'Respond with JSON only.'
    ),
    input=complaint
)
extraction = step1.output_text
print('Step 1 - Extraction:')
print(extraction)

```


```python

# Step 2: Classify severity
step2 = client.responses.create(
    model=MODEL,
    instructions=(
        'Classify this complaint as: critical, high, medium, or low. '
        'Return JSON with: severity, reason. Respond with JSON only.'
    ),
    input=extraction
)
classification = step2.output_text
print('\nStep 2 - Classification:')
print(classification)
```


```python
# Step 3: Draft response
combined = f'Complaint: {complaint}\nExtraction: {extraction}\nSeverity: {classification}'
step3 = client.responses.create(
    model=MODEL,
    instructions=(
        'You are a VoltMotors customer service agent. '
        'Draft a response. Be empathetic and propose a resolution. '
        'Keep it under 150 words.'
    ),
    input=combined
)
print('\nStep 3 - Response:')
print(step3.output_text)
```

### Answer the questions below

<AITextGrader
  question="This complaint processing pipeline uses three separate API calls in a fixed order. Is this a workflow or an agent? Explain the difference."
  templateAnswer="This is a workflow. The code defines three steps and their order: extract, classify, draft. The LLM runs each step but never decides what comes next. An agent would decide the order itself based on what it discovers. Here, the if-logic and sequence are in the code, not in the model's reasoning."
/>

<AITextGrader
  question="When should VoltBot use a workflow (like this complaint pipeline) vs an agent (like the web search examples above)? Give one example of each."
  templateAnswer="Use a workflow when the steps are known in advance and always follow the same order, like complaint processing (always extract, classify, respond). Use an agent when the steps depend on what the model discovers, like answering a customer question about vehicles, where the model may need to search for prices, compare models, check availability, and the order depends on what it finds."
/>
