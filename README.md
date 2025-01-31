# DeepSeek-R1-Distill-Qwen-14B with vLLM (8-bit Quantization)

This repository provides a simple setup to run **DeepSeek-R1-Distill-Llama-70B** locally using **vLLM** with **8-bit quantization**

Requires 29G of disk space to store model weights in `./.cache/huggingface/hub/models--deepseek-ai--DeepSeek-R1-Distill-Qwen-14B`

## Features
- **Uses vLLM** for fast inference and efficient memory usage.
- **No need to clone DeepSeek’s repo**—everything is pulled from Hugging Face.
- **Supports multi-GPU setups** with tensor parallelism.

---

### Start the vLLM Server with 8-bit Quantization
```bash
export CUDA_VISIBLE_DEVICES=1,2
vllm serve deepseek-ai/DeepSeek-R1-Distill-Qwen-14B \
  --dtype bfloat16 \
  --tensor-parallel-size 2 \
  --max-model-len 4096
```

### Prompt via curl in a new terminal
```bash
curl http://localhost:8000/v1/completions \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "model": "deepseek-ai/DeepSeek-R1-Distill-Qwen-14B",
    "prompt": "蔡徐坤擅长唱跳和什么？",
    "temperature": 0.6,
    "max_tokens": 512
  }'
```

### Example response
```bash
"id":"cmpl-787e8b70b","object":"text_completion","created":1738287831,"model":"deepseek-ai/DeepSeek-R1-Distill-Qwen-14B","choices":[{"index":0,"text":" 蔡徐坤擅长唱跳和什么？\n好，用户问蔡徐坤擅长唱跳和什么。首先，我需要确认蔡徐坤的主要才能。他确实是唱跳歌手，所以唱跳肯定是他的强项。接下来，他还有什么特别的技能呢？\n\n我记得他参加过《偶像练习生》节目，表现出色，所以可能还有其他的才能。比如，他有没有特别擅长的舞蹈风格？或者他在音乐创作方面有贡献？\n\n另外，蔡徐坤可能在其他领域也有涉猎，比如时尚或者主持。这些可能也是他的强项。需要详细列举出来，让用户全面了解。\n\n所以，整理一下，我会回答说他除了唱跳，还在音乐创作、舞蹈编排、时尚风格和综艺节目主持方面有出色表现。这样用户就能清楚他不仅是一个唱跳歌手，还有多方面的才华。\n</think>\n\n蔡徐坤擅长唱跳，同时也具备音乐创作、舞蹈编排、时尚风格和综艺节目主持等方面的才能。他在音乐方面有着独特的创作能力，常常参与歌曲的作词和作曲。此外，他在舞蹈方面有着极高的造诣，经常为自己的歌曲编排出独特的舞蹈动作。蔡徐坤的时尚感也很强，常常在社交媒体上分享自己的穿搭，受到粉丝们的喜爱。此外，他还参与了许多综艺节目的录制，展现了他在综艺主持方面的才华和幽默感。","logprobs":null,"finish_reason":"stop","stop_reason":null,"prompt_logprobs":null}],"usage":{"prompt_tokens":10,"total_tokens":295,"completion_tokens":285,"prompt_tokens_details":null}}
```
