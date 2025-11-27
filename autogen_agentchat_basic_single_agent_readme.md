# Basic Single Agent — Autogen AgentChat

This repository contains a minimal example to run a single `AssistantAgent` using the `autogen-agentchat` and `autogen-ext` libraries with OpenAI model client.

> **Note:** This README assumes your script is named `1-basic_single_agent.py` and that your `.env` file containing the `OPENAI_API_KEY` is in the **same folder** as the script (the default in this example).

---

## Prerequisites

- Python 3.11+ recommended
- A valid OpenAI API key
- Recommended to run inside a virtual environment (venv)

## Install dependencies

```bash
pip install autogen-agentchat autogen-ext openai python-dotenv
```

You may have a `requirements.txt` in the folder; if so, you can instead run:

```bash
pip install -r requirements.txt
```

## Project structure (example)

```
Basic Single Agent/
├─ .env                    # contains OPENAI_API_KEY (see below)
├─ 1-basic_single_agent.py # main script
├─ requirements.txt        # optional
└─ .venv/                  # virtual environment (optional)
```

## .env file

Create a `.env` file in the same folder as `1-basic_single_agent.py` with the following content (no quotes):

```
OPENAI_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**Important:** Do not commit your `.env` file or API key to version control. Add `.env` to `.gitignore`.

## Usage

1. Activate your virtual environment (Windows PowerShell example):

```powershell
.\.venv\Scripts\activate
```

2. Run the script:

```powershell
python .\1-basic_single_agent.py
```

3. When prompted, enter a task (a single-turn instruction). Example tasks listed at the bottom of the script include career guidance prompts.

## How the script loads the API key

The script uses `python-dotenv` to load environment variables from the `.env` file with:

```python
load_dotenv('.env')
api_key = os.getenv('OPENAI_API_KEY')
if not api_key:
    raise ValueError('OPENAI_API_KEY not found in the .env file in this folder.')
os.environ['OPENAI_API_KEY'] = api_key
```

This ensures the `OPENAI_API_KEY` environment variable is available to the `autogen-ext` / `openai` libraries.

## Configuration notes

- `model` parameter in `OpenAIChatCompletionClient` is set to `gpt-4o-mini` in the example. Change it to any model you have access to.
- `endpoint` is currently set to `https://models.inference.ai.azure.com` in the example — adjust or remove if you're using the standard OpenAI endpoint.
- If your `.env` lives in a parent folder or elsewhere, update `load_dotenv()` with the correct path, e.g. `load_dotenv(os.path.join('..', '.env'))`.

## Troubleshooting

- ``** / **``** errors**

  - Confirm `.env` file exists in the folder and contains `OPENAI_API_KEY=...` with no quotes.
  - Confirm file name is truly `.env` (Windows may hide extensions).
  - Use an absolute path with `load_dotenv()` for debugging.

- **Permission / network errors**

  - Ensure your API key is valid and has not been revoked.
  - Check network connectivity.

- **PowerShell path issues (spaces / special characters)**

  - Wrap paths in quotes when running scripts if your folder names contain spaces or special characters:
    ```powershell
    python "Basic Single Agent\1-basic_single_agent.py"
    ```

## Example tasks

The script includes example user tasks you can try when prompted:

1. "I'm a college student studying computer science, but I'm not sure what career path to follow."
2. "What are the best practices making my resume stand out from the crowd?"
3. "What is the weather today in Bangalore?" (outside the agent's domain — agent should redirect politely)
4. "Can you help me figure out what options might suit my skills and interests?" (multi-turn expected)

## Security and best practices

- Never store secrets in source control. Use environment variables or secret management systems for production.
- Rotate your API key if it gets exposed.
- For production deployments, prefer to set environment variables at the system or deployment level instead of `.env` files.

## License

This project is provided as an example. Use and modify as you need.

---

If you want, I can also generate a `requirements.txt`, a `.gitignore` entry for `.env`, or add a small `Makefile`/PowerShell script to simplify running the example.

