---
title: "Blog 2"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# Di chuyển từ Anthropic’s Claude 3.5 Sonnet sang Claude 4 Sonnet trên Amazon Bedrock

Anthropic đã ra mắt **Claude 4 Sonnet** trên Amazon Bedrock, đồng thời thông báo ngừng hỗ trợ **Claude 3.5 Sonnet**. Đây vừa là yêu cầu kỹ thuật, vừa là cơ hội để tận dụng khả năng mới: ngữ cảnh lớn hơn, cơ chế suy luận nâng cao và hỗ trợ công cụ mạnh mẽ hơn. Tuy nhiên, quá trình di chuyển cần được quản lý chặt chẽ để tránh gián đoạn dịch vụ.

---

## Khác biệt chính giữa các model

- **Cửa sổ ngữ cảnh**: từ 200.000 tokens → 1 triệu tokens (beta).  
- **Suy luận**: có sẵn *extended thinking* và *interleaved thinking* thay cho CoT truyền thống.  
- **Sử dụng công cụ**: hỗ trợ chạy song song nhiều công cụ và suy nghĩ giữa các bước.  

| Tính năng            | Claude 3.5 Sonnet | Claude 4 Sonnet |
|----------------------|------------------|-----------------|
| Context window       | 200K tokens      | 1M tokens (beta) |
| Cơ chế suy luận      | Prompt-based CoT | Native extended & interleaved thinking |
| Sử dụng công cụ      | Tuần tự          | Song song, có reasoning |

---

## Điều kiện tiên quyết

- Kích hoạt quyền truy cập **Claude 4 Sonnet** trong Bedrock.  
- Xác nhận Region hỗ trợ.  
- Sử dụng **cross-Region inference (CRIS)** khi cần.  

---

## Thay đổi API và cập nhật code

- **InvokeModel API**: chỉ cần đổi `modelId`.  
- **Converse API**: khuyến khích sử dụng để dễ di chuyển sau này.  

```python
import boto3

bedrock_runtime = boto3.client(service_name='bedrock-runtime')

response = bedrock_runtime.converse(
    modelId='us.anthropic.claude-sonnet-4-20250514-v1:0',
    messages=[{'role': 'user', 'content': [{'text': "Ví dụ prompt"}]}],
    inferenceConfig={'maxTokens': 1024}
)

print(response['output']['message']['content'][0]['text'])
