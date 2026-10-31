# CSCI 645 HW2 checkpoints

Model checkpoints for Yan Wen's imitation-learning homework on PandaPickCube.

Download the five assets from the
[checkpoint release](https://github.com/ywen0511/CSCI645-HW2-checkpoints/releases/tag/hw2-v2-checkpoints).

| Bundle | Models |
|---|---|
| `hw2_ckpt_act_quantity.tar` | ACT with 25, 50 and 100 clean demonstrations, seeds 0–2 |
| `hw2_ckpt_act_noise.tar` | ACT with teacher action noise 0.5, 1 and 2, seeds 0–2 |
| `hw2_ckpt_flow_and_teacher.tar` | Flow matching with 100 clean demonstrations, seeds 0–2, and the retrained PPO teacher |

The release also includes `checkpoints_manifest.json` and `SHA256SUMS.txt`.
The 21 students were trained on the demonstration datasets in the submission.
These datasets come from the original teacher. The supplied PPO teacher was
trained separately for evaluation and diagnostics.

## Download and verify

Run these Bash commands from the submission's `WEN_YAN_HW2/` directory:

```bash
set -e
export HW2_RUNS="${HW2_RUNS:-$PWD/runs}"
DL="$HW2_RUNS/_download"
mkdir -p "$DL"
BASE=https://github.com/ywen0511/CSCI645-HW2-checkpoints/releases/download/hw2-v2-checkpoints
for f in hw2_ckpt_act_quantity.tar hw2_ckpt_act_noise.tar hw2_ckpt_flow_and_teacher.tar checkpoints_manifest.json SHA256SUMS.txt; do
  curl -fL --retry 3 -o "$DL/$f" "$BASE/$f"
done
if command -v sha256sum >/dev/null 2>&1; then
  (cd "$DL" && sha256sum -c SHA256SUMS.txt)
else
  (cd "$DL" && shasum -a 256 -c SHA256SUMS.txt)
fi
cmp results/checkpoints_manifest.json "$DL/checkpoints_manifest.json"
for f in hw2_ckpt_act_quantity.tar hw2_ckpt_act_noise.tar hw2_ckpt_flow_and_teacher.tar; do
  tar -xf "$DL/$f" -C "$HW2_RUNS"
done
```

Student weights are extracted to `conditions/<model>/policy.pt`, with their
normalizer, identity and training log. Teacher files are extracted to `teacher/`.
Use the submission README for environment setup and evaluation commands.
