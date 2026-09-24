# 📡 Ultrassonic Transmission - Comunicação Acústica via Áudio

[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)](https://numpy.org/)
[![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)](https://scipy.org/)
[![PyAudio](https://img.shields.io/badge/PyAudio-PortAudio-green?style=for-the-badge)](https://people.csail.mit.edu/hubert/pyaudio/)

Sistema experimental para **transmissão e recepção de dados via ondas sonoras (quase inaudíveis / ultrassônicas)** entre dispositivos usando apenas microfone e alto-falantes convencionais, sem necessidade de Wi-Fi, Bluetooth ou cabos.

---

## 🔬 Como Funciona

O projeto implementa uma modulação digital acústica por chaveamento de frequência (**FSK - Frequency Shift Keying**) operando na faixa de **19 kHz a 22 kHz** (limite superior da audição humana):

1. **Conversão de Dados**: Os dados de entrada (texto ou arquivo) são codificados em representação hexadecimal (`0-F`).
2. **Mapeamento de Frequências**: Cada símbolo hexadecimal (nibble de 4 bits) é mapeado para um tom de frequência específico:
   $$f = f_{base} + (passo \times valor)$$
   Onde $f_{base} = 20000\text{ Hz}$ e $passo = 100\text{ Hz}$.
3. **Técnica "Entrada de Veludo" & Janelamento Blackman**:
   * Para evitar estalos sonoros provocados por descontinuidades de fase abruptas ao ligar os alto-falantes, aplica-se uma suave rampa de entrada senoidal (Hanning) no tom de sincronismo (*Sync*).
   * As notas de dados são moduladas com janela de Blackman para atenuar harmônicos audíveis.
4. **Recepção e Decodificação**:
   * O receptor captura o áudio do microfone em tempo real via **PyAudio**.
   * Processa os blocos de amostras via **FFT (Fast Fourier Transform)** para detectar o tom de sincronismo ($19000\text{ Hz}$).
   * Identifica as frequências dominantes de cada intervalo e reconverte os valores hexadecimais de volta para o texto original.

---

## 🗂️ Estrutura do Projeto

* `transfer_lib.py`: Biblioteca central com as funções matemáticas de síntese de sinal, normalização, janelamento, gravação de áudio, FFT e decodificação.
* `emissor.py`: Script para transmissão (converte a mensagem em sinal acústico e gera o áudio reproduzido nos alto-falantes).
* `receptor.py`: Script para escuta contínua via microfone, captura e decodificação em tempo real.

---

## 🚀 Instalação e Execução

### 1. Instalar Dependências

Recomenda-se utilizar um ambiente virtual:

```bash
pip install numpy scipy pyaudio
```

> **Nota para Windows**: Se tiver dificuldades na instalação do `pyaudio`, instale via pipwin: `pip install pipwin && pipwin install pyaudio`.

### 2. Transmitir Dados (Emissor)

Execute o emissor para gerar e transmitir a mensagem:
```bash
python emissor.py
```

### 3. Receber Dados (Receptor)

No dispositivo receptor (ou em outro terminal com o microfone ligado):
```bash
python receptor.py
```
Ao detectar a frequência de sincronismo, o receptor decodificará a mensagem transmitida automaticamente na tela.

---

## 👨‍💻 Autor

Desenvolvido por **João Mateus** ([@aomaaj](https://github.com/aomaaj)).
