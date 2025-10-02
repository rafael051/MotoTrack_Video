# 🏍️ MotoTrack — Detecção & Rastreamento de Múltiplas Motos

## 📌 Visão Geral
O **MotoTrack** é um projeto de **Visão Computacional** que implementa um sistema de **detecção** e **rastreamento** de múltiplas **motocicletas em tempo real**.

Foi desenvolvido para atender ao **caso de uso da disciplina**:

> *“Script funcional de rastreamento ou detecção de múltiplas motos com output visual com as detecções destacadas em tempo real com uso de algoritmos como YOLO, MediaPipe, etc.”*

Neste projeto utilizamos **YOLOv8** (Ultralytics) integrado ao **ByteTrack** como solução robusta para detecção e rastreamento de motos.

---

## ✨ Funcionalidades
- ✅ Detecção **apenas de motos** (classe `motorcycle` do dataset COCO)
- ✅ Rastreamento robusto com **IDs persistentes** (ByteTrack)
- ✅ Exibição em tempo real com **bounding boxes** + **IDs**
- ✅ HUD informativo com:
  - FPS em tempo real
  - Quantidade de motos no frame
  - Contagem acumulada de motos únicas
- ✅ **Linha de contagem** opcional (bidirecional A→B / B→A)
- ✅ **ROI** opcional para restringir área de detecção
- ✅ **Gravação de vídeo anotado** em `./output/`
- ✅ Suporte a **Google Colab** (upload de vídeo ou webcam experimental)

---

## 🛠️ Tecnologias Utilizadas
- [Python 3.10+](https://www.python.org/)
- [YOLOv8 (Ultralytics)](https://github.com/ultralytics/ultralytics) — detecção em tempo real
- [ByteTrack](https://github.com/ifzhang/ByteTrack) — rastreamento multiobjeto
- [OpenCV](https://opencv.org/) — processamento de vídeo/imagem
- [Google Colab](https://colab.research.google.com/) — execução em nuvem

---

## 📂 Estrutura do Projeto
```
.
├── MotoTrack_Colab.ipynb       # Notebook final (execução no Google Colab)
├── mototrack.ipynb             # Protótipo inicial
├── moto_tracker.py             # Script standalone (execução local)
├── moto_tracker_lib.py         # Biblioteca reutilizável (YOLO + ByteTrack)
├── app_fastapi.py              # API opcional (stream MJPEG + JSON)
├── requirements.txt            # Dependências
├── motos_fake.mp4              # Vídeo sintético de motos estacionadas
└── output/                     # Vídeos anotados gerados
```

---

## ⚙️ Instalação (Execução Local)
1. Clone este repositório.
2. Crie e ative um ambiente virtual:
```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# Linux/Mac
source .venv/bin/activate
```
3. Instale dependências:
```bash
pip install -r requirements.txt
```

---

## ▶️ Execução

### 🔹 No Google Colab (recomendado para entrega)
1. Abra o arquivo **MotoTrack_Colab.ipynb** no Colab.
2. Rode a célula **Instalação & GPU**.
3. Faça **upload de um vídeo** (`.mp4`) — pode usar o `motos_fake.mp4` incluso ou vídeos reais de motos.
4. Rode a célula de **Processamento** para visualizar os resultados em tempo real.
5. Baixe o vídeo anotado em **./output/**.

### 🔹 Local (script standalone)
Rodar com **webcam**:
```bash
python moto_tracker.py
```
Rodar com **vídeo**:
```bash
python moto_tracker.py --source caminho/video.mp4
```

### 🔹 API (opcional)
```bash
uvicorn app_fastapi:app --host 0.0.0.0 --port 8000 --reload
```
- Stream MJPEG: `http://localhost:8000/stream.mjpg`
- Metadados JSON: `http://localhost:8000/meta`

---

## 🎮 Controles (script local)
- `q` → sair
- `p` → pausar/retomar
- `s` → salvar frame atual em `./frames/`

---

## 📊 Exemplos de Uso

### ROI (Região de Interesse)
```python
rcfg.enable_roi = True
rcfg.roi_polygon = [(100,200),(1180,200),(1180,700),(100,700)]
```

### Linha de Contagem
```python
rcfg.enable_count_line = True
rcfg.count_line = ((200,480),(1100,480))
```

---

## 🎥 Vídeo Sintético Incluído
Para validar o pipeline no Colab, incluímos um vídeo gerado artificialmente:
- **`motos_fake.mp4`** → mostra 5 “motos estacionadas” (retângulos pretos) sobre fundo cinza.
- Serve para testar **upload, leitura, HUD, rastreamento e gravação**.
- Observação: o YOLO não vai detectar esses retângulos como motos (pois não são objetos reais).

---

## 👨‍💻 Desenvolvedores
- Rafael Rodrigues de Almeida — RM557837
- Lucas Kenji Miyahira - RM555368

---

## ✅ Conclusão
O projeto **MotoTrack** cumpre integralmente os requisitos do **Caso de Visão Computacional**:
- Detecção e rastreamento em tempo real de múltiplas motos
- Output visual com bounding boxes, IDs, FPS e contagens
- Gravação de evidências em vídeo
- Execução compatível com **Google Colab** para avaliação
