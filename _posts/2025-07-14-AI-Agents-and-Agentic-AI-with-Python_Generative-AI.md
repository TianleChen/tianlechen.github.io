---
layout: post
title: "AI Agents and Agentic AI with Python & Generative AI"
date: 2025-07-14
categories: blog
tags: [GenAI]
description: "Notes for AI Agents and Agentic AI with Python & Generative AI"
---

# **Module 1**
## **Learning from This Design**
This quasi-agent teaches us several important lessons about building LLM-powered systems:
- Prompt Chaining: Breaking complex tasks into sequential steps makes them more manageable.
- Context Management: Carefully controlling what the LLM sees helps maintain focus and consistency.
- Output Processing: Having robust ways to extract and clean LLM output is crucial.
- Progressive Enhancement: Building features iteratively (code → docs → tests) creates better results than trying to do everything at once.

## **The Agent Loop in Python**
The agent loop is the backbone of our AI agent, enabling it to perform tasks by combining response generation, action execution, and memory updates in an iterative process. This section focuses on how the agent loop works and its role in making the agent dynamic and adaptive.
- Construct Prompt: Combine the agent’s memory, user input, and system rules into a single prompt. This ensures the LLM has all the context it needs to decide on the next action, maintaining continuity across iterations.
- Generate Response: Send the constructed prompt to the LLM and retrieve a response. This response will guide the agent’s next step by providing instructions in a structured format.
- Parse Response: Extract the intended action and its parameters from the LLM’s output. The response must adhere to a predefined structure (e.g., JSON format) to ensure it can be interpreted correctly.
- Execute Action: Use the extracted action and its parameters to perform the requested task with the appropriate tool. This could involve listing files, reading content, or printing a message.
- Convert Result to String: Format the result of the executed action into a string. This allows the agent to store the result in its memory and provide clear feedback to the user or itself.
- Continue Loop?: Evaluate whether the loop should continue based on the current action and results. The loop may terminate if a “terminate” action is specified or if the agent has completed the task.


# **Module 2**

## **Breaking Down Function Calling Step by Step**
1. Define the Tool Functions
2. Create a Function Registry
3. Define Tool Specifications Using JSON Schema
4. Set Up the Agent’s Instructions
5. Prepare the Conversation Context
6. Make the API Call with Function Definitions
7. Process the Structured Response
8. Execute the Function with the Provided Arguments

## **Key Benefits of Function Calling APIs**
- Eliminates prompt engineering for structured responses – No need to force the model to output JSON manually.
- Uses standardized JSON Schema – The same format used in API documentation applies seamlessly to AI interactions.
- Allows mixed text and tool execution – The model can decide whether a tool is necessary or provide a natural response.
- Simplifies parsing logic – Instead of handling inconsistent outputs, developers only check for tool_calls in the response.
- Guarantees syntactically correct arguments – The model automatically ensures arguments match the expected parameter format.

## **Function Calling and the Simplified Agent Loop**

## **What This Changes**
- No More Custom Parsing Logic - We don’t need to engineer strict text output parsing; the model always returns structured JSON for function calls.
- Dynamic Execution - Instead of a rigid loop that manually checks what the agent should do, we simply read and execute the function call.
- Unified Text & Action Handling - If no function call is needed, the model responds with a message, allowing mixed conversational and action-driven workflows.
- Automated Function Execution - The agent dynamically maps the tool name from the model to its corresponding Python function and executes it with the provided arguments.

## **Agent Tool Design Best Practices**
When integrating AI into real-world environments, tool descriptions must be explicit, structured, and informative. By following these principles:
- Use descriptive names.
- Provide structured metadata.
- Leverage JSON Schema for parameters.
- Ensure AI has contextual understanding.
- Include robust error handling.
- Provide informative error messages.
- Inject instructions into error messages.

# **Module 3**
## **The GAME Framework: Designing AI Agents**
The GAME framework provides a structured way to design AI agents, ensuring modularity and adaptability. It breaks agent design into four essential components:
- **G** - Goals / Instructions: What the agent is trying to accomplish and its instructions on how to try to achieve its goals.
- **A** - Actions: The tools the agent can use to achieve its goals.
- **M** - Memory: How the agent retains information across interactions, which determines what information it will have available in each iteration of the agent loop.
- **E** - Environment: The agent’s interface to the external world where it executes actions and gets feedback on the results of those actions.

## **Simulating GAME Agents in a Conversation**
How Your Agent Communicates with the LLM: The Agent Language
The AgentLanguage component is crucial because it:
- Centralizes Communication Logic: All prompt construction and response parsing is in one place
- Enables Experimentation: We can try different prompt strategies by creating new language implementations
- Improves Reliability: Structured response formats and error handling make the agent more robust
- Supports Evolution: As LLM capabilities change, we can adapt our communication approach without changing the agent’s core logic

# **Module 4**
## **Keeping Agent Tools Up to Date with Python Decorators**
Our decorator examines the function and automatically:
- Uses the function name as the tool name
- Extracts the docstring for the description
- Analyzes type hints and parameters to build the schema
- Registers the tool in a central registry

## **Why Create a Decorator?**
You will see this decorator approach in many different agent frameworks. Here is why:
- Single Source of Truth: The function itself becomes the authoritative source for all tool information. The docstring describes what it does, the type hints define its parameters, and the implementation shows how it works.
- Automatic Updates: When we modify the function’s signature or documentation, the tool registration automatically stays in sync. No more hunting through code to update parameter schemas.
- Better Organization: The tags system allows us to categorize tools and find related functionality. We can easily get all “file_operations” tools or all “database_tools”.
- Improved Development Experience: We write our tools as normal Python functions with standard documentation. The decorator handles all the complexity of making them available to our agent.

# **Module 5**
## **Real-World Applications**
This pattern extends beyond inventory management. Consider:
- Policy compliance checking (like the travel expenses example)
- Document processing systems
- Customer service applications
- Data analysis tools

The key is always the same:
- Define clear goals and instructions
- Provide simple, focused tools
- Let the agent’s intelligence handle complexity and allow adaptation
The future of software development isn’t about writing every possible edge case - it’s about providing the right framework for AI agents to handle complexity while keeping the tools and infrastructure simple and reliable.