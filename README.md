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