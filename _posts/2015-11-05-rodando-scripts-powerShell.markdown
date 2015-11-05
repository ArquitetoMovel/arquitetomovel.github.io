---
layout: post
title:  "Rodando scripts do PowerShell"
date:   2015-11-05 00:21:00
categories: PowerShell
---

No artigo [Introdução ao Powershell](http://arquitetomovel.com.br/powershell/2015/11/02/introducao-ao-powershell.html), vimos como executar um simples **Olá Mundo** diretamente no prompt de comando.

Mas e se quisermos rodar essa instrução diversas vezes no nosso Windows?

Para tal tarefa, teremos que salvar essa instrução em um arquivo **.ps1**. Essa é a extensnão para os scripts **PowerShell**.

* c:\olamundo.ps1

Com arquivo **.ps1**, temos a oportunidade de executar esse script em um arquivo de sistema **.bat**.

No arquivo **.bat** deveremos colocar a seguinte linha: 

`powershell C:\olamundo.ps1`

A partir desse ponto,  podemos colocar nosso **bat** em um agendador de tarefas, na inicialização do sistema ou simplesmente executar acessando o mesmo.

Ainda podemos ter as seguintes opções na execução da linha de comando do PowerShell: 

* **-ExecutionPolicy** Para executar um script que requer permissões especiais do Windows, é possivél reescrever as politicas 
de segurança para rodar o seu script. Para ignorar politicas que proibibe execução de sripts, usamos a seguinte combinação:
 `-ExecutionPolicy ByPass`.
* **-Command** quando incluimos opções adicionais, é necessário adicionar esse paramentro antes da chamada do script *ps1*.
