---
title: Conteinerização com Docker e Ambientes Virtuais
date: 2026-06-07T22:57:00-03:00
draft: false
tags:
  - docker
  - python
  - flask
  - linux
  - devops
categories:
  - Desenvolvimento
  - Infraestrutura
author: Pamela Iwabuchi
description: Um guia prático e direto sobre as diferenças entre Docker e venv, estrutura de Dockerfile, Docker Compose e inicialização de aplicações com Flask.
---
## 1. O que é Docker?

O docker é uma plataforma open source que permite o empacotamento e execução de aplicações em containers. Isso resolve o problema clássico onde um software funciona no computador do desenvolvedor, mas dá erro em outros computadores, pois o docker garante que a mesma aplicação com as mesmas dependências rode da mesma forma em qualquer sistema (seja windows, linux, etc.).

## 2. Diferença de Docker para máquina virtual

A máquina virtual trabalha com o hardware, para rodar um sistema operacional completo. Já o docker virtualiza apenas o aplicativo, suas dependências e bibliotecas necessárias. Não possui SO completo.

## 3. Pra que serve e qual a estrutura do Dockerfile?

O dockerfile é um arquivo de texto que contém todas as intruções e passos necessários para criar uma imagem de docker personalizada - como se fosse uma receita de bolo. Ela empacota o ambiente, as bibliotecas e a aplicação em um único pacote pronto pra rodar. Isso evita o trabalho de ter que montar a aplicação e baixar tudo passo a passo. 

``` Dockerfile
# Define a imagem base do sistema/linguagem
FROM python:3.11

# Define em qual diretório o código será copiado e todos os comandos serão executados. Se não houver a pasta app, ele cria na hora. Sem isso, tudo seria baixado na pasta raiz e criaria problemas de organização e é considerado uma má prática.
WORKDIR /app

# Copia os ficheiros de dependências na pasta app que acabamos de criar
COPY requirements.txt .

# Executa comandos para instalar as dependências
RUN pip install --no-cache-dir -r requirements.txt

# Copia o resto do código da aplicação
COPY . .

# Informa a porta que o container vai expor
EXPOSE 8000

# Comando padrão executado quando o container iniciar
CMD ["python", "main.py"]

```

## 4. Pra que serve e qual a estrutura do docker-compose?

Enquanto o docker gerencia um container por vez, o docker compose gerencia vários containers juntos de forma automatizada. 

Vamos supor que no nosso projeto precisamos de Flask e MySQL pra rodar. Com o Docker puro precisariamos digitar manualmente no terminal toda a configuração de rede, portas e volumes de cada container. Com o docker compose não precisamos digitar todos os comandos. Simplesmente escrevemos a estrutura de como os containers devem ser criados dentro do arquivo de texto `docker-compose.yml`.

Depois que o arquivo está pronto, só precisamos digitar um único comando no terminal: 

```bash
docker compose up
```

- **O Docker Compose** não substitui o Docker; ele apenas dá ordens ao Docker para criar múltiplos containers de forma coordenada a partir de um arquivo `yml`, nos poupando de gerenciar cada container manualmente pela linha de comando.

``` YAML

version: '3.8'

services:
  # ---- PRIMEIRO SERVIÇO: Aplicação Python ----
  web_api:
    build: # Configuração de construção local
      context: . # Onde está o projeto localmente
      dockerfile: Dockerfile # O script de receita da imagem
    image: lumina_api:1.0.0 # Nome e tag da imagem gerada pelo build
    container_name: python_api_container # Força um nome fixo para o container
    ports:
      - "8000:8000" # Mapeamento de Portas (Máquina Hospedeira:Dentro do Container)
    volumes:
      - .:/app # Bind Mount: Sincroniza o código local com o container em tempo real
    environment:
      - DATABASE_URL=postgresql://admin:secret@db_postgres:5432/lumina_db
    depends_on:
      db_postgres:
        condition: service_healthy # Gerenciamento de Dependência baseado no Healthcheck

  # ---- SEGUNDO SERVIÇO: Banco de Dados PostgreSQL ----
  db_postgres:
    image: postgres:15-alpine # Baixa uma imagem pronta e otimizada do Docker Hub
    container_name: postgres_db_container
    environment: # Variáveis de ambiente para configurar o banco de dados
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: lumina_db
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data # Armazena os dados do banco no volume global
    healthcheck: # Monitor de integridade do serviço
      test: ["CMD-SHELL", "pg_isready -U admin -d lumina_db"]
      interval: 5s
      timeout: 5s
      retries: 5

# ---- DECLARAÇÃO DE VOLUMES GLOBAIS ----
volumes:
  pgdata: # Aloca um espaço persistente gerenciado pelo Docker no HD do host
```

## Como criar o venv

``` bash
# cria o ambiente virtual
python3 -m venv .venv

# ativa o ambiente virtual
source .venv/bin/activate

# desativa
deactivate
``` 

## Flask

``` bash
# Atualiza o gerenciador de pacotes local
pip install --upgrade pip

# Instala o Flask
pip install Flask

# Exporta as dependências exatas instaladas para o arquivo 
pip freeze > requirements.txt
```

``` python 
# 1. Importamos o Flask e a função render_template
from flask import Flask, render_template

app = Flask(__name__)

# Rota principal (Home) - Vai ler o arquivo index.html
@app.route('/')
def home():
    # O Flask procura automaticamente o arquivo dentro da pasta 'templates/'
    return render_template('index.html'), 200

# Rota de Contato - Vai ler o arquivo contato.html
@app.route('/contato')
def contato():
    return render_template('contato.html'), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
```

