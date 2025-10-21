# agentic-ai-notes
Notes for DeepLearning.AI Agentic AI Course

## Module 1: Introduction to Agentic Workflows

Agentic AI workflows: An agentic AI workflow is a process where an LLM-based app executes multiple steps to complete a task.

Degrees of autonomy
Agentic AI can be less or more autonomous
Less: steps predetermined, tool use hard coded, autonomy in text generation
Semi-autonomous: agent can make some decisions, choose tools, all tools predefined
More: agent makes many decisions autonomously, can create new tools on the fly

Benefits of Agentic AI
Much better performance
Faster than humans because of parallelization
Modular: can add or update tools, swap out models

Agentic AI applications
Easier: clear, step-by-step process, standard procedures to follow, text assets only
Harder: steps not known ahead of time, plan/solve as you go, multimodal (sounrd/vision)

Task decomposition: identifying the steps in a workflow
Building block | Examples | Use cases
Models | LLMs | text generation, tool use, planning, information extraction
Models | Other AI models | image analysis, text-to-speech
Tools | API | web search, get realtime data, send email, check calendar
Tools | Information retrieval | database, RAG
Tools | code execution | basic calculator, data analysis

Evaluation agentic AI (evals)
Look for low-quality outputs
Using LLM as a judge
- Can evaluate using code (objective evals), use LLM-as-judge (subjective evals)
- 2 types of evals: end-to-end and component-level
- Examine traces (intermediate outputs) to perform error analysis
- Much more on evals and error analysis

Agentic design patterns
1. reflection (e.g. coder agent vs. human or coder agent vs. critic agent in case of multi-agents scenarios)
2. tool use
    Analysis (code execution, Wolfran Alpha, bearly code interpreter)
    Information gathering (web search, wikipedia, database access)
    Productivity (email, calendar, messaging)
    Images (image generation, image captioning, OCR)
3. planning (LLM decides sequence of steps to take)
4. muti-agentic workflow (e.g. ChatDev software framework)


## Module 2: Reflection Design Pattern

Reflection to improve outputs of a task
Example 1. reflection - Agentic AI: writing an email and ask model to reflect
Example 2. reflection to improve code: choose different models for initial draft and reflection
Example 3. reflection with external feedback: write code, LLM initial draft, code execution with syntax error, feedback, model reflection
Whenever reflection has opportunity for additional information, more powerful LLM.

Why not just direct generation
Direct generation: zero-shot prompting
Zero, one, and few-shot prompting
    zero-shot: no examples
    one-shot: single example
    few-shot: multiple examples
Reflection has been tested with performance improvement. Reflection consistently outperforms direct generation on a variety of tasks.
Tasks where reflection works better
    Example | Problem | Reflection prompt
    Generate html table | Missing '>' | Validate the html code
    How to brew aperfect cup of tea | Missing steps | Check instructions for coherence and completeness
    Generating domain names | Name has unintended meaning, or is hard to pronounce | Does domain name have any negative connotations? Is the domain name hard to pronounce?
Tips for writing reflection prompts
    Clearly indicate the reflection action
    Specify criteria to check

Chart generation workflow
Reflection with a different LLM
How to tune the initial generation or the reflection prompt to try to get better performance

Evaluating the impact of reflection
Known issues with using LLMs for comparision
    answers often not very good
    position bias (LLM tends to pick first / second answer)
Grading with a rubric gives more consistent results
Evaluating reflection
    Objective evals
        Code-based evals are easier
        Build a dataset of ground truth examples
    Subjective evals
        Use LLM as a judge
        Rubric-based grading is better

Using external feedback
Reflection with external feedback tends to bump to a higher level of performance then reflection on existing info.
Other examples of tools to help reflection
    Challenge | Example | Source of feedback
    Mentioning competitors | Our company's shoes are better than RivalCo | Pattern matching for competitor names
    Fact checking an essay | The Taj Mahal was built in 1648 | Web search results
    LLM won't follow output length guidelines | Essay is over word limit | Word count tool


## Module 3: Tool use

What are tools?
Definition: Let LLMs call functions / request to call functions
Tools are functions that we provide to the LLM that it can request to call.
LLMs can choose tools when appropriate.
Examples
Prompt | Tool | Output
Can you find some Italian restaurants near Mountain View, CA? | web_search (query="restaurants near Mountain View, CA") | Spaghetti City is an Italian restaurant in Mountain View
Show me customers who bought white sunglasses | query_database (table="sales", product="sunglasses", color="white") | 28 customers bought white sunglasses. Here they are ...
How much money will I have after 10 years if I deposit $500 at 5% interest | interest_calc(principal=500, interest_rate=5, years=10) OR eval("500 * (1 + 0.05) * 10") | $814.45
Developer think through the things you want an application to do, then create the functions or the tools that are needed to make them available to the LLM to let it use the appropriate tools to complete the tasks.
Depending on the application, need to implement and make different tools available to the LLM.

Creating a tool
Tools are just code that the LLM can request to be executed.
Prompting an LLM to use tools.
Example 1. System prompt: You have access to a tool called get_current_time. To use it, return the following exactly: Function: get_current_time()
Example 2. System prompt: You have access to a tool called get_current_time for a specific timezone. To use it, return the following exactly: Function: get_current_time("timezone")
Process: provide the tool to the LLM, implement the function, tell the LLM that it is available. When LLM decides to call a tool, it generates a specific output, you need to call the function for the LLM. You call the function, get its output, take the output of the the function you called, give it back to the LLM. LLM use it to do next step.
Before LLM was trained natively.

Tool syntax
Defining tools syntax (model, messages, tools, max_turns)
Behind the scenes: AISuite creates Json schema (name, description, parameters)

Code execution
Example. a simple calculator, create multiple tools
Alternative approach: writing code
exec(output)
Reflection with external feedback (if any syntaxError)
Secure code execution: running outside of a sandbox can be risky. sandboxes can help protect against catastrophic errors. (E2B, docker)

MCP (Model xontext Protocol)
Give LLM more context and tools.
Painpoint: Apps (app1 / app2 / ...) -> tools (Slack / GDrive / Github / PostgreSQL)
From each app creates their own tools (m * n), to each app uses shared MCP server (m + n)
Using pre-built clients and servers.
Many serves available, some developed by the service providers.
Example. claude UI, what are the latest pull requests?


## Module 4: Practival Tips for Building Agentic AI

Evaluations (evals)
Example. invoice processing workflow, mixed up dates
Create an eval to measure data extraction
1. manually extract due dates from 10 - 20 invoices
2. specify output format of data in prompt
3. extract date from the LLM response using code
4. compare LLM result to ground truth
Driving your development process with evals
    Build a system and look at outputs to discover where it is behaving in an unsatisfactory way.
    Drive improvement by putting in place a small eval with ~20 examples to help you track progress.
    Monitor as you make changes to workflow (e.g. new prompts, new algorithms) and see if the metric improves.
Example. marketing copy assistant, length guidelines: Instagram captions : 10 works max
Create an eval to measure text length
1. create a set of 10-20 test tasks
2. add code to measyre work count of the output (word_count = len(text.split()))
3. compare length of generated text to limit (if (word_count <= 10>): num_correct += 1)
Example. research agent
Prompt | Issues
Recent clack hole science | Missed high-profile result that had lots of new coverage
Renting vs buying a home in Seattle | Seems to do a good job
Robotics for harvesting fruit | Didn't mention leading quipment company
Create an eval to measure performance
1. choose 3-5 gold standard discussion points for each topic
2. use LLM-as-a-judge to count how many topics were mentioned (slightly more subjective eval)
3. get score for each prompt in eval set
How to build evals for your situation?
Two "axes" of evaluation
                           | Evaluate with code (objective) | LLM-as-judge (subjective)
Per example ground truth | checking invoice date extraction | counting gold-standard talking points
No per example ground truth | checking marketing copy length | grading charts with a rubric
End-to-end evals
Tips for designing end-to-end evals
    Quick and dirty is ok to start (iterate your evals, metric based evals)
    As you find places where your evals fail to capture human judgement as to what system is better, use that as an opportunity to improve the metric
    Look for places where performance is worse than humans

Error analysis and prioritizing next steps
Example. research agent
possible causes, e.g. bad search terms? low quality sear results? poor selection of sources? bad reasoning over texts?
Looking at traces (aka. span): look at the intermediate outputs / steps.
Which is the most problematic components?
Focus on cases where the system made error.
Build up a spreadsheet to count up where the errors are.
Tips for error analysis
    Develop a habit of looking at traces.
    Carry out error analysis to figure out what component performed poorly, leading to a poor final output.
    Use error analysis output to decide where to focus efforts.

More error analysis examples
Example. invoice processing workflow
which component it may have due to for the mixed due date?
Counting up the error
    select 10-100 invoices for which the agentic workflow extracted the wrong due date. (e.g. LLM date extraction component)
Example. responding to customer email (extract key info, find relevant customer records, draft email)
Counting up the error by looking through a handful of emails (e.g. most common error is from LLM-drafted query)

Component-level evaluations
Example. research agent, evaluate web search tool only
end-to-end eval is expensive
build an eval just to measure one component
Benefits of component-level evaluations
    Can provide clearer signal for specific errors
        Avoid the noise in end-to-end system
    More efficient for focused team to optimize
        Work on smaller, more targeted problems faster

How to address problems you identify
Improving non-LLM component performance
    Tune hyperparameters of component (e.g. web search number of results, ml models detection threshold)
    Replace the component (e.g. try a different web search engine, RAG provider, etc)
Improving LLM component performance
    Improve your prompt (e.g. add more explcit instructions)
    Try a new model (e.g. trying multiple LLMs and use evals to pick the best) - how capable different models are
    Split up the step (e.g. decompose the task into smaller steps)
    Fine-tune a model (e.g. fine tune in your internal data to improve performance) -  on more mature application
Instruction following
Developing intuition for model intelligence
    Play with models often
        Having a personal set of evals might be helpful
        Read other people's prompts for ideas of how to best use models
    Use different models in your agentic workflows
        Which models work for which types of tasks?
        AISuite makes it easy to quickly swap out models

Latency, cost optimization
Example。research agent
overall timeline of all components
consider aprallelism
LLM steps too long
    Try smaller/less intelligent model , or faster LLM provider
Costing your workflow
    LLM steps (pay per token)
    Any API-calling tools (pay per API call)
    Compute steps (based on server capacity/cost)
Measure cost / latency for each step

Development process summary
Build + analyze: back and forth
Build end-to-end system -> examine outputs; traces
Improve individual component -> build evals; compute metrics
    -> error analysis
    -> component-level evals
custom evals fit application


## Module 5: Patterns for HIghly Autonomous Agents

Planning workflows
Example. customer service agent
Inventory database
customer query: Do you have any round sunglasses in stock that are under $100?
tools available
1. use get_item_description tool to find round sunglasses
2. use check_inventory to see if results are in stock
3. use get_item_price to see if in-stock results are < $100
step 1 text -> LLM -> get_item_description
-> step 1 output + step 2 text -> LLM -> check_inventory
-> step 2 output + step 3 text -> LLM -> get_item_price
Have LLM to wirte out multiple steps of a plan, task it to execute each step of the plan in turn with some appropriate surrounding context about what is the task, what are the tools available.
Don't need to decide the sequence
Example. email assistant
System prompt: You have access to the following tools: xxx. Return a step-by-step plan to carry out the user's request.

Creating and executing LLM plans
Formatting plan as JSON
Updated system prompt: You have access to the following tools: xxx. Create a step-by-step plan in JSON format. Each step should have the following items: step number, description, tool name, and args.
{
    "plan": [
        {
            "step": 1,
            "description": "Find round sunglasses",
            "tool": "get_item_descriptions",
            "args": {"query": "round sunglasses"}
        },
        {
            "step": 2,
            "description": "Check available stock",
            "tool": "check_inventory",
            "args": {"items": "results from step 1"}
        }
    ]
}

Planning with code execution
The challenge of planning with tools
Which month had the highest sales of hot chocolate?
Who many unique transactions ast week?
Maybe tools are insufficient to get the data.
system promt: Write code to solve the user's query. Return your answer as python code delimited with <execute_python> and </execute_python> tags.
LLM can call functions that have seen enough data.
Advantage: better performance than JSON and text.
Disadvantage: hard to control

Multi-agentic workflows
Some tasks require more than 1 person. e.g. writing a research article needs researcher, statistician, lead writer, editor.
Example. marketing team
researcher
tasks: analyze market trends, research competitors
tools: web search
graphic design
tasks: create data visualizations, create artwork
tools: image generation, manipulation, code execution for chart egenration
writer
tasks: transform research into report text and marketing copy
tools: none
Example. marketing team with linear plan
Create a summer marketing campaign for sunglasses
-> researcher (here are current sunglasses trends and competitor offering...) -> research
-> graphic designer (here are 5 data visualization and 5 artwork options for the report...) -> research, visualizations, artworks
-> writer (final report)
Example. planning with multiple agents
system prompt: You are marketing manager and have the following team of agents to work with xxx(description of agents). Return a step-by-step to carry out the user's request.
1. Ask research to research current sunglasses trends
2. Ask graphic designer to create ad images
3. Ask writer to create report
4. Review report
What is the communication pattern?

Communication patterns for multi-agent systems
Most common 2 communication patterns
Linear
Hierarchy
Deeper hierarchy: agents -> subagents -> sub-subagents
all-to-all: agents can call each other until everyone decide the task is done
