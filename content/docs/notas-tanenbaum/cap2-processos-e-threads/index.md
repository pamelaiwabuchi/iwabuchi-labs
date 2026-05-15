---
title: "Notas sobre Tanenbaum"
description: "Breves notas sobre o capítulo 2 de tanenbaum - Sistemas Operacionais"
date: 2026-05-15T15:00:00-03:00
draft: true
categories: ["Sistemas Operacionais", "Livros"]
tags: ["Tanenbaum", "Processos", "Threads"]
weight: 2
---
Um processo é uma instancia de um programa em execução Conceitualmente, cada processo tem sua própria CPU virtual.

4 eventos principais faz com que os processos sejam criados:

1. Inicialização do sistema;
2. Execução de uma chamada de sistema de criação de processo por um processo em execução;
3. Solicitação de um usuário para criar um novo processo
4. Inicio de uma tarefa em lote.

**fork** : chamada única de sistema UNIX para criar um novo processo. - Cria um clone exato do processo que a chamou. Após `fork` , os dois processos, o pai e o filho tem a mesma imagem de memória, as mesmas variáveis de ambiente e os mesmos arquivos abertos. - Normalmente o processo filho executa `execve` para mudar sua imagem de memória e executar um novo programa.

A função fork é a unica que retorna 2 valores diferentes ao mesmo tempo (um em cada processo)

Daemons - Processos de segundo plano pra lidar com atividades, como email, paginas web, notícias, impressão e assim por diante. - Não possui interface com usuário ou terminal de controle.
