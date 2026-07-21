# YouTube Audio Transcriber

## Aplicação

Esta aplicação automatiza o processo de pesquisa, download e transcrição de vídeos do YouTube.

O fluxo de execução consiste nas seguintes etapas:

1. Pesquisa vídeos no YouTube utilizando a YouTube Data API v3.
2. Baixa o áudio de cada vídeo encontrado utilizando o `yt-dlp`.
3. Transcreve o áudio utilizando o modelo Whisper através da biblioteca `faster-whisper`.
4. Salva a transcrição em formato `.txt`.
5. Caso algum vídeo não possa ser baixado (por exemplo, erro HTTP 403, vídeo privado ou removido), o erro é registrado em um arquivo de log e o processamento continua normalmente para os demais vídeos.

---

# Estrutura do projeto

```
.
├── audios/                # Áudios baixados do YouTube
├── transcripts/           # Transcrições geradas
├── logs/                  # Logs de erros de download
│
├── config.py
├── downloader.py
├── search.py
├── transcriber.py
├── main.py
├── .env
├── requirements.txt
└── README.md
```

---

# Tecnologias utilizadas

- Python 3
- YouTube Data API v3
- yt-dlp
- Faster-Whisper
- CTranslate2
- FFmpeg
- Google API Python Client
- python-dotenv

---

# Pré-requisitos

Antes de executar o projeto, é necessário instalar:

- Python 3.10 ou superior
- FFmpeg
- Uma chave de API da YouTube Data API v3 (obtida via Google Cloud)

---

# Clonando o projeto

```bash
git clone https://github.com/grazianii/youtube_audio_transcriber.git

cd youtube_audio_transcriber
```

---

# Instalando as dependências

Via arquivo `requirements.txt`:

```bash
pip install -r requirements.txt
```

Ou instale manualmente:

```bash
pip install yt-dlp
pip install faster-whisper
pip install google-api-python-client
pip install python-dotenv
```

---

# Instalando o FFmpeg

O Faster-Whisper depende do FFmpeg para processar arquivos de áudio.

## Windows

Faça o download:

https://ffmpeg.org/download.html

Extraia os arquivos.

Adicione a pasta **bin** do FFmpeg às variáveis de ambiente (PATH).

Exemplo:

```
C:\ffmpeg\bin
```

Depois confirme a instalação:

```bash
ffmpeg -version
```

---

# Criando uma chave da API do YouTube

O projeto utiliza a **YouTube Data API v3** para pesquisar vídeos.

## 1. Acesse

https://console.cloud.google.com/

---

## 2. Crie um projeto

Clique em **Novo Projeto**.

---

## 3. Ative a API

No menu:

```
APIs e Serviços
```

Clique em:

```
Ativar APIs e Serviços
```

Pesquise por:

```
YouTube Data API v3
```

Clique em **Ativar**.

---

## 4. Gere uma chave

Acesse:

```
APIs e Serviços

→ Credenciais

→ Criar Credenciais

→ Chave de API
```

Será gerada uma chave semelhante a:

```
AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

---

# Configurando o projeto

Crie um arquivo chamado:

```
.env
```

Adicione sua chave:

```env
YOUTUBE_API_KEY=SUA_CHAVE_DA_API
```

Exemplo:

```env
YOUTUBE_API_KEY=AIzaSyXXXXXXXXXXXXXXXXXXXXXXXX
```

O projeto lê automaticamente essa variável utilizando o pacote `python-dotenv`.

---

# Personalizando as pesquisas

As pesquisas realizadas no YouTube ficam definidas em:

```
main.py
```

Exemplo:

```python
QUERIES = [
    "currículo TI",
    "linkedin desenvolvedor",
    "engenheiro de dados",
]
```

Basta alterar essa lista para pesquisar qualquer outro assunto.

---

# Executando o projeto

Execute:

```bash
python main.py
```

O sistema irá:

- Pesquisar vídeos;
- Baixar o áudio de cada vídeo;
- Gerar a transcrição;
- Salvar os arquivos na pasta `transcripts`.

---

# Arquivos gerados

## Áudios

```
audios/
```

---

## Transcrições

```
transcripts/
```

Cada vídeo gera um arquivo `.txt`.

---

## Logs de erro

```
logs/
```

Exemplo:

```
download_errors_2026-07-21.log
```

Caso algum vídeo não possa ser baixado, a execução **não é interrompida**.

O erro é registrado no arquivo de log e o próximo vídeo é processado normalmente.

Exemplo:

```
================================================================================
Data/Hora: 2026-07-21 20:15:33

URL:
https://www.youtube.com/watch?v=XXXXXXXX

Erro:
ERROR: unable to download video data: HTTP Error 403: Forbidden
```

---

# O que deve ser alterado por quem utilizar o projeto

Antes da primeira execução, o usuário deverá:

- Criar uma chave da YouTube Data API v3.
- Criar um arquivo `.env`.
- Informar sua chave em `YOUTUBE_API_KEY`.
- Instalar todas as dependências.
- Instalar o FFmpeg.
- Alterar a lista `QUERIES` no `main.py`, caso deseje pesquisar outros temas.

Nenhuma outra modificação é necessária para executar o projeto.

---

# Observações

- É necessário possuir conexão com a internet.
- Alguns vídeos do YouTube não podem ser baixados por restrições da própria plataforma (vídeos privados, removidos, bloqueados por região, etc.).
- Na primeira execução, o Faster-Whisper fará automaticamente o download do modelo de transcrição utilizado.
- O tempo de processamento depende da velocidade da conexão e do desempenho do computador.
