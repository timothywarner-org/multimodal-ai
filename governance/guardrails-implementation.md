# Technical Guardrails Implementation Guide

Practical guidance for implementing safety controls across multimodal AI platforms.

## Quick Reference

**Implementation Time**: 2-4 weeks basic, 8-12 weeks comprehensive

**Key Components**:
- Input validation (prompt injection detection)
- Content filtering (Azure/Gemini safety APIs)
- Output validation (PII detection, groundedness)
- Rate limiting (per-user quotas)
- Monitoring (real-time alerts)

## 5-Layer Defense Model

```text
Layer 1: Input Validation → Block malicious inputs
Layer 2: Content Filtering → Detect harmful content  
Layer 3: Prompt Controls → Guide AI behavior
Layer 4: Output Validation → Verify responses
Layer 5: Monitoring → Detect anomalies
```

## Critical Implementation Patterns

### 1. Prompt Injection Protection

```python
# Azure Content Safety
async def detect_injection(text):
    result = await content_safety.analyze_text(
        text=text,
        categories=["PromptInjection"]
    )
    return result.severity >= 2  # Block medium+
```

### 2. Content Filters (Azure OpenAI)

```python
filter_config = {
    "hate": {"blocking_level": "medium"},
    "sexual": {"blocking_level": "medium"},
    "violence": {"blocking_level": "high"},
    "self_harm": {"blocking_level": "low"}
}
```

### 3. PII Detection

```python
from presidio_analyzer import AnalyzerEngine

def scan_pii(text):
    analyzer = AnalyzerEngine()
    return analyzer.analyze(
        text=text,
        entities=["PERSON", "EMAIL", "PHONE", "SSN"]
    )
```

### 4. Rate Limiting

```python
# Redis-based rate limiter
async def check_rate_limit(user_id, limit=100):
    key = f"ratelimit:{user_id}"
    count = await redis.incr(key)
    if count == 1:
        await redis.expire(key, 3600)  # 1 hour
    return count <= limit
```

## Platform-Specific Setup

### M365 Copilot

```powershell
# Enable DLP
New-DlpCompliancePolicy -Name "Copilot-Protection"

# Configure information barriers  
New-InformationBarrierPolicy -Name "Segment-Barrier"
```

### Azure OpenAI

```python
deployment = client.deployments.create(
    model="gpt-4o",
    content_filter="default",
    rate_limit_per_minute=60
)
```

### Google Gemini

```python
safety_settings = {
    "HARASSMENT": "BLOCK_MEDIUM_AND_ABOVE",
    "HATE_SPEECH": "BLOCK_MEDIUM_AND_ABOVE"
}
```

## Testing Checklist

- [ ] Prompt injection detection active
- [ ] Content filters configured
- [ ] PII detection enabled
- [ ] Rate limiting implemented
- [ ] Monitoring and alerts configured
- [ ] Incident response procedures documented

## Key Metrics

Track these daily:
- Blocked inputs (prompt injection attempts)
- Content filter activations
- PII detections in outputs
- Rate limit hits
- False positive rate

## Common Issues

**High false positives**: Lower filter sensitivity
**Slow performance**: Cache validation results  
**Bypassed guardrails**: Update detection patterns regularly

## Resources

- [Azure Content Safety Docs](https://learn.microsoft.com/azure/ai-services/content-safety/)
- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [Presidio PII Detection](https://microsoft.github.io/presidio/)

See [Proven Practices](proven-practices.md) for deployment patterns.
