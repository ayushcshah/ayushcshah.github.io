---
layout: post
title: "Automating Client Model Updates with AI"
date: 2025-09-22
categories: ai automation dev-productivity
---

## TL;DR/Intro
When backend engineers change how data is structured, client app engineers (iOS, Android, Web) usually spend hours updating boring boilerplate code just so the app can understand that data.  

In this demo:  
- A backend PR is merged.  
- AI detects the model change.  
- A new PR is automatically raised for the client app with the updated decoding logic.  

This saves developers hours of repetitive work, boosts productivity, and lets them focus on building features, not boilerplate.  

## How Much Time Does This Save? Lets Run Some Numbers

Manual updates take time:
- iOS Codable update → ~15 minutes per field  
- Android Kotlin data class update → ~15 minutes per field  
- Web TypeScript interface update → ~10–15 minutes per field  

On average, **~15 minutes per field per client**.

### Example Savings
- **1 field** → 45 minutes saved across 3 clients  
- **5 fields** → ~3.75 hours saved  
- **10 fields** → ~7.5 hours saved  

### Real-World Impact
If your backend team merges **2 PRs per sprint** that each touch 5 fields:
- **3.75 hours × 2 = 7.5 developer hours saved per sprint**  
That’s nearly a **full work day** of productivity gained every sprint, just by eliminating boilerplate updates.


## The Pain: BE–FE Model Sync  
Keeping backend (BE) and frontend (FE) models in sync is tedious. Add a new property to a backend model, and suddenly:  
- iOS needs Codable structs updated.  
- Android needs new Kotlin data classes.  
- Web needs updated TypeScript interfaces.  

Manually updating these across clients is repetitive, error-prone, and slows teams down.  


## The Demo in Action  
In our setup, a **backend PR** that changes models automatically triggers a workflow that updates the corresponding client model code and raises a PR.  

- **BE PR Example:** [UsersWebApp PR #12](https://github.com/ayushcshah/UsersWebApp/pull/12)  
- **Auto-Generated Client PR:** [Sample-App PR #5](https://github.com/ayushcshah/Sample-App/pull/5)  

Instead of devs writing Codable by hand, the AI handles it. Developers just review, merge, and move on.  


## Core Snippets That Make the Magic Happen  

### GitHub Actions Workflow (`client_model_update.yml`)  

```yaml
on:
  pull_request:
    types: [closed]
    branches: [main]

jobs:
  auto-pr:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - name: Check if models folder changed
        run: |
          CHANGED=$(gh api repos/${{ github.repository }}/pulls/${{ github.event.pull_request.number }}/files             --paginate -q '.[].filename' | grep '^models/' || true)
          if [ -z "$CHANGED" ]; then exit 0; fi
```

### Python Script (`update_client_model.py`)  

```python
from openai import OpenAI
client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])

completion = client.chat.completions.create(
    model="gpt-4.1",
    messages=[
        {"role": "system", "content": "You are an expert Swift developer."},
        {"role": "user", "content": f"""
Here is the new backend model:
{be_content}

Here is the current Swift Codable file:
{client_content}

Update the Swift Codable structs so they fully match the backend model.
Preserve formatting. Only output valid Swift code.
"""}
    ]
)

new_code = completion.choices[0].message.content.strip()
```

👉 Full code:  
- [Workflow file](https://raw.githubusercontent.com/ayushcshah/UsersWebApp/refs/heads/main/.github/workflows/client_model_update.yml)  
- [Python script](https://raw.githubusercontent.com/ayushcshah/UsersWebApp/refs/heads/main/automated_scripts/model_update/update_client_model.py)


## Scaling Beyond iOS  
While the demo uses **iOS Codable** as an example, the same process works for:  
- **Android** → auto-update Kotlin data classes.  
- **Web** → auto-update TypeScript interfaces.  


## Beyond JSON: Other Protocols  
This approach isn’t limited to JSON/Codable. With prompt adjustments, the same pipeline can target:  
- **XML**  
- **ProtoBuf / gRPC**  
- **GraphQL** (⚠️ more challenging, but possible with conventions)  


## The Power of Standardization  
The secret sauce? **Keep it simple.**  
- BE and FE models use the same filenames.  
- Folder structures are mirrored.  
- AI doesn’t need to guess — it just transforms one into the other.  

> The idea is to keep it **F\*\*ing** Simple**.  


## Developer Experience Boost 🚀  
- No more manual Codable boilerplate.  
- Automatic PRs for iOS, Android, and Web.  
- Developers just connect data to UI.  
- Faster shipping, fewer bugs, happier teams.  


## 🚀 Call to Action
End with something actionable:
“If you’re maintaining multiple clients, start by mirroring your model file structures. Once standardized, you can plug in automation like this with minimal setup.”


## Final Thoughts  
This demo shows how automation + AI + standardization can transform a dull maintenance task into an invisible background process. Developers get to spend their time on real features, while the pipeline handles the rest.  

The result? Less boilerplate, more creativity, and apps that ship faster.  

