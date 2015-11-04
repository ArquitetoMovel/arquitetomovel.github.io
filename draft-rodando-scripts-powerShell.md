# Rodando scripts do PowerShell

No artigo ![Introdução ao Powershell](), vimos como executar um simples **Olá Mundo** diretamente no prompt de comando.

Mas e se quisermos rodar essa instrução diversas vezes no nosso Windows?

Para tal tarefa, teremos que salvar essa instrução em um arquivo **.ps1**. Essa é a extensnão para os scripts **PowerShell**.

* c:\olamundo.ps1

Com arquivo **.ps1**, temos a oportunidade de executar esse script em um arquivo de sistema **.bat**.

No arquivo **.bat** deveremos colocar a seguinte linha: 

`powershell C:\olamundo.ps1`

A partir desse ponto,  podemos colocar nosso **bat** em um agendador de tarefas, na inicialização do sistema ou simplesmente executar acessando o mesmo.

Ainda podemos ter as seguintes opções na execução da linha de comando do PowerShell: 

* **-NoProfile** 
* **-ExecutionPolicy**
* **-Command** quando incluimos opções adicionais, é necessário adicionar esse paramentro antes da chamada do script *ps1*.
