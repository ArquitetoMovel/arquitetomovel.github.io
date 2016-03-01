---
layout: post
title:  "Rodando scripts do PowerShell"
date:   2015-11-05 18:40:00
categories: PowerShell
---
No artigo [Introdução ao Powershell] (http://arquitetomovel.com.br/powershell/2015/11/02/introducao-ao-powershell.html), vimos como executar um simples **Olá Mundo** diretamente no prompt de comando.

Mas e se quisermos rodar essa instrução diversas vezes no nosso Windows?

Para tal tarefa, teremos que salvar essa instrução em um arquivo **.ps1**. Essa é a extensão para os scripts **PowerShell**.

* c:\olamundo.ps1

Com arquivo **.ps1**, temos a oportunidade de executar esse script em um arquivo de sistema **.bat**.

No arquivo **.bat** deveremos colocar a seguinte linha: 

`powershell C:\olamundo.ps1`

A partir desse ponto, podemos colocar nosso **bat** em uma tarefa agendada, na inicialização do sistema ou simplesmente executar acessando o mesmo.

Ainda podemos ter as seguintes opções na execução da linha de comando do PowerShell: 

* **-ExecutionPolicy** Para executar um script que requer permissões especiais do Windows, é possível reescrever as politicas 
Segurança para rodar o seu script. Para ignorar políticas que proíbe execução de scripts, usamos a seguinte combinação:
 `-ExecutionPolicy ByPass`.
* **-Command** quando incluímos opções adicionais, é necessário adicionar esse parâmetro antes da chamada do script *ps1*.

