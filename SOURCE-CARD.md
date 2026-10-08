---
license: cc-by-4.0
base_model: moondream/parakeet-redux
pipeline_tag: automatic-speech-recognition
library_name: sherpa-onnx
language: [en, de, fr, es, it, pt, ru, uk, hr, sl, lv, lt, et, fi, sv, da, nl, pl, cs, sk, hu, ro, bg, el, mt]
tags: [sherpa-onnx, onnx, parakeet, tdt, ternary, speech-recognition]
---

# Parakeet Redux for sherpa-onnx

[moondream/parakeet-redux](https://huggingface.co/moondream/parakeet-redux) (the 1.58-bit version of NVIDIA
[parakeet-tdt-0.6b-v3](https://huggingface.co/nvidia/parakeet-tdt-0.6b-v3), 25 European languages) packaged in the
sherpa-onnx NeMo TDT layout, so it loads with `model_type: nemo_transducer` like
`sherpa-onnx-nemo-parakeet-tdt-0.6b-v3-int8`.

`sherpa-onnx-nemo-parakeet-redux.tar.bz2` (169 MB, 407 MB extracted) contains:

- `encoder.onnx`: the ONNX export by [eschmidbauer/parakeet-redux-onnx](https://huggingface.co/eschmidbauer/parakeet-redux-onnx)
  (ternary weights as ONNX Runtime `MatMulNBits` 4-bit blocks, which reproduce -1/0/+1 exactly) with sherpa-onnx
  metadata added.
- `decoder.onnx`, `joiner.onnx`: that export's `decoder_joint-model.onnx` split into sherpa-onnx's two-model interface
  (float32, identical outputs).
- `tokens.txt`, and `test_wavs/` from sherpa-onnx's Parakeet package.

Checked with sherpa-onnx 1.13.8: the en/de/es/fr test clips transcribe exactly like the int8 Parakeet TDT model.
Needs an ONNX Runtime build with the `com.microsoft` `MatMulNBits` contrib op (sherpa-onnx's default builds have it).

```python
import sherpa_onnx
d = "sherpa-onnx-nemo-parakeet-redux/"
rec = sherpa_onnx.OfflineRecognizer.from_transducer(
    encoder=d + "encoder.onnx", decoder=d + "decoder.onnx", joiner=d + "joiner.onnx",
    tokens=d + "tokens.txt", model_type="nemo_transducer")
```

License: CC-BY-4.0. Original model by NVIDIA; ternary version by Moondream; ONNX export by eschmidbauer.
