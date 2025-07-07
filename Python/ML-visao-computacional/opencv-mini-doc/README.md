# [⬅ Voltar para o índice principal](../../../README.md)

# OpenCV: Visão Computacional para Projetos Profissionais

## Introdução ao OpenCV

OpenCV (Open Source Computer Vision Library) é uma biblioteca de visão computacional de código aberto que contém centenas de algoritmos otimizados para processamento de imagens e vídeos. Criada inicialmente pela Intel em 1999, hoje é mantida por uma comunidade global e conta com interfaces para várias linguagens de programação, sendo Python e C++ as mais populares.

## Instalação e Configuração

### Instalação no Python

```python
# Usando pip
pip install opencv-python

# Para recursos extras, incluindo módulos não livres
pip install opencv-python-contrib
```

### Verificação da Instalação

```python
import cv2
print(cv2.__version__)
```

## Funcionalidades Básicas

### Leitura e Exibição de Imagens

```python
import cv2
import numpy as np

# Carregar uma imagem
imagem = cv2.imread('foto.jpg')

# Converter para escala de cinza
imagem_cinza = cv2.cvtColor(imagem, cv2.COLOR_BGR2GRAY)

# Exibir imagem
cv2.imshow('Imagem Original', imagem)
cv2.imshow('Imagem em Cinza', imagem_cinza)
cv2.waitKey(0)
cv2.destroyAllWindows()

# Salvar imagem
cv2.imwrite('foto_cinza.jpg', imagem_cinza)
```

### Captura e Processamento de Vídeo

```python
import cv2

# Iniciar captura de vídeo da webcam (câmera 0)
cap = cv2.VideoCapture(0)

while True:
    # Capturar frame por frame
    ret, frame = cap.read()

    # Converter para escala de cinza
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Exibir resultados
    cv2.imshow('Original', frame)
    cv2.imshow('Cinza', gray)

    # Sair com 'q'
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# Liberar a captura e fechar janelas
cap.release()
cv2.destroyAllWindows()
```

## Técnicas Intermediárias

### Detecção de Bordas

```python
import cv2
import numpy as np

imagem = cv2.imread('objeto.jpg')
imagem_cinza = cv2.cvtColor(imagem, cv2.COLOR_BGR2GRAY)

# Detector de bordas Canny
bordas = cv2.Canny(imagem_cinza, 100, 200)

cv2.imshow('Original', imagem)
cv2.imshow('Bordas', bordas)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Detecção de Faces usando Haar Cascades

```python
import cv2

# Carregar o classificador pré-treinado
detector_face = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')

# Iniciar captura de vídeo
cap = cv2.VideoCapture(0)

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # Converter para escala de cinza
    cinza = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Detectar faces
    faces = detector_face.detectMultiScale(
        cinza,
        scaleFactor=1.1,
        minNeighbors=5,
        minSize=(30, 30)
    )

    # Desenhar retângulos nas faces
    for (x, y, w, h) in faces:
        cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)

    # Mostrar resultado
    cv2.imshow('Detecção de Faces', frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

### Rastreamento de Objetos por Cor

```python
import cv2
import numpy as np

cap = cv2.VideoCapture(0)

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # Converter BGR para HSV
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    # Definir faixa de cor azul em HSV
    azul_baixo = np.array([100, 50, 50])
    azul_alto = np.array([130, 255, 255])

    # Criar máscara
    mascara = cv2.inRange(hsv, azul_baixo, azul_alto)

    # Aplicar operações morfológicas para remover ruído
    kernel = np.ones((5, 5), np.uint8)
    mascara = cv2.erode(mascara, kernel, iterations=2)
    mascara = cv2.dilate(mascara, kernel, iterations=2)

    # Encontrar contornos
    contornos, _ = cv2.findContours(mascara, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

    # Desenhar contornos
    for contorno in contornos:
        area = cv2.contourArea(contorno)
        if area > 1000:  # Filtrar por tamanho
            x, y, w, h = cv2.boundingRect(contorno)
            cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
            cv2.putText(frame, f'Area: {area}', (x, y-10),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)

    # Mostrar resultado
    cv2.imshow('Rastreamento por Cor', frame)
    cv2.imshow('Máscara', mascara)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

## Técnicas Avançadas

### Reconhecimento de Objetos com YOLO

```python
import cv2
import numpy as np

# Carregar YOLO
net = cv2.dnn.readNet("yolov3.weights", "yolov3.cfg")
with open("coco.names", "r") as f:
    classes = [line.strip() for line in f.readlines()]
layer_names = net.getLayerNames()
output_layers = [layer_names[i - 1] for i in net.getUnconnectedOutLayers()]

# Carregar imagem
img = cv2.imread("objetos.jpg")
height, width, channels = img.shape

# Detectar objetos
blob = cv2.dnn.blobFromImage(img, 0.00392, (416, 416), (0, 0, 0), True, crop=False)
net.setInput(blob)
outs = net.forward(output_layers)

# Mostrar informações na tela
class_ids = []
confidences = []
boxes = []
for out in outs:
    for detection in out:
        scores = detection[5:]
        class_id = np.argmax(scores)
        confidence = scores[class_id]
        if confidence > 0.5:
            # Coordenadas do objeto
            center_x = int(detection[0] * width)
            center_y = int(detection[1] * height)
            w = int(detection[2] * width)
            h = int(detection[3] * height)

            # Coordenadas do retângulo
            x = int(center_x - w / 2)
            y = int(center_y - h / 2)

            boxes.append([x, y, w, h])
            confidences.append(float(confidence))
            class_ids.append(class_id)

indexes = cv2.dnn.NMSBoxes(boxes, confidences, 0.5, 0.4)

for i in range(len(boxes)):
    if i in indexes:
        x, y, w, h = boxes[i]
        label = str(classes[class_ids[i]])
        confidence = confidences[i]
        cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)
        cv2.putText(img, f"{label} {confidence:.2f}", (x, y - 10),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)

cv2.imshow("YOLO Detection", img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### OCR (Reconhecimento Óptico de Caracteres) com Tesseract e OpenCV

```python
import cv2
import pytesseract

# Configurar caminho do Tesseract (para Windows)
# pytesseract.pytesseract.tesseract_cmd = r'C:\Program Files\Tesseract-OCR\tesseract.exe'

# Carregar imagem
imagem = cv2.imread('texto.jpg')
cinza = cv2.cvtColor(imagem, cv2.COLOR_BGR2GRAY)

# Pré-processamento
_, binaria = cv2.threshold(cinza, 150, 255, cv2.THRESH_BINARY_INV)
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (3, 3))
processada = cv2.morphologyEx(binaria, cv2.MORPH_OPEN, kernel)

# Reconhecimento de texto
texto = pytesseract.image_to_string(processada, lang='por')
print("Texto reconhecido:")
print(texto)

# Exibir imagem com caixas de texto
caixas = pytesseract.image_to_data(processada, output_type=pytesseract.Output.DICT)

for i, palavra in enumerate(caixas['text']):
    if palavra.strip() != '':
        x = caixas['left'][i]
        y = caixas['top'][i]
        w = caixas['width'][i]
        h = caixas['height'][i]
        cv2.rectangle(imagem, (x, y), (x + w, y + h), (0, 255, 0), 2)

cv2.imshow('OCR Resultado', imagem)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

## Ideias de Projetos Profissionais com OpenCV

### 1. Sistema de Vigilância Inteligente

Desenvolva um sistema de monitoramento que detecta movimentos, pessoas e veículos, gerando alertas automaticamente.

```python
import cv2
import numpy as np
import time
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.image import MIMEImage

# Configuração da detecção de movimento
cap = cv2.VideoCapture(0)
ret, frame1 = cap.read()
ret, frame2 = cap.read()

# Função para enviar email com alerta
def enviar_alerta(imagem):
    # Configurações de email
    remetente = "seu_email@gmail.com"
    senha = "sua_senha"
    destinatario = "cliente@email.com"

    msg = MIMEMultipart()
    msg['From'] = remetente
    msg['To'] = destinatario
    msg['Subject'] = "ALERTA DE SEGURANÇA!"

    texto = MIMEText("Foi detectado movimento na câmera de segurança!")
    msg.attach(texto)

    # Converter imagem OpenCV para formato de email
    _, buffer = cv2.imencode('.jpg', imagem)
    img_attachment = MIMEImage(buffer.tobytes())
    msg.attach(img_attachment)

    # Enviar email
    try:
        servidor = smtplib.SMTP('smtp.gmail.com', 587)
        servidor.starttls()
        servidor.login(remetente, senha)
        servidor.send_message(msg)
        servidor.quit()
        print("Alerta enviado!")
    except Exception as e:
        print(f"Erro ao enviar alerta: {e}")

# Loop principal
ultimo_alerta = time.time() - 60  # Inicializar para permitir o primeiro alerta
while True:
    diff = cv2.absdiff(frame1, frame2)
    cinza = cv2.cvtColor(diff, cv2.COLOR_BGR2GRAY)
    blur = cv2.GaussianBlur(cinza, (5, 5), 0)
    _, thresh = cv2.threshold(blur, 20, 255, cv2.THRESH_BINARY)
    dilated = cv2.dilate(thresh, None, iterations=3)
    contornos, _ = cv2.findContours(dilated, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

    movimento_detectado = False
    for contorno in contornos:
        (x, y, w, h) = cv2.boundingRect(contorno)
        if cv2.contourArea(contorno) < 5000:  # Filtra pequenos movimentos
            continue
        cv2.rectangle(frame1, (x, y), (x+w, y+h), (0, 255, 0), 2)
        cv2.putText(frame1, "Status: Movimento", (10, 20), cv2.FONT_HERSHEY_SIMPLEX,
                    1, (0, 0, 255), 3)
        movimento_detectado = True

    # Enviar alerta se movimento detectado (no máximo a cada 60 segundos)
    agora = time.time()
    if movimento_detectado and (agora - ultimo_alerta) > 60:
        enviar_alerta(frame1)
        ultimo_alerta = agora

    cv2.imshow("Segurança", frame1)

    frame1 = frame2
    ret, frame2 = cap.read()

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

### 2. Contador de Pessoas para Estabelecimentos Comerciais

```python
import cv2
import numpy as np
from datetime import datetime

# Inicializar captura
cap = cv2.VideoCapture(0)  # Ou caminho para o vídeo

# Configurar detector de pessoas HOG
hog = cv2.HOGDescriptor()
hog.setSVMDetector(cv2.HOGDescriptor_getDefaultPeopleDetector())

# Contadores
pessoas_dentro = 0
pessoas_fora = 0
contagem_total = 0

# Linha de entrada/saída
linha_y = 240  # Ajuste para a posição desejada

# Log de contagem
def registrar_contagem(entrada=True):
    global pessoas_dentro, pessoas_fora, contagem_total

    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    if entrada:
        pessoas_dentro += 1
        contagem_total += 1
        print(f"{timestamp} - Entrada: {pessoas_dentro} pessoas dentro, {contagem_total} total")
    else:
        pessoas_fora += 1
        pessoas_dentro -= 1
        print(f"{timestamp} - Saída: {pessoas_dentro} pessoas dentro, {pessoas_fora} saídas")

    # Salvar em arquivo
    with open("contagem_pessoas.csv", "a") as f:
        f.write(f"{timestamp},{1 if entrada else -1},{pessoas_dentro},{contagem_total}\n")

# Tracking de pessoas
rastreadores = []
ids_pessoas = {}
proximo_id = 1

while True:
    ret, frame = cap.read()
    if not ret:
        break

    altura, largura = frame.shape[:2]

    # Desenhar linha de contagem
    cv2.line(frame, (0, linha_y), (largura, linha_y), (0, 0, 255), 2)

    # Detectar pessoas a cada 15 frames
    if len(rastreadores) == 0 or cv2.getTickCount() % 15 == 0:
        # Redimensionar para melhorar desempenho
        frame_redim = cv2.resize(frame, (largura // 2, altura // 2))

        # Detectar pessoas
        pessoas, _ = hog.detectMultiScale(
            frame_redim,
            winStride=(8, 8),
            padding=(8, 8),
            scale=1.05
        )

        # Ajustar coordenadas para o tamanho original
        pessoas = pessoas * 2

        # Criar novos rastreadores
        for (x, y, w, h) in pessoas:
            rastreador = cv2.TrackerKCF_create()
            rastreador.init(frame, (x, y, w, h))
            rastreadores.append(rastreador)
            ids_pessoas[(x, y, w, h)] = proximo_id
            proximo_id += 1

    # Atualizar rastreadores
    rastreadores_ativos = []
    for rastreador in rastreadores:
        success, box = rastreador.update(frame)
        if success:
            (x, y, w, h) = [int(v) for v in box]
            cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)

            # Verificar cruzamento da linha
            centro_y = y + h // 2
            if abs(centro_y - linha_y) < 5:  # Na linha
                if y < linha_y:  # Movendo para baixo
                    registrar_contagem(entrada=False)
                else:  # Movendo para cima
                    registrar_contagem(entrada=True)

            rastreadores_ativos.append(rastreador)

    rastreadores = rastreadores_ativos

    # Mostrar contagem
    cv2.putText(frame, f"Dentro: {pessoas_dentro}", (10, 30),
                cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0, 255, 0), 2)
    cv2.putText(frame, f"Total hoje: {contagem_total}", (10, 60),
                cv2.FONT_HERSHEY_SIMPLEX, 0.8, (0, 255, 0), 2)

    # Exibir resultado
    cv2.imshow("Contador de Pessoas", frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

### 3. Sistema de Controle de Qualidade Industrial

```python
import cv2
import numpy as np
import time
import csv
from datetime import datetime

# Inicializar câmera
cap = cv2.VideoCapture(0)

# Configurações para detecção de defeitos
def verificar_produto(imagem):
    # Converter para escala de cinza
    cinza = cv2.cvtColor(imagem, cv2.COLOR_BGR2GRAY)

    # Aplicar threshold para binarização
    _, binaria = cv2.threshold(cinza, 200, 255, cv2.THRESH_BINARY)

    # Encontrar contornos
    contornos, _ = cv2.findContours(binaria, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

    # Analisar formato e tamanho
    defeitos = []
    for contorno in contornos:
        area = cv2.contourArea(contorno)
        perimetro = cv2.arcLength(contorno, True)

        # Verificar se o formato está correto
        if area > 1000:  # Tamanho mínimo
            # Calcular circularidade
            circularidade = 4 * np.pi * area / (perimetro * perimetro)

            if circularidade < 0.8:  # Não é circular o suficiente
                x, y, w, h = cv2.boundingRect(contorno)
                defeitos.append((x, y, w, h, "Formato irregular"))

            # Verificar textura
            roi = cinza[y:y+h, x:x+w]
            if roi.size > 0:
                desvio = np.std(roi)
                if desvio > 30:  # Textura irregular
                    defeitos.append((x, y, w, h, "Textura irregular"))

    return defeitos

# Configurar log
log_file = "controle_qualidade.csv"
with open(log_file, "w", newline='') as f:
    writer = csv.writer(f)
    writer.writerow(["Timestamp", "Produto", "Status", "Defeitos"])

# Contador de produtos
contador = 0
produtos_ok = 0
produtos_defeituosos = 0

# Estado do sistema
analisando = False
ultima_analise = time.time()

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # Fazer cópia para exibição
    exibicao = frame.copy()

    # Interface
    cv2.putText(exibicao, f"Produtos analisados: {contador}", (10, 30),
                cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 255, 255), 2)
    cv2.putText(exibicao, f"Aprovados: {produtos_ok}", (10, 60),
                cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 255, 0), 2)
    cv2.putText(exibicao, f"Defeituosos: {produtos_defeituosos}", (10, 90),
                cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 0, 255), 2)

    # Mostrar área de análise
    altura, largura = frame.shape[:2]
    cv2.rectangle(exibicao, (100, 100), (largura-100, altura-100), (0, 255, 0), 2)

    # Analisar produto (a cada 3 segundos ou quando solicitado)
    agora = time.time()
    if cv2.waitKey(1) & 0xFF == ord('a') or (analisando and agora - ultima_analise > 3):
        # Região de interesse
        roi = frame[100:altura-100, 100:largura-100]

        # Analisar produto
        defeitos = verificar_produto(roi)
        contador += 1

        timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

        if len(defeitos) == 0:
            status = "APROVADO"
            produtos_ok += 1
            cv2.putText(exibicao, "PRODUTO APROVADO", (largura//3, altura//2),
                        cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 3)
        else:
            status = "REJEITADO"
            produtos_defeituosos += 1
            cv2.putText(exibicao, "PRODUTO REJEITADO", (largura//3, altura//2),
                        cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 255), 3)

            # Marcar defeitos
            for (x, y, w, h, tipo) in defeitos:
                cv2.rectangle(exibicao, (x+100, y+100), (x+w+100, y+h+100), (0, 0, 255), 2)
                cv2.putText(exibicao, tipo, (x+100, y+90),
                            cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 255), 1)

        # Registrar log
        with open(log_file, "a", newline='') as f:
            writer = csv.writer(f)
            writer.writerow([timestamp, f"Produto{contador}", status, len(defeitos)])

        ultima_analise = agora

    # Alternar modo de análise contínua
    if cv2.waitKey(1) & 0xFF == ord('c'):
        analisando = not analisando
        if analisando:
            cv2.putText(exibicao, "MODO AUTOMÁTICO ATIVADO", (largura//3, 30),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 255, 255), 2)

    # Exibir resultado
    cv2.imshow("Controle de Qualidade", exibicao)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

### 4. Sistema de Reconhecimento Facial para Controle de Acesso (Continuação)

```python
# Continuação do código anterior

# Loop principal
while True:
    opcao = exibir_menu()

    if opcao == "1":
        nome = input("Digite o nome da pessoa: ")
        cadastrar_pessoa(nome)

    elif opcao == "2":
        if len(nomes_pessoas) == 0:
            print("Cadastre pelo menos uma pessoa antes de treinar o modelo!")
        else:
            treinar_modelo()
            modelo_carregado = True

    elif opcao == "3":
        if not modelo_carregado:
            print("Treine o modelo antes de iniciar o controle de acesso!")
            continue

        print("Iniciando sistema de controle de acesso...")
        print("Pressione 'q' para sair")

        cap = cv2.VideoCapture(0)
        ultimo_reconhecimento = ""
        tempo_ultimo = datetime.datetime.now()

        while True:
            ret, frame = cap.read()
            if not ret:
                break

            cinza = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
            faces = detector_face.detectMultiScale(cinza, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))

            for (x, y, w, h) in faces:
                cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)

                # Reconhecer face
                id_previsto, confianca = reconhecedor.predict(cinza[y:y+h, x:x+w])

                # Verificar confiança (menor valor = maior confiança)
                if confianca < 70:  # Threshold de confiança
                    nome = nomes_pessoas.get(id_previsto, "Desconhecido")

                    # Evitar reconhecimentos repetidos em sequência
                    agora = datetime.datetime.now()
                    if (nome != ultimo_reconhecimento or
                        (agora - tempo_ultimo).total_seconds() > 5):
                        registrar_acesso(id_previsto, nome, True)
                        ultimo_reconhecimento = nome
                        tempo_ultimo = agora

                    cor = (0, 255, 0)  # Verde para autorizado
                    cv2.putText(frame, f"{nome} - Autorizado", (x, y-10),
                                cv2.FONT_HERSHEY_SIMPLEX, 0.5, cor, 2)
                else:
                    nome = "Desconhecido"

                    # Evitar reconhecimentos repetidos em sequência
                    agora = datetime.datetime.now()
                    if (nome != ultimo_reconhecimento or
                        (agora - tempo_ultimo).total_seconds() > 5):
                        registrar_acesso(0, nome, False)
                        ultimo_reconhecimento = nome
                        tempo_ultimo = agora

                    cor = (0, 0, 255)  # Vermelho para não autorizado
                    cv2.putText(frame, "Não Autorizado", (x, y-10),
                                cv2.FONT_HERSHEY_SIMPLEX, 0.5, cor, 2)

            # Mostrar informações
            cv2.putText(frame, "Pressione 'q' para sair", (10, 30),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 255, 255), 2)
            cv2.putText(frame, f"Pessoas cadastradas: {len(nomes_pessoas)}", (10, 60),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 255, 255), 2)

            cv2.imshow("Controle de Acesso", frame)

            if cv2.waitKey(1) & 0xFF == ord('q'):
                break

        cap.release()
        cv2.destroyAllWindows()

    elif opcao == "4":
        print("Encerrando sistema...")
        break

    else:
        print("Opção inválida!")
```

### 5. Análise de Tráfego e Detecção de Infrações de Trânsito

```python
import cv2
import numpy as np
import time
import datetime
import csv
import os

# Configurações
if not os.path.exists("trafego_logs"):
    os.makedirs("trafego_logs")

# Inicializar detecção de veículos
car_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_car.xml')

# Alternativa usando YOLOv3
def detectar_veiculos_yolo(frame, net, output_layers, classes):
    altura, largura = frame.shape[:2]

    # Preparar imagem para YOLO
    blob = cv2.dnn.blobFromImage(frame, 1/255.0, (416, 416), swapRB=True, crop=False)
    net.setInput(blob)
    saidas = net.forward(output_layers)

    caixas = []
    confiancas = []
    ids_classes = []

    # Processar detecções
    for saida in saidas:
        for deteccao in saida:
            scores = deteccao[5:]
            id_classe = np.argmax(scores)
            confianca = scores[id_classe]

            if confianca > 0.5 and classes[id_classe] in ["car", "truck", "bus", "motorbike"]:
                # Coordenadas da caixa
                centro_x = int(deteccao[0] * largura)
                centro_y = int(deteccao[1] * altura)
                w = int(deteccao[2] * largura)
                h = int(deteccao[3] * altura)

                # Coordenadas do retângulo
                x = int(centro_x - w/2)
                y = int(centro_y - h/2)

                caixas.append([x, y, w, h])
                confiancas.append(float(confianca))
                ids_classes.append(id_classe)

    # Aplicar supressão não-máxima
    indices = cv2.dnn.NMSBoxes(caixas, confiancas, 0.5, 0.4)

    veiculos = []
    for i in indices:
        i = i[0] if isinstance(i, list) else i  # Compatibilidade com diferentes versões
        caixa = caixas[i]
        veiculos.append({
            "caixa": caixa,
            "tipo": classes[ids_classes[i]],
            "confianca": confiancas[i]
        })

    return veiculos

# Inicializar trânsito
class AnalisadorTrafego:
    def __init__(self):
        self.veiculos_rastreados = {}
        self.proximo_id = 1
        self.contagem_veiculos = 0
        self.infrações = []
        self.velocidades = []
        self.ultimo_log = time.time()

        # Criar arquivo de log
        data_atual = datetime.datetime.now().strftime("%Y-%m-%d")
        self.log_file = f"trafego_logs/trafego_{data_atual}.csv"

        # Verificar se o arquivo já existe
        if not os.path.exists(self.log_file):
            with open(self.log_file, "w", newline="") as f:
                writer = csv.writer(f)
                writer.writerow(["timestamp", "veiculos_hora", "velocidade_media", "infracoes"])

    def rastrear_veiculos(self, frame, veiculos_detectados):
        altura, largura = frame.shape[:2]
        timestamp = time.time()

        # Atualizar rastreadores existentes
        for veiculo_id in list(self.veiculos_rastreados.keys()):
            rastreador = self.veiculos_rastreados[veiculo_id]

            # Remover rastreadores antigos
            if timestamp - rastreador["ultimo_visto"] > 2.0:
                # Calcular a velocidade estimada se possível
                if len(rastreador["posicoes"]) >= 2:
                    tempo_total = rastreador["posicoes"][-1][2] - rastreador["posicoes"][0][2]
                    if tempo_total > 0:
                        # Distância em pixels
                        dx = rastreador["posicoes"][-1][0] - rastreador["posicoes"][0][0]
                        dy = rastreador["posicoes"][-1][1] - rastreador["posicoes"][0][1]
                        distancia_pixels = np.sqrt(dx*dx + dy*dy)

                        # Conversão para km/h (estimativa)
                        # Considere que 100 pixels = 1 metro (ajuste conforme necessário)
                        distancia_metros = distancia_pixels / 100
                        velocidade_ms = distancia_metros / tempo_total
                        velocidade_kmh = velocidade_ms * 3.6

                        self.velocidades.append(velocidade_kmh)

                        # Verificar limite de velocidade (ex: 60 km/h)
                        if velocidade_kmh > 60:
                            self.infrações.append({
                                "tipo": "Excesso de velocidade",
                                "velocidade": velocidade_kmh,
                                "timestamp": datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
                                "posicao": rastreador["posicoes"][-1][:2]
                            })

                del self.veiculos_rastreados[veiculo_id]
                continue

            # Verificar outras infrações (como avanço de sinal)
            pos_y = rastreador["posicoes"][-1][1]
            if 240 <= pos_y <= 250:  # Posição da linha de parada
                # Verificar semáforo (simulado como variável)
                semaforo_vermelho = True  # Simulação
                if semaforo_vermelho:
                    self.infrações.append({
                        "tipo": "Avanço de sinal vermelho",
                        "timestamp": datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
                        "posicao": rastreador["posicoes"][-1][:2]
                    })

        # Associar detecções com rastreadores existentes
        veiculos_associados = set()
        for veiculo in veiculos_detectados:
            x, y, w, h = veiculo["caixa"]
            centro_x = x + w//2
            centro_y = y + h//2

            # Encontrar o rastreador mais próximo
            melhor_id = None
            menor_distancia = float('inf')

            for veiculo_id, rastreador in self.veiculos_rastreados.items():
                if len(rastreador["posicoes"]) > 0:
                    ultimo_x, ultimo_y = rastreador["posicoes"][-1][:2]
                    distancia = np.sqrt((centro_x - ultimo_x)**2 + (centro_y - ultimo_y)**2)

                    if distancia < menor_distancia and distancia < 50:  # Limiar de associação
                        menor_distancia = distancia
                        melhor_id = veiculo_id

            if melhor_id is not None:
                # Atualizar rastreador existente
                self.veiculos_rastreados[melhor_id]["posicoes"].append((centro_x, centro_y, timestamp))
                self.veiculos_rastreados[melhor_id]["ultimo_visto"] = timestamp
                self.veiculos_rastreados[melhor_id]["tipo"] = veiculo["tipo"]
                veiculos_associados.add(melhor_id)
            else:
                # Criar novo rastreador
                self.veiculos_rastreados[self.proximo_id] = {
                    "posicoes": [(centro_x, centro_y, timestamp)],
                    "ultimo_visto": timestamp,
                    "tipo": veiculo["tipo"]
                }
                veiculos_associados.add(self.proximo_id)
                self.proximo_id += 1
                self.contagem_veiculos += 1

        # Registrar log a cada minuto
        agora = time.time()
        if agora - self.ultimo_log >= 60:
            self.registrar_log()
            self.ultimo_log = agora

        return veiculos_associados

    def registrar_log(self):
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")

        # Calcular estatísticas
        veiculos_hora = self.contagem_veiculos * (3600 / (time.time() - self.ultimo_log))
        velocidade_media = np.mean(self.velocidades) if len(self.velocidades) > 0 else 0
        num_infracoes = len(self.infrações)

        # Registrar no arquivo
        with open(self.log_file, "a", newline="") as f:
            writer = csv.writer(f)
            writer.writerow([timestamp, round(veiculos_hora, 2), round(velocidade_media, 2), num_infracoes])

        # Resetar contadores para o próximo intervalo
        self.velocidades = []
        self.infrações = []
        self.contagem_veiculos = 0

# Função principal
def iniciar_analise_trafego():
    # Carregar modelo YOLO (ou usar Haar Cascade)
    usar_yolo = False

    if usar_yolo:
        print("Carregando modelo YOLO...")
        net = cv2.dnn.readNet("yolov3.weights", "yolov3.cfg")
        with open("coco.names", "r") as f:
            classes = [line.strip() for line in f.readlines()]
        layer_names = net.getLayerNames()
        output_layers = [layer_names[i - 1] for i in net.getUnconnectedOutLayers()]

    # Iniciar captura
    cap = cv2.VideoCapture(0)  # Ou caminho para vídeo

    # Inicializar analisador
    analisador = AnalisadorTrafego()

    while True:
        ret, frame = cap.read()
        if not ret:
            break

        # Detectar veículos
        if usar_yolo:
            veiculos = detectar_veiculos_yolo(frame, net, output_layers, classes)
        else:
            gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
            cars = car_cascade.detectMultiScale(gray, 1.1, 3)

            veiculos = []
            for (x, y, w, h) in cars:
                veiculos.append({
                    "caixa": [x, y, w, h],
                    "tipo": "car",
                    "confianca": 1.0
                })

        # Rastrear veículos
        veiculos_ativos = analisador.rastrear_veiculos(frame, veiculos)

        # Desenhar resultados
        for veiculo_id in veiculos_ativos:
            rastreador = analisador.veiculos_rastreados[veiculo_id]
            if len(rastreador["posicoes"]) > 0:
                x, y = rastreador["posicoes"][-1][:2]

                # Desenhar círculo na posição atual
                cv2.circle(frame, (int(x), int(y)), 5, (0, 255, 0), -1)

                # Desenhar ID
                cv2.putText(frame, f"ID: {veiculo_id}", (int(x), int(y) - 10),
                            cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)

                # Desenhar trajeto
                for i in range(1, len(rastreador["posicoes"])):
                    pt1 = (int(rastreador["posicoes"][i-1][0]), int(rastreador["posicoes"][i-1][1]))
                    pt2 = (int(rastreador["posicoes"][i][0]), int(rastreador["posicoes"][i][1]))
                    cv2.line(frame, pt1, pt2, (0, 0, 255), 1)

        # Desenhar linha de parada
        altura, largura = frame.shape[:2]
        cv2.line(frame, (0, 240), (largura, 240), (0, 0, 255), 2)

        # Mostrar estatísticas
        cv2.putText(frame, f"Veículos: {analisador.contagem_veiculos}", (10, 30),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 255, 255), 2)
        cv2.putText(frame, f"Infrações: {len(analisador.infrações)}", (10, 60),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 0, 255), 2)

        # Mostrar resultado
        cv2.imshow("Análise de Tráfego", frame)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    # Finalizar
    cap.release()
    cv2.destroyAllWindows()

if __name__ == "__main__":
    iniciar_analise_trafego()
```

### 6. Sistema de Classificação de Produtos Agrícolas

```python
import cv2
import numpy as np
import os
import csv
import datetime
import time
from sklearn.cluster import KMeans
from collections import Counter

# Diretórios
if not os.path.exists("agricola_logs"):
    os.makedirs("agricola_logs")
if not os.path.exists("agricola_amostras"):
    os.makedirs("agricola_amostras")

# Classes de produtos
produtos = {
    1: {"nome": "Tomate", "cor_min": np.array([0, 120, 70]), "cor_max": np.array([10, 255, 255])},
    2: {"nome": "Laranja", "cor_min": np.array([10, 100, 100]), "cor_max": np.array([25, 255, 255])},
    3: {"nome": "Limão", "cor_min": np.array([25, 100, 100]), "cor_max": np.array([35, 255, 255])},
    4: {"nome": "Maçã Verde", "cor_min": np.array([35, 50, 50]), "cor_max": np.array([90, 255, 255])}
}

class ClassificadorAgricola:
    def __init__(self):
        self.produtos_classificados = {produto_id: 0 for produto_id in produtos.keys()}
        self.produtos_rejeitados = 0
        self.inicio_sessao = time.time()
        self.amostras_salvas = 0

        # Arquivo de log
        data_atual = datetime.datetime.now().strftime("%Y-%m-%d")
        self.log_file = f"agricola_logs/classificacao_{data_atual}.csv"

        if not os.path.exists(self.log_file):
            with open(self.log_file, "w", newline="") as f:
                writer = csv.writer(f)
                writer.writerow(["timestamp", "produto", "qualidade", "tamanho", "cor"])

    def analisar_produto(self, imagem):
        """Analisa e classifica um produto agrícola na imagem"""
        # Converter para HSV (melhor para detecção de cores)
        hsv = cv2.cvtColor(imagem, cv2.COLOR_BGR2HSV)

        # Máscara para isolar o produto
        mascara_geral = np.zeros(imagem.shape[:2], dtype=np.uint8)

        produto_detectado = None
        area_max = 0

        # Verificar todas as classes de produtos
        for produto_id, info in produtos.items():
            # Criar máscara baseada na faixa de cor
            mascara = cv2.inRange(hsv, info["cor_min"], info["cor_max"])

            # Aplicar operações morfológicas para limpar a máscara
            kernel = np.ones((5, 5), np.uint8)
            mascara = cv2.morphologyEx(mascara, cv2.MORPH_OPEN, kernel)
            mascara = cv2.morphologyEx(mascara, cv2.MORPH_CLOSE, kernel)

            # Encontrar contornos
            contornos, _ = cv2.findContours(mascara, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

            for contorno in contornos:
                area = cv2.contourArea(contorno)
                if area > 1000 and area > area_max:  # Filtrar por tamanho mínimo
                    area_max = area
                    produto_detectado = {
                        "id": produto_id,
                        "nome": info["nome"],
                        "contorno": contorno,
                        "area": area
                    }
                    mascara_geral = mascara.copy()

        if produto_detectado is None:
            return None

        # Análise detalhada do produto
        x, y, w, h = cv2.boundingRect(produto_detectado["contorno"])

        # Recortar o produto
        produto_roi = imagem[y:y+h, x:x+w]
        mascara_roi = mascara_geral[y:y+h, x:x+w]

        # Análise de tamanho (baseada na área)
        if produto_detectado["area"] < 5000:
            tamanho = "Pequeno"
        elif produto_detectado["area"] < 15000:
            tamanho = "Médio"
        else:
            tamanho = "Grande"

        # Análise de cor
        hsv_roi = hsv[y:y+h, x:x+w]
        pixels_validos = hsv_roi[mascara_roi > 0]

        if len(pixels_validos) > 0:
            # Usar K-means para encontrar as cores dominantes
            pixels_amostra = pixels_validos[np.random.randint(0, len(pixels_validos), size=min(1000, len(pixels_validos)))]
            kmeans = KMeans(n_clusters=3)
            kmeans.fit(pixels_amostra)
            cores_dominantes = kmeans.cluster_centers_

            # Converter cores para RGB para visualização
            cores_rgb = []
            for cor in cores_dominantes:
                cor_hsv = np.uint8([[cor]])
                cor_bgr = cv2.cvtColor(cor_hsv, cv2.COLOR_HSV2BGR)
                cores_rgb.append(cor_bgr[0][0])
        else:
            cores_rgb = []

        # Análise de qualidade (aqui você pode implementar sua lógica)
        # Por exemplo, procurar por manchas, defeitos, etc.
        # Para este exemplo, usaremos uma heurística simples:
        # - Verificar a uniformidade da cor (desvio padrão baixo = mais uniforme)

        if len(pixels_validos) > 0:
            # Usar apenas o canal de matiz (H) para avaliar uniformidade da cor
            desvio_padrao = np.std(pixels_validos[:, 0])

            if desvio_padrao < 10:
                qualidade = "Excelente"
            elif desvio_padrao < 20:
                qualidade = "Boa"
            else:
                qualidade = "Regular"
        else:
            qualidade = "Indeterminada"

        # Salvar amostra (1 a cada 10 produtos)
        if self.amostras_salvas % 10 == 0:
            timestamp = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
            nome_arquivo = f"agricola_amostras/{produto_detectado['nome']}_{qualidade}_{timestamp}.jpg"
            cv2.imwrite(nome_arquivo, produto_roi)

        self.amostras_salvas += 1

        # Registrar resultado
        self.registrar_produto(produto_detectado["id"], qualidade, tamanho, cores_rgb)

        return {
            "produto": produto_detectado,
            "qualidade": qualidade,
            "tamanho": tamanho,
            "cores": cores_rgb,
            "roi": produto_roi,
            "bbox": (x, y, w, h)
        }

    def registrar_produto(self, produto_id, qualidade, tamanho, cores):
        """Registra informações do produto classificado"""
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")

        # Cores para texto
        cores_txt = "-".join([f"{int(c[0])},{int(c[1])},{int(c[2])}" for c in cores])

        # Salvar no log
        with open(self.log_file, "a", newline="") as f:
            writer = csv.writer(f)
            writer.writerow([timestamp, produtos[produto_id]["nome"], qualidade, tamanho, cores_txt])

        # Atualizar contadores
        if qualidade != "Rejeitado":
            self.produtos_classificados[produto_id] += 1
        else:
            self.produtos_rejeitados += 1

    def obter_estatisticas(self):
        """Retorna estatísticas da sessão atual"""
        duracao = time.time() - self.inicio_sessao
        total_produtos = sum(self.produtos_classificados.values()) + self.produtos_rejeitados

        # Produtos por hora
        if duracao > 0:
            taxa_processamento = (total_produtos / duracao) * 3600
        else:
            taxa_processamento = 0

        return {
            "total": total_produtos,
            "por_tipo": {produtos[k]["nome"]: v for k, v in self.produtos_classificados.items()},
            "rejeitados": self.produtos_rejeitados,
            "taxa_processamento": round(taxa_processamento, 2),
            "duracao_minutos": round(duracao / 60, 2)
        }

# Função principal
def iniciar_classificacao():
    cap = cv2.VideoCapture(0)

    classificador = ClassificadorAgricola()
    modo_analise = False

    print("Controles:")
    print("A - Alternar modo de análise automática")
    print("S - Classificar produto manualmente")
    print("Q - Sair")

    ultimo_processamento = 0

    while True:
        ret, frame = cap.read()
        if not ret:
            break

        # Desenhar área de classificação
        altura, largura = frame.shape[:2]
        cv2.rectangle(frame, (100, 100), (largura-100, altura-100), (0, 255, 0), 2)

        # Interface
        cv2.putText(frame, "CLASSIFICADOR DE PRODUTOS AGRICOLAS", (10, 30),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 255, 255), 2)

        if modo_analise:
            cv2.putText(frame, "Modo: AUTOMATICO", (10, 60),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 255, 0), 2)
        else:
            cv2.putText(frame, "Modo: MANUAL (pressione S)", (10, 60),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.7, (0, 0, 255), 2)

        # Estatísticas
        stats = classificador.obter_estatisticas()
        cv2.putText(frame, f"Total: {stats['total']} produtos", (10, 90),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 255, 255), 2)
        cv2.putText(frame, f"Taxa: {stats['taxa_processamento']} prod/hora", (10, 120),
                    cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 255, 255), 2)

        # Modo automático ou acionamento manual
        agora = time.time()
        if (modo_analise and agora - ultimo_processamento > 2) or cv2.waitKey(1) & 0xFF == ord('s'):
            # Região de interesse
            roi = frame[100:altura-100, 100:largura-100]

            # Classificar produto
            resultado = classificador.analisar_produto(roi)

            if resultado is not None:
                # Mostrar resultados
                produto = resultado["produto"]
                bbox = resultado["bbox"]

                # Ajustar coordenadas para o frame original
                x, y, w, h = bbox
                x += 100
                y += 100

                # Desenhar retângulo
                cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)

                # Mostrar informações
                info_text = [
                    f"Produto: {produto['nome']}",
                    f"Qualidade: {resultado['qualidade']}",
                    f"Tamanho: {resultado['tamanho']}"
                ]

                for i, texto in enumerate(info_text):
                    cv2.putText(frame, texto, (x, y + h + 20 + i*20),
                                cv2.FONT_HERSHEY_SIMPLEX, 0.6, (0, 255, 0), 2)

                # Exibir imagem do produto
                produto_img = resultado["roi"]
                if produto_img.shape[0] > 0 and produto_img.shape[1] > 0:
                    produto_img_resized = cv2.
```
