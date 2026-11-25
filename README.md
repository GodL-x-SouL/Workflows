# Kaggle Notebooks

A growing vault of self-contained Kaggle notebooks for running AI tools on cloud GPUs. Each notebook launches a browser UI through a temporary Cloudflare Quick Tunnel.

## Notebooks

| Notebook | Description | GPU |
|---|---|---|
| [Strata Studio](strata_kaggle.ipynb) | Browser-first manager for installing, running, and chatting with local models. | Optional (needed for inference) |
| [Unsloth Studio](unsloth_studio_kaggle.ipynb) | One-cell Unsloth Studio setup with automatic password capture and `/tmp` model storage. | Recommended (T4 x2 or P100) |

## Common setup

1. Open the notebook in Kaggle.
2. Enable **Internet** in the notebook settings.
3. Enable a **GPU** for inference.
4. Run the cells, then open the generated Cloudflare link.

Kaggle sessions are temporary: tunnels, credentials, and `/tmp` storage disappear when the session ends. Keep notebooks private and clear outputs before saving or sharing.

## Strata Studio

Strata Studio opens before an engine or model exists. From the browser, you can detect the GPU, install the matching engine, download and load a model, tune it, chat with it, or unload and delete it. The WebUI host stays on loopback; only the authenticated Studio manager gateway is tunneled. No `strata launch` step is used.

**Run order**

1. **01 - Install WebUI** → wait for `READY`. Installs Node 24.12+ if needed, clones Strata and Strata-WebUI, and builds the UI. No engine or model is downloaded.
2. **01b - Prepare Python** → wait for `READY`. Creates or repairs the Python 3.10+ environment.
3. **02 - Start Studio** → wait for `LIVE`. Opens the tunnel and displays the **Studio key**.
4. Open the private link, paste the Studio key, then use **Setup** → **Models** → **Chat**.

Interrupt cell 02 to return to the notebook without stopping Studio. Run **03 - Stop** for a clean shutdown; models and support files are kept.

**Storage and logs**

- Models and caches: `/kaggle/tmp/strata-models/` (cleared at session end).
- Source, environment, and engine files: `/kaggle/working/`.
- Host log: `/kaggle/working/.strata-webui/host.log`.

The Studio key only controls the manager; it is not an inference API key. It changes when the host restarts.

## Unsloth Studio

A single-cell notebook that installs Unsloth Studio, starts it on port `8889`, verifies local health, and opens a Cloudflare Quick Tunnel. A live UI box streams logs and automatically captures the generated password.

**Run**

1. Upload the notebook to Kaggle.
2. Enable **GPU** (T4 x2 or P100 recommended) and **Internet**.
3. Run the cell and wait for the tunnel link.
4. Click **Open Unsloth Studio** and sign in with the captured password (default: `unsloth123`).

**Storage**

Model downloads and caches are redirected to Kaggle's larger `/tmp` disk:

```text
HF_HOME=/tmp/huggingface
HF_HUB_CACHE=/tmp/huggingface/hub
TORCH_HOME=/tmp/torch
UNSLOTH_CACHE_DIR=/tmp/unsloth
```

The tunnel link and password are session-scoped. If the link stops working, rerun the cell.

## Security

Anyone with a tunnel link and its credentials can control the running tool. Do not share live links, keys, or passwords, and clear notebook outputs before committing or sharing.
