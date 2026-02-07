# AlbSub Setup Guide for GitHub Codespaces

This guide walks you through setting up the `albsub` subtitle translation tool in GitHub Codespaces.

## Prerequisites

- GitHub account with access to Codespaces
- An API key from one of these providers:
  - **Anthropic** (Claude) - Recommended for best quality
  - **OpenAI** (GPT-4) - Good alternative
  - **OpenRouter** - Has free tier options
  - **Ollama** - Free local models (limited by Codespace resources)

---

## Step 1: Open the Repository in Codespaces

1. Go to https://github.com/IrdiZ/albsub
2. Click the **Code** button (green)
3. Select **Codespaces** tab
4. Click **Create codespace on master**
5. Wait for the environment to load

---

## Step 2: Install Dependencies

Once your Codespace is ready, run:

```bash
npm install
```

This will install all required Node.js packages including TypeScript, the LLM SDKs, and other dependencies.

---

## Step 3: Build the Project

Compile the TypeScript source code:

```bash
npm run build
```

This creates the compiled JavaScript files in the `dist/` directory.

---

## Step 4: Link the Command Globally

Make the `albsub` command available system-wide:

```bash
npm link
```

This creates a symbolic link so you can run `albsub` from anywhere in your terminal.

---

## Step 5: Verify Installation

Check that the command is working:

```bash
albsub --help
```

You should see the help menu with available commands and options.

---

## Step 6: Configure Your API Key

Choose one of the following options based on your preferred LLM provider:

### Option A: Anthropic (Claude) - Recommended

**Best for:** Highest quality Albanian translations

1. Get your API key from https://console.anthropic.com/
2. Set the environment variable:

```bash
export ANTHROPIC_API_KEY="sk-ant-api03-YOUR_ACTUAL_KEY_HERE"
```

3. Test the setup:

```bash
albsub translate test.srt -o test.alb.srt --language en --provider anthropic
```

**Note:** You'll need to add credits to your Anthropic account (minimum $5).

---

### Option B: OpenAI (GPT-4)

**Best for:** Good quality, widely available API

1. Get your API key from https://platform.openai.com/api-keys
2. Set the environment variable:

```bash
export OPENAI_API_KEY="sk-proj-YOUR_ACTUAL_KEY_HERE"
```

3. Test the setup:

```bash
albsub translate test.srt -o test.alb.srt --language en --provider openai --model gpt-4o
```

---

### Option C: OpenRouter (Free Tier Available)

**Best for:** Testing without payment, decent quality

1. Get a free API key from https://openrouter.ai/
2. Set the environment variables:

```bash
export OPENAI_API_KEY="sk-or-v1-YOUR_OPENROUTER_KEY"
export OPENAI_BASE_URL="https://openrouter.ai/api/v1"
```

3. Test with a free model:

```bash
albsub translate test.srt -o test.alb.srt --language en --provider openai --model meta-llama/llama-3-8b-instruct:free
```

---

### Option D: Ollama (Local Models) - ⚠️ Not Recommended for Codespaces

**Best for:** Privacy, no API costs (but requires powerful hardware)

**Warning:** Standard Codespaces (2-core, 8GB RAM) cannot run most Ollama models. You'll encounter out-of-memory errors with models like Llama3.

If you want to try anyway:

1. Install Ollama:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

2. Start Ollama server:

```bash
ollama serve &
```

3. Pull a **small** model (phi3 is the smallest viable option):

```bash
ollama pull phi3
```

4. Use with albsub:

```bash
albsub translate test.srt -o test.alb.srt --language en --provider ollama --model phi3
```

**Expected Issues:**
- `Load failed` errors with larger models (llama3, mistral)
- Very slow processing
- Lower translation quality compared to Claude/GPT-4

**Alternative:** If you need local models, run this on your own machine instead of Codespaces.

---

## Troubleshooting

### Error: "Could not resolve authentication method"

**Cause:** API key not set

**Fix:**
```bash
# Check if key is set
echo $ANTHROPIC_API_KEY

# If empty, set it
export ANTHROPIC_API_KEY="your-actual-key"
```

---

### Error: "400 Your credit balance is too low"

**Cause:** Anthropic account has no credits

**Fix:**
1. Go to https://console.anthropic.com/settings/billing
2. Add a payment method
3. Purchase credits (minimum $5)

Or switch to a different provider with credits.

---

### Error: "Ollama error: 500 Internal Server Error" or "Load failed"

**Cause:** Codespace doesn't have enough memory to run the Ollama model

**Fix:**
1. Try a smaller model: `ollama pull phi3`
2. Or switch to API-based provider (Anthropic/OpenAI/OpenRouter)
3. Or upgrade your Codespace to 4-core, 16GB (costs money)

---

### Error: "command not found: albsub"

**Cause:** The `npm link` step didn't complete successfully

**Fix:**
```bash
cd /workspaces/albsub
npm link
```

---

## Recommended Setup for Best Results

For high-quality Albanian subtitle translation:

1. **Provider:** Anthropic (Claude Sonnet 4)
2. **Batch size:** 25-50 blocks (default: 50)
3. **Workers:** 2-4 for parallel processing
4. **Cost:** ~$0.50-2.00 per movie (depending on subtitle length)

```bash
export ANTHROPIC_API_KEY="sk-ant-your-key"
albsub translate movie.srt -o movie.alb.srt --language it --provider anthropic --workers 3
```

---


**Made by following the setup experience in GitHub Codespaces**
