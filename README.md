# Chart-Generation-Agentic-Workflow-Reflection-
A multi-modal LLM will review the first draft chart, identify potential improvements—such as chart type, labels, or color choices and then rewrite the chart generation code to produce a more effective visualization.

The steps that the workflow will carry out are:
Generate an initial version (V1): Use a Large Language Model (LLM) to create the first version of the plotting code.
Execute code and create chart: Run the generated code and display the resulting chart. ** (check everywhere)
Reflect on the output: Evaluate both the code and the chart using an LLM to detect areas for improvement (e.g., clarity, accuracy, design).
Generate and execute improved version (V2): Produce a refined version of the plotting code based on reflection insights and render the enhanced chart.
<img width="994" height="385" alt="image" src="https://github.com/user-attachments/assets/87a5799d-a68f-40c4-b23c-c4100dc3420d" />

Reflection pattern in code and used it to improve a data visualization.
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
1. Setup: Initialize environment and client
In this step, you import the key libraries that will support the workflow:

re: Python’s regular expression module, which you’ll use to extract snippets of code or structured text from the LLM’s output.
json: Provides functions to read and write JSON, useful for handling structured responses returned by the LLM.
utils: A custom helper module provided. It includes utility functions to work with the dataset, generate charts, and display results in a clean, readable format.

2. Loading the dataset
You’ll build an agentic workflow that generates data visualizations from this dataset, helping you answer questions about coffee sales from the vending machine.

3.Building the pipeline
Step 1 — Generate Code to Create a Chart (V1)
In this step, you’ll prompt an LLM to write Python code that generates a chart in response to a user query about the coffee dataset. The dataset includes fields such as date, coffee_type, quantity, and revenue, and you will pass this schema into the LLM so it knows what data is available.
The question you’ll ask the model is the same one used in the lecture:
“Create a plot comparing Q1 coffee sales in 2024 and 2025 using the data in coffee_sales.csv.”
The LLM’s output will be Python code using the matplotlib library. Instead of displaying the chart directly, the code will be written between <execute_python> tags so it can be extracted and run in later steps. 

Step 2 — Execute Code and Create Chart
In this step, you’ll use a regular expression to extract the Python code that the LLM generated in the previous step (the part written between <execute_python> tags). Once extracted, you’ll run this code to produce the first draft chart.
Here's how it works:
Extract the code:
A regex pattern is used to grab the code that’s wrapped inside the <execute_python> tags.
Execute the code: The extracted code is run in a predefined global context where the DataFrame df is already available. This means your code can directly use df without needing to reload the dataset.
Generate the chart:: If the code executes successfully, it will create a chart and save it as chart_v1.png.
View the chart in the notebook: The saved chart is then displayed inline using utils.print_html, making it easy for you to review the results.
By completing this step, you’ll have your first draft visualization (V1) ready — a big milestone in the reflection workflow!

Step 3 — Reflect on the output
The goal here is to simulate how a human would review a first draft of a chart—looking for strengths, weaknesses, and areas for improvement.
Here’s what happens:
1. Provide the chart to the LLM: The generated chart (chart_v1.png) is shared with the LLM so it can “see” the visualization.
2. Analyze the chart visually: The LLM reviews elements like clarity, labeling, accuracy, and overall readability.
3. Generate feedback: The LLM suggests improvements—for example, fixing axis labels, adjusting the chart type, improving color choices, or highlighting missing legends.
By doing this, you create an intelligent feedback loop where the chart is not just produced once, but actively critiqued—setting the stage for a stronger second version (V2).
Note that, the model is instructed to return its response in JSON format.

JSON is a lightweight, structured format (key–value pairs) that makes it easy to parse the LLM’s output programmatically.
Here, we require two fields:
feedback: a short critique of the current chart.
refined_code: an improved Python code snippet wrapped in <execute_python> tags.
We also include a “constraints” section in the prompt. These rules (e.g., use matplotlib only, save the file to a specific path, call plt.close() at the end) help the model generate consistent, runnable code that fits the workflow. Without these constraints, the output might vary too much or include unwanted formatting.

Step 4 — Generate and Execute Improved Version (V2)
In this final step, it’s time to generate and run the improved version of the chart (V2).
After running the cell, you’ll see both the reflection written by the LLM (explaining what needed improvement) and the new code it generated. The new code will then be executed to produce the updated chart.
Now you’ll execute the refined code returned by the reflection step. The code inside the <execute_python> tags is extracted, run against the dataset, and used to generate the updated chart.

If the execution is successful, you’ll see the new image (chart_v2.png) displayed below as the Regenerated Chart (V2).

Put it all together — creating the end-to-end workflow
Now it’s time to wrap everything into a single automated workflow the agent can run from start to finish.

The run_workflow function links together the components you implemented earlier:

1) Load and prepare data — via utils.load_and_prepare_data(...). 2) Generate V1 code — with generate_chart_code(...), which returns the first-draft matplotlib code (wrapped in <execute_python> tags).
3) Execute V1 immediately — the workflow extracts the code between <execute_python> tags and runs it to produce the first chart image.
4) Reflect and refine — reflect_on_image_and_regenerate(...) critiques the V1 image (and the original code) against the instruction, returns concise feedback plus revised code (V2). 5) Execute V2 immediately — the refined code is extracted and executed to generate the improved chart.

What this workflow accepts
dataset_path: location of the input CSV.
user_instructions: the chart request (e.g., “Create a plot comparing Q1 coffee sales in 2024 and 2025 using the data in coffee_sales.csv.”).
generation_model: model used for the initial code generation.
reflection_model: model used for the image-based reflection and code refinement.
image_basename: base filename for saving chart images (e.g., chart_v1.png, chart_v2.png).

Choosing models
You can mix and match different models for generation and reflection. For example:

Use a fast model for initial code generation (gpt-4.1-mini or gpt-3.5-turbo).
Use a stronger reasoning model for reflection (gpt-4.1 or claude-3-7-sonnet-latest).
This flexibility lets you explore trade-offs between speed and quality.

Final Takeaways:
Generate an initial chart (V1).
Critique and refine it into a better version (V2).
Automate the full workflow with different models.
The key idea: reflection helps you create clearer, more accurate, and more effective visualizations.
