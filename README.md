# sherpa-onnx-nemo-parakeet-redux

**Mirror of [`Codyfederer/sherpa-onnx-nemo-parakeet-redux`](https://huggingface.co/Codyfederer/sherpa-onnx-nemo-parakeet-redux)** (Hugging Face).

[moondream/parakeet-redux](https://huggingface.co/moondream/parakeet-redux) — the 1.58-bit (ternary)
version of NVIDIA's `parakeet-tdt-0.6b-v3`, 25 European languages — packaged in the **sherpa-onnx
NeMo TDT layout**, so it loads with `model_type="nemo_transducer"` like
`sherpa-onnx-nemo-parakeet-tdt-0.6b-v3-int8`.

⚠️ The packaging is **by [Codyfederer](https://huggingface.co/Codyfederer)** (the bundle's own README
signs the conversion as "Vyvo") — this GitHub repo is a byte-identical mirror with the checksum,
license files, and a load test added. The original model card is preserved verbatim in
[SOURCE-CARD.md](SOURCE-CARD.md).

Provenance chain: NVIDIA `parakeet-tdt-0.6b-v3` → Moondream `parakeet-redux` (ternary) →
[eschmidbauer/parakeet-redux-onnx](https://huggingface.co/eschmidbauer/parakeet-redux-onnx) ONNX export
(also mirrored at [github.com/chemcoder-2020/parakeet-redux-onnx](https://github.com/chemcoder-2020/parakeet-redux-onnx)) →
this sherpa-onnx bundle.

## Contents — `sherpa-onnx-nemo-parakeet-redux.tar.bz2`

**169113813 bytes** — sha256 `4f1b06857fa85abab4d7fb14b644a6a98f2e241d92f9e5227f6719e0050417c4`

Extracts (407 MB) to a `sherpa-onnx-nemo-parakeet-redux/` directory containing:

- `encoder.onnx` — the ternary `MatMulNBits` export with sherpa-onnx TDT metadata
- `decoder.onnx`, `joiner.onnx` — float32 split of the export's `decoder_joint-model.onnx`
- `tokens.txt` — vocabulary (identical to sherpa-onnx's parakeet-tdt-0.6b-v3 tokens)
- `test_wavs/` — en / de / es / fr sample clips
- inner `README.md` from the packager

Requires an ONNX Runtime build with the `com.microsoft` `MatMulNBits` contrib op — sherpa-onnx's
default builds (including the pip package) have it; verified below.

## Usage

```python
import sherpa_onnx
d = "sherpa-onnx-nemo-parakeet-redux/"
rec = sherpa_onnx.OfflineRecognizer.from_transducer(
    encoder=d + "encoder.onnx", decoder=d + "decoder.onnx", joiner=d + "joiner.onnx",
    tokens=d + "tokens.txt", model_type="nemo_transducer")
```

## Verified on this mirror (2026-10-08)

- sha256 of the bundle == the checksum published on the HF Hub (verified again on a fresh clone);
  `tar -xjf` extraction is clean.
- Loaded and decoded with **sherpa-onnx 1.13.8** (pip package, macOS arm64 / Apple M1, 4 threads) —
  all four bundled test clips plus a local weather clip:
  - `en.wav` → *"Ask not what your country can do for you, ask what you can do for your country."* — 12.8× realtime
  - `de.wav` → *"Alles hat ein Ende, nur die Wurst hat zwei."* — 11.8×
  - `es.wav` → *"No preguntes qué puede hacer tu país por ti, pregunta qué puedes hacer tú por tu país."* — 12.6×
  - `fr.wav` → *"Ne vous demandez pas ce que votre pays peut faire pour vous, demandez-vous plutôt ce que vous pouvez faire pour lui."* — 13.9×
  - `bcn_weather.wav` (11 s) → *"Yesterday it was 35 degrees in Barcelona, but today the temperature will go down to minus 20 degrees."* — 14.8×
- Short-clip numbers on an M1 (the bundled wavs are resampled from 22–24 kHz automatically); this is
  not a re-run of the packager's test-clip comparison.

## License & attribution

**CC BY 4.0** — give credit to:

- **NVIDIA Corporation** — `parakeet-tdt-0.6b-v3` base model (CC BY 4.0)
- **Moondream** — `parakeet-redux` ternary weights (CC BY 4.0)
- **eschmidbauer** — ONNX export (CC BY 4.0)
- **Codyfederer** — sherpa-onnx packaging (bundle README: "Converted by Vyvo")

See [LICENSE](LICENSE) (verbatim CC BY 4.0) and [NOTICE](NOTICE). Source of truth:
https://huggingface.co/Codyfederer/sherpa-onnx-nemo-parakeet-redux
