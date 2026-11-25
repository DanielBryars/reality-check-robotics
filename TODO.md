# Install TODO — Reality Check Robotics Hackathon (28–29 Nov 2026, London)

Event: https://luma.com/aiengine-r88j · Site: https://www.aiengine.build/reality-check-hack

**Caveat:** the organisers have not published a setup guide, repo, or version pins
("Access Provisioning: Methods will be shared with the participants"). This list is
inferred from the event's inventory page: NVIDIA Isaac Sim (simulation), Modal (cloud
GPU training), Hugging Face / LeRobot (listed as partner), Dell GB10 boxes (offline
local evaluation), Whistle Robotics arms (final). Anything marked *(guess)* may turn
out to be unnecessary. Check the Discord (https://discord.gg/7hHVMbH2h) before the
weekend for an official list, and let it override this one.

Tick one box per machine: **L** = Linux laptop, **U** = Ubuntu desktop, **W** = Windows.

## 0. Before installing anything

| L | U | W | Task |
|---|---|---|------|
| ☐ | ☐ | ☐ | Note GPU model + VRAM (`nvidia-smi`). Isaac Sim needs an NVIDIA RTX GPU; without one, that machine can only do Modal / remote work |
| ☐ | ☐ | ☐ | Free up disk: aim for 100 GB+ (Isaac Sim, model weights ~3B params, datasets, Docker images) |
| ☐ | – | – | Join the Discord and look for a setup channel / pinned repo |

## 1. Accounts and logins (do once, then log in on each machine)

| L | U | W | Task |
|---|---|---|------|
| ☐ | ☐ | ☐ | GitHub SSH key on this machine, `git clone` of this repo works |
| ☐ | ☐ | ☐ | Hugging Face account + access token; `hf auth login` |
| ☐ | ☐ | ☐ | Modal account; `modal setup` (event credits will probably be attached to a workspace on the day) |
| ☐ | ☐ | ☐ | Weights & Biases account + `wandb login` *(guess — handy for overnight training runs)* |
| ☐ | – | – | NVIDIA developer / NGC account *(guess — needed for some Isaac containers and GR00T assets)* |

## 2. Base tooling

| L | U | W | Task |
|---|---|---|------|
| ☐ | ☐ | ☐ | git + git-lfs (`sudo apt install git git-lfs && git lfs install`; Windows: Git for Windows includes LFS) |
| ☐ | ☐ | ☐ | uv (Python + venv manager) — Linux: `curl -LsSf https://astral.sh/uv/install.sh \| sh`; Windows: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 \| iex"` |
| ☐ | ☐ | ☐ | Python 3.10, 3.11 and 3.12 available via uv (`uv python install 3.10 3.11 3.12`) — Isaac Sim and LeRobot pin different versions |
| ☐ | ☐ | – | `sudo apt install build-essential cmake ffmpeg tmux htop nvtop` |
| – | – | ☐ | ffmpeg (`winget install Gyan.FFmpeg`) |
| – | – | ☐ | WSL2 + Ubuntu (`wsl --install`) — robot-learning repos generally assume Linux; repeat the Linux column inside WSL. Note Isaac Sim itself must run on native Windows, not in WSL |
| ☐ | ☐ | ☐ | Hugging Face CLI: `uv tool install "huggingface_hub[cli]"` |
| ☐ | ☐ | ☐ | Modal CLI: `uv tool install modal` |

## 3. GPU stack

| L | U | W | Task |
|---|---|---|------|
| ☐ | ☐ | ☐ | Recent NVIDIA driver; `nvidia-smi` works |
| ☐ | ☐ | ☐ | PyTorch with CUDA in a test venv: `python -c "import torch; print(torch.cuda.is_available())"` prints `True` |
| ☐ | ☐ | ☐ | Docker (Windows: Docker Desktop with WSL2 backend) |
| ☐ | ☐ | – | NVIDIA Container Toolkit; `docker run --rm --gpus all ubuntu nvidia-smi` works |

## 4. Simulation — NVIDIA Isaac Sim (the qualifier runs in sim)

Only on machines with an RTX GPU. Follow the current install docs rather than
commands from memory: https://docs.isaacsim.omniverse.nvidia.com/

| L | U | W | Task |
|---|---|---|------|
| ☐ | ☐ | ☐ | Run the Isaac Sim Compatibility Checker |
| ☐ | ☐ | ☐ | Install Isaac Sim (pip-in-venv or workstation install) and launch it once — first launch downloads/caches a lot, do this at home not on venue wifi |
| ☐ | ☐ | ☐ | Install Isaac Lab on top and run one of its example manipulation environments *(guess — likely what the shared sim environment is built on)* |

## 5. Robot learning stack *(guesses until the base model is announced)*

| L | U | W | Task |
|---|---|---|------|
| ☐ | ☐ | ☐ | LeRobot: clone https://github.com/huggingface/lerobot, install into a venv, run one example training script for a few steps |
| ☐ | ☐ | ☐ | Rerun viewer (`uv tool install rerun-sdk`) for inspecting tele-op datasets |
| ☐ | ☐ | – | Pre-download a likely ~3B-param open base model so the HF cache is warm and you have read its fine-tuning docs — candidates: NVIDIA Isaac GR00T N1.x (NVIDIA is a partner), π0 / openpi, SmolVLA (much smaller) |

## 6. Things to know rather than install

- **Evaluation is offline on Dell GB10 systems.** These are ARM64 (aarch64) NVIDIA
  boxes, not x86. Your policy must run with no internet: weights bundled, no
  download-on-first-use, no hosted API calls at inference. x86 Docker images will not
  run there — avoid dependencies that have no aarch64 wheels.
- **Training happens on Modal**, so the laptop mainly needs: editor, Modal CLI, sim for
  local testing, and SSH. A weak laptop GPU is workable.
- Devin and OpenAI model access are provided at the event; nothing to install.

## 7. Night-before checklist (laptop)

- ☐ `git pull` this repo; re-check Discord for an official setup guide
- ☐ All logins still valid: `hf auth whoami`, `modal profile current`, `ssh -T git@github.com`
- ☐ Isaac Sim launches; `torch.cuda.is_available()` is `True`
- ☐ Charger, extension lead, mouse (Isaac Sim viewport is miserable on a trackpad), USB-C/Ethernet adapter
- ☐ Remote access to the Ubuntu desktop from the venue (Tailscale or similar + SSH) if you want its GPU available
