# 10-Minute Quick Start: Your First Multimodal Agent in M365 Copilot

Get up and running with a real, working agent in 10 minutes. No theory—just execution.

## Prerequisites (2 minutes)

* Access to Microsoft 365 with Copilot enabled
* A web browser (Edge, Chrome, or Safari)
* An image file ready (screenshot, diagram, or photo)

**Don't have M365 Copilot yet?** See [SETUP_GUIDE.md](setup-guide.md) (coming soon).

## The Scenario

You're a project manager. You need an agent that:
1. Takes a project screenshot
2. Extracts action items from the image
3. Formats them as a checklist

**Time**: 10 minutes end-to-end.

## Step 1: Open Microsoft Copilot Studio (1 minute)

1. Go to [copilot.microsoft.com](https://copilot.microsoft.com)
2. Click **"Create"** → **"New Agent"**
3. Name it: **"Project Action Extractor"**
4. Description: *"Analyzes project images and extracts action items"*
5. Click **Create**

## Step 2: Add Your First Instruction (2 minutes)

In the **Agent Builder** canvas, find the **System Instructions** panel:

```
You are a project management assistant. When users upload project screenshots or images:

1. Analyze the image for action items, tasks, or decisions
2. Extract all visible items (from headers, lists, sticky notes, etc.)
3. Format as a numbered checklist
4. Flag items marked "blocked" or "urgent" with 🚫 or ⭐
5. Ask for clarification if the image is unclear

Be concise. Return only the checklist unless asked for details.
```

Copy and paste this. Click **Save**.

## Step 3: Enable Image Understanding (1 minute)

1. Go to **Settings** → **Features**
2. Toggle **"Vision/Image Input"** to **On**
3. Under **Input Modalities**, ensure **Image** is checked
4. Save

## Step 4: Test It (4 minutes)

1. Click **Test Agent** (bottom right)
2. In the test panel, upload an image or screenshot (or use this example):
   * Try a screenshot of a spreadsheet, to-do list, or whiteboard photo
   * Or describe an image: *"I have a screenshot of my sprint board with three columns: To Do, In Progress, Done"*
3. The agent should respond with an extracted checklist

**Example output:**
```
📋 Action Items from Sprint Board

1. Finalize API documentation ⭐
2. Code review for authentication module
3. Schedule customer demo 🚫 (blocked on design)
4. Update test coverage to 85%
5. Deploy to staging
```

## Step 5: Deploy to Teams (2 minutes optional)

Want to share it with your team?

1. Click **Publish** (top right)
2. Select **Microsoft Teams**
3. Add to your team channel
4. Team members can now use it directly in Teams

---

## What Just Happened?

✅ You created an agent that understands **both text and images**
✅ You gave it **specific instructions** for a real workflow
✅ You **tested** it with visual input
✅ You can **deploy** it to your team in 2 clicks

**That's multimodal AI in action.**

---

## Next Steps

* **Learn more**: Head to [tutorials/m365-agent-tutorial.md](tutorials/m365-agent-tutorial.md) for advanced techniques
* **Add governance**: Review [governance/responsible-ai.md](governance/responsible-ai.md) before sharing with your org
* **Troubleshooting**: If the agent doesn't recognize images, see [TROUBLESHOOTING.md](troubleshooting.md) (coming soon)

## Stuck?

**Agent returns gibberish**:
- Try clearer instructions: *"Return ONLY a numbered list"*
- Test with a simpler image first

**Image won't upload**:
- Check file size (<10 MB)
- Try JPG or PNG format
- Ensure image has clear text or visuals

**Want to customize more**:
- [View the full M365 Copilot tutorial](tutorials/m365-agent-tutorial.md)
- [Explore agentic AI guardrails](governance/agentic-ai-guardrails.md)

---

**Congrats!** You've just built your first multimodal agent. 🚀
