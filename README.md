# TransNetV2 em ONNX (janela de 400 quadros)

Export para ONNX dos pesos do **TransNetV2** (detecção de troca de cena), para rodar na CPU com OpenVINO ou ONNX
Runtime, sem PyTorch nem TensorFlow.

- **Arquivo:** `transnetv2-L400.onnx` (33.424.842 bytes)
- **SHA-256:** `f42966e8ab1d5cf8771c703b139b9eca7d67aaceea98ece2381d3cb6cd5d3a8a`
- **Entrada:** `frames`, uint8 `[1, 400, 27, 48, 3]`: 400 quadros RGB reduzidos a 48x27.
- **Saída:** `single`, float32 `[1, 400, 1]`: logits da troca de cena por quadro. A probabilidade é `sigmoid(logit)`;
  o limiar oficial é 0,5.
- **Uso em fluxo:** janelas de 400 quadros, das quais valem os 350 do meio (25 de contexto de cada lado). O início e o
  fim do vídeo são completados repetindo o primeiro e o último quadro, como no `predict_frames` oficial.

## Origem

1. Modelo e pesos originais: Tomáš Souček e Jakub Lokoč, *TransNet V2: An effective deep network architecture for
   fast shot transition detection* (2020). Código e pesos em https://github.com/soCzech/TransNetV2, licença MIT.
2. Pesos convertidos para PyTorch: https://huggingface.co/Sn4kehead/TransNetV2 (revisão
   `db6ceabeb692ec71ecc6beb6d00db67ad1412d7f`), licença Apache-2.0.
3. Este arquivo: export dos pesos do item 2 com `torch.onnx.export` (opset 17), saída "single" apenas, janela fixa de
   400 quadros. Nenhum peso foi alterado nem re-treinado.

Conferência do export: em 174 vídeos de teste, as trocas detectadas com este ONNX no OpenVINO (f32) foram idênticas
às do modelo em PyTorch em 174/174 vídeos.

## Licenças

O arquivo do modelo é distribuído sob a **Apache-2.0** (`LICENSE`), a licença da conversão do item 2. O trabalho
original é MIT, e o aviso dele vai abaixo, como a MIT pede. Os direitos sobre a arquitetura e os pesos são dos autores
originais; este repositório só redistribui o export.

```
MIT License

Copyright (c) 2020 Tomáš Souček

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## Citação

```
@article{soucek2020transnetv2,
  title={TransNet V2: An effective deep network architecture for fast shot transition detection},
  author={Sou{\v{c}}ek, Tom{\'a}{\v{s}} and Loko{\v{c}}, Jakub},
  journal={arXiv preprint arXiv:2008.04838},
  year={2020}
}
```
