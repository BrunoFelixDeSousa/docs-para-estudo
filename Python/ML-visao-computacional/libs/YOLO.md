# [⬅ Voltar para o índice principal](../../../README.md)

# Documentação Completa YOLOv8

## Sumário

1. [Introdução](#introdução)
2. [Instalação](#instalação)
3. [Arquitetura YOLOv8](#arquitetura-yolov8)
4. [Modelos Disponíveis](#modelos-disponíveis)
5. [Uso Básico](#uso-básico)
6. [Detecção de Objetos](#detecção-de-objetos)
7. [Segmentação de Imagens](#segmentação-de-imagens)
8. [Classificação de Imagens](#classificação-de-imagens)
9. [Pose Estimation](#pose-estimation)
10. [Treinamento Personalizado](#treinamento-personalizado)
11. [Inferência](#inferência)
12. [Exportação de Modelos](#exportação-de-modelos)
13. [Métricas de Avaliação](#métricas-de-avaliação)
14. [Otimização de Performance](#otimização-de-performance)
15. [Casos de Uso Avançados](#casos-de-uso-avançados)
16. [Solução de Problemas](#solução-de-problemas)
17. [Recursos e Referências](#recursos-e-referências)

## Introdução

YOLOv8 (You Only Look Once versão 8) é a mais recente evolução da família de modelos YOLO desenvolvida pela Ultralytics. Lançado em 2023, o YOLOv8 representa um avanço significativo em relação aos seus predecessores, oferecendo melhor precisão e velocidade na detecção de objetos em tempo real.

O YOLOv8 não é apenas um detector de objetos - é uma arquitetura de rede neural que suporta múltiplas tarefas de visão computacional:

- **Detecção de objetos**: Identificação e localização de objetos em imagens
- **Segmentação de instâncias**: Criação de máscaras para cada objeto detectado
- **Classificação de imagens**: Categorização de imagens inteiras
- **Pose estimation**: Detecção de pontos-chave do corpo humano

Uma das maiores vantagens do YOLOv8 é sua capacidade de processar imagens com alta velocidade, permitindo aplicações em tempo real como sistemas de vigilância, carros autônomos, análise de vídeo, e muito mais.

## Instalação

A instalação do YOLOv8 é simples e pode ser feita via pip, o gerenciador de pacotes Python. A biblioteca principal é chamada `ultralytics`.

```python
# Instalação via pip
pip install ultralytics

# Verificar a instalação importando a biblioteca
import ultralytics
ultralytics.checks()  # Executa verificações de ambiente

# Atualizar para a versão mais recente
pip install -U ultralytics
```

Alternativamente, você pode instalar a partir do código fonte:

```python
# Instalação a partir do GitHub
pip install git+https://github.com/ultralytics/ultralytics.git
```

### Requisitos de Sistema

- Python 3.7+
- PyTorch 1.7+
- CUDA (opcional, mas recomendado para aceleração por GPU)

## Arquitetura YOLOv8

YOLOv8 introduz várias melhorias arquiteturais em relação às versões anteriores:

1. **Backbone**: Utiliza um CSPDarknet modificado, otimizado para extração de características
2. **Neck**: Implementa um PANet (Path Aggregation Network) para fusão de características em múltiplas escalas
3. **Head**: Adota cabeças de detecção separadas para classificação e regressão de bounding boxes
4. **Loss Function**: Utiliza uma combinação de funções de perda para otimizar diferentes aspectos (IoU, classificação, etc.)

A arquitetura do YOLOv8 foi projetada para balancear:

- Precisão (mAP - mean Average Precision)
- Velocidade (FPS - Frames Por Segundo)
- Tamanho do modelo (parâmetros)

### Principais Inovações

- **Anchor-free Detection**: Elimina o uso de anchor boxes pré-definidas
- **C2f Module**: Módulo de feature fusion melhorado
- **Ativação SiLU**: Substitui LeakyReLU por SiLU (Sigmoid Linear Unit)
- **Detecção de Cabeça Única**: Simplificação do processo de detecção para melhor eficiência

## Modelos Disponíveis

YOLOv8 oferece uma variedade de modelos pré-treinados, permitindo escolher o equilíbrio ideal entre velocidade e precisão:

| Modelo  | Tamanho (pixels) | mAP val 50-95 | Velocidade CPU (ms) | Parâmetros (M) | FLOPs (B) |
| ------- | ---------------- | ------------- | ------------------- | -------------- | --------- |
| YOLOv8n | 640              | 37.3          | 80.4                | 3.2            | 8.7       |
| YOLOv8s | 640              | 44.9          | 128.4               | 11.2           | 28.6      |
| YOLOv8m | 640              | 50.2          | 234.7               | 25.9           | 78.9      |
| YOLOv8l | 640              | 52.9          | 375.2               | 43.7           | 165.2     |
| YOLOv8x | 640              | 53.9          | 479.1               | 68.2           | 257.8     |

Para cada tarefa (detecção, segmentação, classificação, pose), existem versões específicas desses modelos:

- YOLOv8n-seg, YOLOv8s-seg, etc. (segmentação)
- YOLOv8n-cls, YOLOv8s-cls, etc. (classificação)
- YOLOv8n-pose, YOLOv8s-pose, etc. (pose estimation)

## Uso Básico

Vamos explorar o uso básico do YOLOv8 com exemplos simples:

```python
# Importar a biblioteca
from ultralytics import YOLO

# Carregar um modelo pré-treinado
model = YOLO('yolov8n.pt')  # modelo nano para detecção

# Executar inferência em uma imagem
results = model('caminho/para/imagem.jpg')

# Processar resultados
for result in results:
    boxes = result.boxes  # Caixas delimitadoras (x, y, w, h)
    probs = result.probs  # Probabilidades de classe (classificação)
    masks = result.masks  # Máscaras de segmentação
    keypoints = result.keypoints  # Pontos-chave (pose)

# Visualizar resultados
results[0].plot()  # Gera uma visualização dos resultados
```

Este exemplo básico carrega um modelo pré-treinado e realiza inferência em uma imagem. O YOLOv8 retorna objetos `Results` que contêm todas as informações sobre as detecções.

## Detecção de Objetos

A detecção de objetos é a funcionalidade principal do YOLOv8. Vamos explorar isso em detalhes:

```python
# Exemplo completo de detecção de objetos
from ultralytics import YOLO
import cv2
import numpy as np

# Carregar o modelo YOLOv8 para detecção
model = YOLO('yolov8m.pt')  # Usando o modelo médio para melhor equilíbrio precisão/velocidade

# Lista de classes do COCO dataset (usado por padrão)
class_names = model.names

# Processar uma imagem
image_path = 'caminho/para/imagem.jpg'
image = cv2.imread(image_path)
original_image = image.copy()

# Realizar a detecção
# conf: Limiar de confiança (0-1)
# iou: Limiar de IoU para Non-Maximum Suppression (0-1)
# max_det: Máximo de detecções por imagem
results = model(image, conf=0.25, iou=0.45, max_det=300)

# Processar os resultados
detections = results[0]
boxes = detections.boxes.xyxy.cpu().numpy()  # Coordenadas dos boxes em formato (x1, y1, x2, y2)
confidences = detections.boxes.conf.cpu().numpy()  # Valores de confiança
class_ids = detections.boxes.cls.cpu().numpy().astype(int)  # IDs das classes

# Desenhar os boxes na imagem
for box, confidence, class_id in zip(boxes, confidences, class_ids):
    x1, y1, x2, y2 = box.astype(int)
    class_name = class_names[class_id]

    # Definir cor com base na classe (para variedade visual)
    color = (int(hash(class_name) % 256), int(hash(class_name + 'a') % 256), int(hash(class_name + 'b') % 256))

    # Desenhar retângulo
    cv2.rectangle(image, (x1, y1), (x2, y2), color, 2)

    # Adicionar texto com classe e confiança
    label = f"{class_name}: {confidence:.2f}"
    cv2.putText(image, label, (x1, y1 - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, color, 2)

# Mostrar resultados
cv2.imshow("YOLOv8 Detection", image)
cv2.waitKey(0)
cv2.destroyAllWindows()

# Salvar imagem com detecções
cv2.imwrite('deteccoes_yolov8.jpg', image)
```

### Detecção em Vídeo

```python
# Detecção em vídeo em tempo real
from ultralytics import YOLO
import cv2
import time

# Carregar o modelo
model = YOLO('yolov8n.pt')  # Modelo nano para maior velocidade em tempo real

# Abrir a webcam
cap = cv2.VideoCapture(0)  # 0 para webcam padrão

# Verificar se a câmera foi aberta corretamente
if not cap.isOpened():
    print("Erro ao abrir a webcam")
    exit()

# Configurações de vídeo
frame_width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
frame_height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
fps = int(cap.get(cv2.CAP_PROP_FPS))

# Para salvar o vídeo (opcional)
# out = cv2.VideoWriter('output.mp4', cv2.VideoWriter_fourcc(*'mp4v'), fps, (frame_width, frame_height))

# Loop para processar frames
while True:
    # Capturar frame
    ret, frame = cap.read()
    if not ret:
        break

    # Registrar o tempo para cálculo de FPS
    start_time = time.time()

    # Executar detecção
    results = model(frame, conf=0.35)

    # Desenhar resultados no frame
    annotated_frame = results[0].plot()

    # Calcular FPS
    end_time = time.time()
    fps_current = 1 / (end_time - start_time)

    # Exibir informações de FPS
    cv2.putText(annotated_frame, f"FPS: {fps_current:.2f}", (10, 30),
                cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)

    # Mostrar o frame
    cv2.imshow("YOLOv8 Video Detection", annotated_frame)

    # Salvar o frame (opcional)
    # out.write(annotated_frame)

    # Sair com a tecla 'q'
    if cv2.waitKey(1) == ord('q'):
        break

# Liberar recursos
cap.release()
# out.release()  # Descomentar se estiver salvando o vídeo
cv2.destroyAllWindows()
```

### Processamento por Lote (Batch Processing)

```python
# Processamento em lote para maior eficiência
from ultralytics import YOLO
import glob
import os

# Carregar modelo
model = YOLO('yolov8m.pt')

# Obter lista de imagens para processamento
image_paths = glob.glob('pasta_de_imagens/*.jpg')

# Processar imagens em lote
results = model(image_paths, batch=16)  # Processa 16 imagens por lote

# Criar pasta para salvar resultados
os.makedirs('resultados', exist_ok=True)

# Processar e salvar cada resultado
for i, (img_path, result) in enumerate(zip(image_paths, results)):
    # Obter nome do arquivo da imagem original
    filename = os.path.basename(img_path)

    # Desenhar resultados na imagem
    im_with_boxes = result.plot()

    # Salvar imagem com anotações
    save_path = os.path.join('resultados', filename)
    result.save(save_path)

    print(f"Processada imagem {i+1}/{len(image_paths)}: {filename}")
```

## Segmentação de Imagens

A segmentação de instâncias no YOLOv8 vai além da detecção de objetos, criando máscaras precisas para cada objeto detectado.

```python
# Segmentação de instâncias com YOLOv8
from ultralytics import YOLO
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Carregar modelo específico para segmentação
model = YOLO('yolov8m-seg.pt')  # Usar modelo de segmentação

# Processar uma imagem
image_path = 'caminho/para/imagem.jpg'

# Realizar segmentação
results = model(image_path, conf=0.25)

# Extrair resultados da primeira imagem
result = results[0]

# Obter a imagem original
original_image = cv2.imread(image_path)
original_image = cv2.cvtColor(original_image, cv2.COLOR_BGR2RGB)  # Converter BGR para RGB

# Criar uma nova imagem para visualização de máscaras
height, width = original_image.shape[:2]
mask_image = np.zeros((height, width, 3), dtype=np.uint8)

# Processar cada máscara e classe
if result.masks is not None:
    # Para cada objeto detectado
    for i, (mask, box, cls) in enumerate(zip(result.masks.data, result.boxes.data, result.boxes.cls)):
        # Converter máscara tensor para numpy
        mask = mask.cpu().numpy()

        # Redimensionar máscara para o tamanho da imagem
        mask = cv2.resize(mask, (width, height))

        # Criar uma cor única para esta classe
        class_id = int(cls.item())
        class_name = model.names[class_id]
        color = (int(hash(class_name) % 256), int(hash(class_name + 'a') % 256), int(hash(class_name + 'b') % 256))

        # Aplicar máscara colorida
        mask_bool = mask > 0.5  # Limiar para binarizar a máscara
        mask_image[mask_bool] = color

        # Adicionar contorno da máscara na imagem original
        contours, _ = cv2.findContours((mask_bool).astype(np.uint8), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
        cv2.drawContours(original_image, contours, -1, color, 2)

        # Adicionar texto com classe
        x1, y1, x2, y2 = map(int, box[:4])
        cv2.putText(original_image, class_name, (x1, y1 - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, color, 2)

# Criar blend da imagem original com as máscaras
alpha = 0.5
blend = cv2.addWeighted(original_image, 1 - alpha, mask_image, alpha, 0)

# Visualizar resultados
plt.figure(figsize=(15, 10))

plt.subplot(1, 3, 1)
plt.title("Imagem Original")
plt.imshow(original_image)
plt.axis('off')

plt.subplot(1, 3, 2)
plt.title("Máscaras")
plt.imshow(mask_image)
plt.axis('off')

plt.subplot(1, 3, 3)
plt.title("Combinação (Blend)")
plt.imshow(blend)
plt.axis('off')

plt.tight_layout()
plt.savefig('segmentacao_yolov8.png')
plt.show()
```

### Processamento de Segmentação em Vídeo

```python
# Segmentação em vídeo
from ultralytics import YOLO
import cv2
import numpy as np
import time

# Carregar modelo para segmentação
model = YOLO('yolov8n-seg.pt')  # Modelo nano para maior velocidade

# Abrir vídeo
video_path = 'caminho/para/video.mp4'
cap = cv2.VideoCapture(video_path)

# Verificar se o vídeo foi aberto corretamente
if not cap.isOpened():
    print("Erro ao abrir o vídeo")
    exit()

# Configurações do vídeo
frame_width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
frame_height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
fps = int(cap.get(cv2.CAP_PROP_FPS))

# Configurar gravação do vídeo resultante
output_path = 'segmentacao_video_resultado.mp4'
out = cv2.VideoWriter(output_path, cv2.VideoWriter_fourcc(*'mp4v'), fps, (frame_width, frame_height))

# Loop para processar cada frame
while True:
    # Ler frame
    ret, frame = cap.read()
    if not ret:
        break

    # Medir tempo de processamento
    start_time = time.time()

    # Realizar segmentação
    results = model(frame, conf=0.3)

    # Preparar visualização
    result = results[0]

    # Opção 1: Usar o método plot() para visualização rápida
    annotated_frame = result.plot()

    # Opção 2: Personalização manual (mais controle)
    # annotated_frame = frame.copy()
    # if result.masks is not None:
    #     for mask, box, cls in zip(result.masks.data, result.boxes.data, result.boxes.cls):
    #         # Processar cada máscara [código similar ao exemplo anterior]

    # Calcular FPS
    fps_current = 1.0 / (time.time() - start_time)

    # Adicionar informação de FPS
    cv2.putText(annotated_frame, f"FPS: {fps_current:.1f}", (20, 40),
                cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)

    # Mostrar resultado
    cv2.imshow("YOLOv8 Segmentation", annotated_frame)

    # Gravar frame no vídeo de saída
    out.write(annotated_frame)

    # Sair com 'q'
    if cv2.waitKey(1) == ord('q'):
        break

# Liberar recursos
cap.release()
out.release()
cv2.destroyAllWindows()
```

## Classificação de Imagens

YOLOv8 também suporta classificação de imagens completas, usando modelos específicos para esta tarefa.

```python
# Classificação de imagens com YOLOv8
from ultralytics import YOLO
import cv2
import matplotlib.pyplot as plt
import numpy as np

# Carregar modelo específico para classificação
model = YOLO('yolov8m-cls.pt')  # Modelo médio para classificação

# Lista de imagens para classificar
image_paths = [
    'caminho/para/imagem1.jpg',
    'caminho/para/imagem2.jpg',
    'caminho/para/imagem3.jpg'
]

# Configurar visualização
fig, axs = plt.subplots(len(image_paths), 2, figsize=(12, 4*len(image_paths)))

# Processar cada imagem
for i, img_path in enumerate(image_paths):
    # Carregar imagem
    img = cv2.imread(img_path)
    img_rgb = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

    # Realizar classificação
    results = model(img)
    result = results[0]

    # Obter as classes mais prováveis (top 5)
    probs = result.probs.top5conf  # Top 5 probabilidades
    classes = result.probs.top5  # Top 5 classes correspondentes

    # Criar lista de labels com nome da classe e probabilidade
    labels = []
    for j in range(len(probs)):
        class_id = int(classes[j])
        class_name = model.names[class_id]
        probability = float(probs[j])
        labels.append(f"{class_name}: {probability:.2f}")

    # Visualizar imagem original
    axs[i, 0].imshow(img_rgb)
    axs[i, 0].set_title(f"Imagem {i+1}")
    axs[i, 0].axis('off')

    # Visualizar resultado da classificação
    axs[i, 1].barh(np.arange(len(labels)), probs[::-1], color='skyblue')
    axs[i, 1].set_yticks(np.arange(len(labels)))
    axs[i, 1].set_yticklabels(labels[::-1])
    axs[i, 1].set_title("Top 5 Classes")
    axs[i, 1].set_xlim(0, 1)

plt.tight_layout()
plt.savefig('classificacao_yolov8.png')
plt.show()
```

### Uso de Classificação em Aplicações

```python
# Exemplo de aplicação de classificação em um diretório de imagens
from ultralytics import YOLO
import os
import pandas as pd
import glob
import cv2
from tqdm import tqdm

# Carregar modelo de classificação
model = YOLO('yolov8s-cls.pt')

# Diretório contendo imagens
input_dir = 'pasta_de_imagens/'
output_file = 'resultados_classificacao.csv'

# Obter lista de imagens
image_files = glob.glob(os.path.join(input_dir, '*.jpg')) + \
              glob.glob(os.path.join(input_dir, '*.jpeg')) + \
              glob.glob(os.path.join(input_dir, '*.png'))

# Preparar resultados
results_data = []

# Processar cada imagem
for img_path in tqdm(image_files, desc="Classificando imagens"):
    # Nome do arquivo
    filename = os.path.basename(img_path)

    # Realizar classificação
    results = model(img_path)
    result = results[0]

    # Obter classe principal e probabilidade
    top_prob = float(result.probs.top1conf)
    top_class_id = int(result.probs.top1)
    top_class_name = model.names[top_class_id]

    # Obter top 3 classes
    top3_classes = []
    top3_probs = []

    for cls, prob in zip(result.probs.top5[:3], result.probs.top5conf[:3]):
        cls_id = int(cls)
        cls_name = model.names[cls_id]
        top3_classes.append(cls_name)
        top3_probs.append(float(prob))

    # Adicionar aos resultados
    results_data.append({
        'filename': filename,
        'top_class': top_class_name,
        'confidence': top_prob,
        'class2': top3_classes[1] if len(top3_classes) > 1 else '',
        'conf2': top3_probs[1] if len(top3_probs) > 1 else 0,
        'class3': top3_classes[2] if len(top3_classes) > 2 else '',
        'conf3': top3_probs[2] if len(top3_probs) > 2 else 0
    })

# Criar DataFrame e salvar em CSV
df = pd.DataFrame(results_data)
df.to_csv(output_file, index=False)

print(f"Classificação concluída. Resultados salvos em {output_file}")
```

## Pose Estimation

YOLOv8 também oferece modelos para estimativa de pose (pose estimation), detectando pontos-chave do corpo humano.

```python
# Estimativa de pose (pose estimation) com YOLOv8
from ultralytics import YOLO
import cv2
import numpy as np

# Carregar modelo para pose estimation
model = YOLO('yolov8m-pose.pt')

# Abrir a imagem
image_path = 'caminho/para/pessoas.jpg'
image = cv2.imread(image_path)

# Realizar a detecção de poses
results = model(image)
result = results[0]

# Obter a imagem original
original_image = image.copy()

# Verificar se foram detectados keypoints
if result.keypoints is not None:
    keypoints = result.keypoints.data

    # Definir cores para as conexões entre keypoints
    # As conexões são definidas pelos pares de índices de keypoints
    # Esqueleto do COCO: https://github.com/ultralytics/ultralytics/blob/main/ultralytics/utils/plotting.py
    skeleton = [
        [16, 14], [14, 12], [17, 15], [15, 13], [12, 13], [6, 12], [7, 13],
        [6, 7], [6, 8], [7, 9], [8, 10], [9, 11], [2, 3], [1, 2], [1, 3],
        [2, 4], [3, 5], [4, 6], [5, 7]
    ]

    # Cores para visualização
    pose_palette = np.array([
        [255, 128, 0], [255, 153, 51], [255, 178, 102], [230, 230, 0], [255, 153, 255],
        [153, 204, 255], [255, 102, 255], [255, 51, 255], [102, 178, 255], [51, 153, 255],
        [255, 153, 153], [255, 102, 102], [255, 51, 51], [153, 255, 153], [102, 255, 102],
        [51, 255, 51], [0, 255, 0], [0, 0, 255], [255, 0, 0]
    ])

    # Desenhar cada pessoa detectada
    for person_keypoints in keypoints:
        # As coordenadas dos keypoints estão nas primeiras colunas, e a confiança na última
        kpts = person_keypoints[:, :2].cpu().numpy().astype(np.int32)
        conf = person_keypoints[:, 2].cpu().numpy()

        # Desenhar os keypoints
        for i, (x, y) in enumerate(kpts):
            if conf[i] > 0.5:  # Filtrar por confiança
                cv2.circle(original_image, (x, y), 5, pose_palette[i % len(pose_palette)].tolist(), -1)

        # Desenhar as conexões (esqueleto)
        for i, (idx1, idx2) in enumerate(skeleton):
            # Índices do COCO começam em 1, subtrair 1 para obter o índice 0-based
            p1, p2 = kpts[idx1-1], kpts[idx2-1]
            conf1, conf2 = conf[idx1-1], conf[idx2-1]

            if conf1 > 0.5 and conf2 > 0.5:  # Verificar confiança
                color = pose_palette[i % len(pose_palette)].tolist()
                cv2.line(original_image, (p1[0], p1[1]), (p2[0], p2[1]), color, 2)

# Visualizar e salvar resultados
cv2.imwrite('pose_estimation_result.jpg', original_image)
cv2.imshow('Pose Estimation', original_image)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Rastreamento de Pose em Vídeo

```python
# Rastreamento de pose em vídeo
from ultralytics import YOLO
import cv2
import numpy as np
import time

# Carregar modelo de pose
model = YOLO('yolov8n-pose.pt')

# Abrir vídeo ou webcam
video_source = 0  # 0 para webcam, ou um caminho para um arquivo de vídeo
cap = cv2.VideoCapture(video_source)

# Verificar se a fonte de vídeo foi aberta
if not cap.isOpened():
    print("Erro ao abrir a fonte de vídeo")
    exit()

# Configurações de vídeo
frame_width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
frame_height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
fps = int(cap.get(cv2.CAP_PROP_FPS)) if int(cap.get(cv2.CAP_PROP_FPS)) > 0 else 30

# Configurar gravação (opcional)
# out = cv2.VideoWriter('pose_tracking.mp4', cv2.VideoWriter_fourcc(*'mp4v'), fps, (frame_width, frame_height))

# Definir conexões do esqueleto para visualização
skeleton = [
    [16, 14], [14, 12], [17, 15], [15, 13], [12, 13], [6, 12], [7, 13],
    [6, 7], [6, 8], [7, 9], [8, 10], [9, 11], [2, 3], [1, 2], [1, 3],
    [2, 4], [3, 5], [4, 6], [5, 7]
]

# Cores para os keypoints e conexões
pose_palette = np.array([
    [255, 128, 0], [255, 153, 51], [255, 178, 102], [230, 230, 0], [255, 153, 255],
    [153, 204, 255], [255, 102, 255], [255, 51, 255], [102, 178, 255], [51, 153, 255],
    [255, 153, 153], [255, 102, 102], [255, 51, 51], [153, 255, 153], [102, 255, 102],
    [51, 255, 51], [0, 255, 0], [0, 0, 255], [255, 0, 0]
])

# Loop principal para processar frames
while True:
    # Capturar frame
    ret, frame = cap.read()
    if not ret:
        break

    # Registrar tempo para cálculo de FPS
    start_time = time.time()

    # Executar detecção de pose
    # track=True permite o rastreamento dos indivíduos entre frames
    results = model.track(frame, persist=True, conf=0.3, verbose=False)

    # Obter o resultado do primeiro frame
    if results and len(results) > 0:
        result = results[0]

        # Obter a imagem anotada (método simples)
        annotated_frame = result.plot()

        # Alternativa: personalização manual da visualização
        # annotated_frame = frame.copy()
        # if result.keypoints is not None:
        #     keypoints = result.keypoints.data
        #     for person_keypoints in keypoints:
        #         # Implementar visualização personalizada aqui

        # Adicionar IDs de rastreamento (se disponíveis)
        if hasattr(result, 'boxes') and hasattr(result.boxes, 'id') and result.boxes.id is not None:
            tracks = result.boxes.id.int().cpu().numpy().tolist()
            for i, track_id in enumerate(tracks):
                # Obter posição central da pessoa
                box = result.boxes.xyxy[i].int().cpu().numpy()
                x_center = (box[0] + box[2]) // 2
                y_center = (box[1] + box[3]) // 2

                # Adicionar ID do rastreamento
                cv2.putText(annotated_frame, f"ID: {track_id}", (x_center, y_center),
                            cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0, 255, 0), 2)
    else:
        annotated_frame = frame.copy()

    # Calcular e exibir FPS
    fps_value = 1.0 / (time.time() - start_time)
    cv2.putText(annotated_frame, f"FPS: {fps_value:.1f}", (20, 40),
                cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)

    # Mostrar resultado
    cv2.imshow("YOLOv8 Pose Tracking", annotated_frame)

    # Salvar frame no vídeo de saída (opcional)
    # out.write(annotated_frame)

    # Sair com a tecla 'q'
    if cv2.waitKey(1) == ord('q'):
        break

# Liberar recursos
cap.release()
# out.release()  # Descomentar se estiver salvando o vídeo
cv2.destroyAllWindows()
```

## Treinamento Personalizado

Uma das características mais poderosas do YOLOv8 é a facilidade de treinamento em conjuntos de dados personalizados.

### Preparação do Dataset

Para treinar o YOLOv8, você precisa organizar seu conjunto de dados no formato YOLO:

```
dataset/
├── train/
│   ├── images/
│   │   ├── img1.jpg
│   │   ├── img2.jpg
│   │   └── ...
│   └── labels/
│       ├── img1.txt
│       ├── img2.txt
│       └── ...
├── val/
│   ├── images/
│   │   ├── img_val1.jpg
│   │   ├── img_val2.jpg
│   │   └── ...
│   └── labels/
│       ├── img_val1.txt
│       ├── img_val2.txt
│       └── ...
└── data.yaml
```

Os arquivos de label (`.txt`) devem estar no formato YOLO:

- Um objeto por linha
- Formato: `class_id x_center y_center width height`
- Valores normalizados entre 0 e 1
- Coordenadas relativas ao centro do objeto

Exemplo de arquivo `data.yaml`:

```yaml
# Caminho para os diretórios de treino e validação
train: ../train/images
val: ../val/images

# Número de classes
nc: 3

# Nomes das classes
names: ["pessoa", "carro", "cachorro"]
```

### Exemplo de Treinamento

```python
# Treinamento personalizado YOLOv8
from ultralytics import YOLO
import os

# Configurar diretório para salvar os resultados
os.makedirs('runs', exist_ok=True)

# 1. Inicializar modelo
# Opção 1: Partir de um modelo pré-treinado (recomendado)
model = YOLO('yolov8n.pt')  # Carregar modelo nano pré-treinado

# Opção 2: Treinar do zero
# model = YOLO('yolov8n.yaml')  # Inicializar com a configuração do modelo, sem pesos

# 2. Treinar o modelo
results = model.train(
    data='caminho/para/data.yaml',       # Caminho para o arquivo de configuração
    epochs=100,                          # Número de épocas
    imgsz=640,                           # Tamanho das imagens
    batch=16,                            # Tamanho do batch
    name='meu_modelo_yolov8',            # Nome do experimento
    project='meu_projeto',               # Nome do projeto
    patience=20,                         # Early stopping (parar se não houver melhoria após N épocas)
    lr0=0.01,                            # Taxa de aprendizado inicial
    lrf=0.01,                            # Taxa de aprendizado final
    optimizer='Adam',                    # Otimizador (SGD, Adam, AdamW, etc.)
    weight_decay=0.0005,                 # Decaimento de peso
    cos_lr=True,                         # Usar curva de aprendizado cosseno
    close_mosaic=10,                     # Desabilitar mosaico nas últimas N épocas
    augment=True,                        # Usar augmentações
    warmup_epochs=3,                     # Épocas de warmup
    save=True,                           # Salvar checkpoints
    save_period=10,                      # Salvar a cada N épocas
    device=0,                            # Dispositivo (0 para primeira GPU, 'cpu' para CPU)
    workers=8,                           # Número de workers para carregamento de dados
    resume=False,                        # Retomar treinamento de um checkpoint
    freeze=[0, 1, 2]                     # Congelar camadas (útil para fine-tuning)
)

# 3. Avaliar modelo treinado
metrics = model.val()  # Validar no conjunto de validação

# 4. Exportar modelo para diferentes formatos
model.export(format='onnx')  # Exportar para ONNX (opções: torchscript, onnx, openvino, etc.)
```

### Arquivo de Configuração para Treinamento

Para maior controle e reprodutibilidade, você pode criar um arquivo YAML de configuração:

```yaml
# config.yaml
# Parâmetros de treinamento para YOLOv8
task: detect # detect, segment, classify, pose
mode: train

# Dados
data: caminho/para/data.yaml
epochs: 100
patience: 20
batch: 16
imgsz: 640

# Modelo
model: yolov8n.pt # ou yolov8s.pt, yolov8m.pt, etc.
pretrained: True
optimizer: Adam
lr0: 0.01
lrf: 0.01
momentum: 0.937
weight_decay: 0.0005
warmup_epochs: 3
warmup_momentum: 0.8
warmup_bias_lr: 0.1
box: 7.5 # Peso da perda de box
cls: 0.5 # Peso da perda de classe
dfl: 1.5 # Peso da focal loss
close_mosaic: 10

# Augmentações
hsv_h: 0.015 # Mudança de matiz
hsv_s: 0.7 # Mudança de saturação
hsv_v: 0.4 # Mudança de valor (brilho)
degrees: 0.0 # Rotação (+/- graus)
translate: 0.1 # Translação (+/- fração)
scale: 0.5 # Escala (+/- ganho)
fliplr: 0.5 # Probabilidade de flip horizontal
flipud: 0.0 # Probabilidade de flip vertical
mosaic: 1.0 # Probabilidade de usar mosaico
mixup: 0.0 # Probabilidade de usar mixup

# Logging & Salvamento
project: meu_projeto
name: experimento1
exist_ok: False # Sobrescrever pasta de saída
save: True
save_period: 10
```

E depois usá-lo durante o treinamento:

```python
# Treinar usando arquivo de configuração
from ultralytics import YOLO

model = YOLO('yolov8n.pt')
results = model.train(cfg='config.yaml')
```

### Monitoramento de Treinamento com Callbacks

```python
# Usando callbacks para monitorar o treinamento
from ultralytics import YOLO
from ultralytics.callbacks.base import Callback
import time
import matplotlib.pyplot as plt
import numpy as np

# Criar uma classe de callback personalizada
class CustomCallback(Callback):
    def __init__(self):
        self.start_time = None
        self.epoch_times = []
        self.train_losses = []
        self.val_metrics = []

    def on_train_start(self, trainer):
        self.start_time = time.time()
        print(f"Treinamento iniciado com {trainer.epochs} épocas")

    def on_train_epoch_start(self, trainer):
        self.epoch_start_time = time.time()
        print(f"Época {trainer.epoch+1}/{trainer.epochs} iniciada")

    def on_train_epoch_end(self, trainer):
        epoch_time = time.time() - self.epoch_start_time
        self.epoch_times.append(epoch_time)

        # Capturar métricas de treinamento
        if hasattr(trainer, 'tloss'):
            self.train_losses.append(float(trainer.tloss))

        print(f"Época {trainer.epoch+1} concluída em {epoch_time:.2f}s")

    def on_val_end(self, validator):
        # Capturar métricas de validação
        if hasattr(validator, 'metrics'):
            metrics = validator.metrics
            self.val_metrics.append({
                'map50': float(metrics.get('metrics/mAP50(B)', 0)),
                'map': float(metrics.get('metrics/mAP50-95(B)', 0))
            })

            print(f"Validação: mAP50={self.val_metrics[-1]['map50']:.4f}, "
                  f"mAP50-95={self.val_metrics[-1]['map']:.4f}")

    def on_train_end(self, trainer):
        total_time = time.time() - self.start_time
        print(f"Treinamento finalizado em {total_time/60:.2f} minutos")

        # Plotar métricas
        self.plot_metrics()

    def plot_metrics(self):
        # Plot de perda de treinamento
        plt.figure(figsize=(12, 8))

        plt.subplot(2, 2, 1)
        plt.plot(self.train_losses)
        plt.title('Perda de Treinamento')
        plt.xlabel('Época')
        plt.ylabel('Perda')

        # Plot de métricas de validação
        plt.subplot(2, 2, 2)
        val_epochs = list(range(1, len(self.val_metrics) + 1))
        plt.plot(val_epochs, [m['map50'] for m in self.val_metrics], label='mAP50')
        plt.plot(val_epochs, [m['map'] for m in self.val_metrics], label='mAP50-95')
        plt.title('Métricas de Validação')
        plt.xlabel('Época')
        plt.ylabel('mAP')
        plt.legend()

        # Plot de tempo por época
        plt.subplot(2, 2, 3)
        plt.plot(self.epoch_times)
        plt.title('Tempo por Época')
        plt.xlabel('Época')
        plt.ylabel('Tempo (s)')

        # Plot de tempo cumulativo
        plt.subplot(2, 2, 4)
        cumulative_time = np.cumsum(self.epoch_times)
        plt.plot(cumulative_time / 60)  # Converter para minutos
        plt.title('Tempo Cumulativo')
        plt.xlabel('Época')
        plt.ylabel('Tempo (min)')

        plt.tight_layout()
        plt.savefig('metricas_treinamento.png')
        plt.close()

# Usar o callback durante o treinamento
if __name__ == "__main__":
    model = YOLO('yolov8n.pt')
    custom_callback = CustomCallback()
    model.add_callback("on_train_start", custom_callback.on_train_start)
    model.add_callback("on_train_epoch_start", custom_callback.on_train_epoch_start)
    model.add_callback("on_train_epoch_end", custom_callback.on_train_epoch_end)
    model.add_callback("on_val_end", custom_callback.on_val_end)
    model.add_callback("on_train_end", custom_callback.on_train_end)

    results = model.train(
        data='caminho/para/data.yaml',
        epochs=50,
        imgsz=640,
        batch=16,
        name='callback_demo'
    )
```

## Inferência

Após treinar seu modelo, você pode usá-lo para realizar inferência em novos dados de diferentes maneiras.

### Inferência Básica

```python
# Inferência básica com modelo treinado
from ultralytics import YOLO

# Carregar modelo treinado
model = YOLO('caminho/para/modelo_treinado.pt')

# Inferência em uma imagem
results = model('caminho/para/nova_imagem.jpg')

# Obter previsões
for result in results:
    boxes = result.boxes  # Caixas delimitadoras
    probs = result.probs  # Probabilidades (classificação)
    masks = result.masks  # Máscaras (segmentação)
    keypoints = result.keypoints  # Keypoints (pose)

    # Processar caixas delimitadoras (para detecção)
    if boxes is not None:
        for box in boxes:
            # Coordenadas (x1, y1, x2, y2)
            coords = box.xyxy[0].cpu().numpy()

            # Classe e confiança
            cls_id = int(box.cls[0].item())
            confidence = float(box.conf[0].item())

            print(f"Objeto detectado: classe {cls_id}, confiança {confidence:.2f}, coordenadas {coords}")
```

### Inferência em Lote

```python
# Inferência em lote para maior eficiência
from ultralytics import YOLO
import glob
from tqdm import tqdm
import numpy as np
import cv2
import os

# Carregar modelo
model = YOLO('caminho/para/modelo_treinado.pt')

# Obter lista de imagens
image_paths = glob.glob('pasta_de_imagens/*.jpg')
batch_size = 16  # Ajustar conforme memória disponível

# Criar pasta de saída
os.makedirs('resultados', exist_ok=True)

# Processar imagens em lotes
for i in tqdm(range(0, len(image_paths), batch_size)):
    # Selecionar lote atual
    batch_paths = image_paths[i:i+batch_size]

    # Realizar inferência em lote
    results = model(batch_paths, conf=0.25)

    # Processar cada resultado
    for img_path, result in zip(batch_paths, results):
        # Nome do arquivo
        filename = os.path.basename(img_path)

        # Ler imagem original
        img = cv2.imread(img_path)

        # Desenhar resultados
        annotated_img = result.plot()

        # Salvar resultado
        save_path = os.path.join('resultados', filename)
        cv2.imwrite(save_path, annotated_img)
```

### Inferência com GPU vs CPU

```python
# Comparação de desempenho entre GPU e CPU
from ultralytics import YOLO
import time
import torch
import matplotlib.pyplot as plt
import numpy as np

# Carregar modelo
model = YOLO('yolov8n.pt')  # Modelo leve para teste

# Preparar imagem de teste
image_path = 'caminho/para/imagem_teste.jpg'

# Testar performance em diferentes dispositivos
devices = ['cpu']
if torch.cuda.is_available():
    devices.append('cuda:0')

# Número de iterações para teste
num_iters = 20

# Armazenar resultados
results = {}

for device in devices:
    print(f"Testando inferência em {device}...")
    model.to(device)  # Mover modelo para dispositivo

    # Warm-up
    for _ in range(5):
        _ = model(image_path)

    # Teste de tempo
    times = []
    for _ in range(num_iters):
        start_time = time.time()
        _ = model(image_path)
        end_time = time.time()
        times.append(end_time - start_time)

    # Calcular estatísticas
    avg_time = np.mean(times)
    std_time = np.std(times)
    fps = 1.0 / avg_time

    results[device] = {
        'avg_time': avg_time,
        'std_time': std_time,
        'fps': fps
    }

    print(f"  Tempo médio: {avg_time:.4f}s (±{std_time:.4f}s)")
    print(f"  FPS médio: {fps:.2f}")

# Visualizar resultados
plt.figure(figsize=(10, 5))

# Gráfico de FPS
plt.subplot(1, 2, 1)
plt.bar(results.keys(), [res['fps'] for res in results.values()])
plt.title('FPS por Dispositivo')
plt.ylabel('Frames Por Segundo')

# Gráfico de tempo de inferência
plt.subplot(1, 2, 2)
plt.bar(results.keys(), [res['avg_time'] for res in results.values()],
        yerr=[res['std_time'] for res in results.values()])
plt.title('Tempo de Inferência')
plt.ylabel('Tempo (s)')

plt.tight_layout()
plt.savefig('comparacao_desempenho.png')
plt.show()
```

### Inferência em Streaming de Vídeo

```python
# Inferência em streaming de vídeo
from ultralytics import YOLO
import cv2
import time
import numpy as np
from collections import deque

# Carregar modelo
model = YOLO('yolov8n.pt')  # Modelo leve para streaming em tempo real

# Abrir streaming
source = 0  # 0 para webcam ou URL de streaming (RTSP, HTTP)
cap = cv2.VideoCapture(source)

# Definir resolução (opcional)
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 1280)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 720)

# Histórico para calcular FPS suavizado
fps_history = deque(maxlen=30)

# Configurações
conf_threshold = 0.3  # Limiar de confiança
class_filter = None  # Filtrar classes específicas, ex: [0, 2, 5]

# Loop principal
while True:
    # Ler frame
    ret, frame = cap.read()
    if not ret:
        print("Erro ao ler frame")
        break

    # Registrar tempo para cálculo de FPS
    start_time = time.time()

    # Realizar inferência
    results = model(frame, conf=conf_threshold, classes=class_filter)

    # Desenhar resultados
    annotated_frame = results[0].plot()

    # Calcular FPS
    process_time = time.time() - start_time
    fps_history.append(1.0 / process_time)
    avg_fps = sum(fps_history) / len(fps_history)

    # Mostrar FPS
    cv2.putText(annotated_frame, f"FPS: {avg_fps:.1f}", (20, 30),
                cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)

    # Mostrar frame processado
    cv2.imshow("YOLOv8 Streaming", annotated_frame)

    # Verificar eventos de teclado
    key = cv2.waitKey(1)

    # Sair com ESC ou 'q'
    if key == 27 or key == ord('q'):
        break
    # Aumentar limiar de confiança com '+'
    elif key == ord('+') or key == ord('='):
        conf_threshold = min(conf_threshold + 0.05, 1.0)
        print(f"Limiar de confiança: {conf_threshold:.2f}")
    # Diminuir limiar de confiança com '-'
    elif key == ord('-'):
        conf_threshold = max(conf_threshold - 0.05, 0.05)
        print(f"Limiar de confiança: {conf_threshold:.2f}")

# Liberar recursos
cap.release()
cv2.destroyAllWindows()
```

## Exportação de Modelos

YOLOv8 oferece diferentes formatos de exportação para compatibilidade com várias plataformas e ambientes.

```python
# Exportação de modelo para diferentes formatos
from ultralytics import YOLO

# Carregar modelo
model = YOLO('caminho/para/modelo_treinado.pt')

# Exportar para ONNX
model.export(format='onnx', opset=12)

# Exportar para TensorRT (requer GPU NVIDIA)
# model.export(format='engine', half=True)

# Exportar para OpenVINO (otimizado para Intel)
# model.export(format='openvino')

# Exportar para TFLite (mobile)
# model.export(format='tflite')

# Exportar para CoreML (iOS)
# model.export(format='coreml')
```

### Exemplo de Uso com Modelo Exportado (ONNX)

```python
# Uso de modelo YOLOv8 exportado para ONNX
import cv2
import numpy as np
import onnxruntime as ort
import time

# Carregar modelo ONNX
onnx_model_path = 'caminho/para/modelo.onnx'
providers = ['CUDAExecutionProvider', 'CPUExecutionProvider']  # Usar CUDA se disponível
session = ort.InferenceSession(onnx_model_path, providers=providers)

# Obter informações do modelo
input_name = session.get_inputs()[0].name
output_names = [output.name for output in session.get_outputs()]
input_shape = session.get_inputs()[0].shape
input_width, input_height = input_shape[2], input_shape[3]

# Lista de classes (ajustar conforme seu modelo)
classes = ['pessoa', 'bicicleta', 'carro', ...]  # Classes do COCO ou sua lista personalizada

# Função para pré-processamento
def preprocess_image(img):
    # Redimensionar imagem
    resized = cv2.resize(img, (input_width, input_height))

    # Normalizar (0-255 -> 0-1)
    normalized = resized / 255.0

    # Converter para formato NCHW (batch, channel, height, width)
    transposed = normalized.transpose(2, 0, 1)  # HWC -> CHW

    # Adicionar dimensão de batch
    batched = np.expand_dims(transposed, axis=0)

    # Garantir tipo float32
    return batched.astype(np.float32)

# Função para pós-processamento (parsing de saídas)
def process_output(outputs, img_width, img_height, conf_threshold=0.25):
    # Extrair saídas (varia conforme a versão do YOLOv8)
    # Para YOLOv8 exportado sem parâmetro --prediction:
    predictions = outputs[0]  # output[0] contém todas as previsões

    # Para modelos exportados com --prediction, ajustar conforme necessário

    # Processar predições
    boxes = []
    scores = []
    class_ids = []

    # Para cada detecção no batch (geralmente apenas 1)
    for i, prediction in enumerate(predictions):
        # As previsões geralmente têm formato [batch, num_detections, num_classes+5]
        # Os 5 primeiros valores são [x, y, w, h, confidence]
        for detection in prediction:
            # Extrair confiança da detecção
            confidence = float(detection[4])

            if confidence < conf_threshold:
                continue

            # Encontrar índice da classe com maior probabilidade
            class_scores = detection[5:]
            class_id = np.argmax(class_scores)
            class_score = float(class_scores[class_id])

            # Combinar confiança da detecção com score da classe
            score = confidence * class_score

            if score < conf_threshold:
                continue

            # Extrair coordenadas da caixa
            x, y, w, h = detection[0:4]

            # Converter para coordenadas da imagem original
            x1 = int((x - w/2) * img_width)
            y1 = int((y - h/2) * img_height)
            x2 = int((x + w/2) * img_width)
            y2 = int((y + h/2) * img_height)

            boxes.append([x1, y1, x2, y2])
            scores.append(score)
            class_ids.append(class_id)

    return boxes, scores, class_ids

# Função principal para inferência
def detect_objects(image_path):
    # Carregar imagem
    img = cv2.imread(image_path)
    orig_height, orig_width = img.shape[:2]

    # Pré-processar imagem
    input_tensor = preprocess_image(img)

    # Medir tempo de inferência
    start_time = time.time()

    # Executar inferência
    outputs = session.run(output_names, {input_name: input_tensor})

    # Medir tempo total
    inference_time = time.time() - start_time
    print(f"Tempo de inferência: {inference_time*1000:.2f} ms")

    # Processar saídas
    boxes, scores, class_ids = process_output(outputs, orig_width, orig_height, conf_threshold=0.25)

    # Desenhar resultados na imagem
    result_img = img.copy()

    for box, score, class_id in zip(boxes, scores, class_ids):
        x1, y1, x2, y2 = box
        label = f"{classes[class_id]}: {score:.2f}"

        # Gerar cor diferente para cada classe
        color = (int(hash(classes[class_id]) % 256),
                 int(hash(classes[class_id] + 'a') % 256),
                 int(hash(classes[class_id] + 'b') % 256))

        # Desenhar retângulo e texto
        cv2.rectangle(result_img, (x1, y1), (x2, y2), color, 2)
        cv2.putText(result_img, label, (x1, y1 - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.5, color,
```
