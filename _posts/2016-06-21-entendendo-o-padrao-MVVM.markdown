---
layout: post
title:  "Entendendo o padrão MVVM"
date:   2016-06-21 07:07
categories: Microsoft
---

**MVVM** é um padrão criado pela Microsoft para construção de aplicações Windows 
utilizando sua tecnologia de marcação **XAML** para construção de interfaces gráficas.

Esse padrão proporciona uma separação entre linguagem XAML e a linguagem que leva e admite
 dados de entrada através da interface com o usuário sem a necessidade de realizar uma 
 atribuição dura entre elemento XAML e código C#.

### Exemplo: 
` this.txtUIName.Text = nomePessoa`

Essa seria a forma de **Bind** mais simples entre um elemento da interface com o usuário 
e a aplicação, trata-se de uma atribuição simples utilizando camada de eventos padrão da pagina XAML **codebehind**.

Porém quando temos que exibir o mesmo dado em diferentes interfaces e o nosso 
modelo pode se conectar a diferentes fontes dados, temos um problema com o modelo acima 
por não conseguir reutilizar o nosso contrato de dados. 
Também temos problemas com o Designer da aplicação que utiliza o **Visual Blend** e não 
precisa lidar diretamente com código C#.

> Com o MVVM, temos um objeto intermediário entre a UI-XAML e minhas classes de modelo 
ou DATA SERVICES.

## Elementos do MVVM 

A seguir vamos detalhar os principais componentes que viabilizam a aplicação do *MVVM*

### 1. X:Bind ou Bind

Essas tags são utilizadas 