---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Migrating from Anthropic’s Claude 3.5 Sonnet to Claude 4 Sonnet on Amazon Bedrock

With the launch of Anthropic’s **Claude 4 Sonnet** on Amazon Bedrock, organizations are encouraged to migrate from the deprecated **Claude 3.5 Sonnet** models. This transition offers significant benefits: a larger context window, improved reasoning features, and more advanced tool use. However, migration must be carefully managed to avoid service disruptions and ensure smooth adoption.

---

## Key Model Differences

- **Context Window**: Increased from 200,000 tokens → 1 million tokens (beta).  
- **Reasoning**: Built-in *extended thinking* and *interleaved thinking*, unlike Claude 3.5 which relied on chain-of-thought prompting.  
- **Tool Use**: Parallel execution of multiple tools with reasoning steps between calls (vs sequential execution in 3.5).  

| Feature              | Claude 3.5 Sonnet | Claude 4 Sonnet |
|-----------------------|------------------|-----------------|
| Context window        | 200K tokens      | 1M tokens (beta) |
| Reasoning mechanism   | Prompt-based CoT | Native extended & interleaved thinking |
| Tool use              | Sequential only  | Parallel with reasoning support |

---

## Prerequisites

- Enable **Claude 4 Sonnet** access in Amazon Bedrock.  
- Accept the **EULA** and confirm availability in your chosen AWS Region.  
- Use **cross-Region inference (CRIS)** if necessary to optimize throughput.  

---

## API Changes and Code Updates

- **InvokeModel API**: Just update the modelId.  
  - Old ID: `'anthropic.claude-3-5-sonnet-20240620-v1:0'`  
  - New ID: `'anthropic.claude-4-sonnet-20240514-v1:0'`  

- **Converse API**: Recommended for easier future migrations.  

```python
import boto3

bedrock_runtime = boto3.client(service_name='bedrock-runtime')

response = bedrock_runtime.converse(
    modelId='us.anthropic.claude-sonnet-4-20250514-v1:0',
    messages=[{'role': 'user', 'content': [{'text': "Your prompt here"}]}],
    inferenceConfig={'maxTokens': 1024}
)

print(response['output']['message']['content'][0]['text'])
