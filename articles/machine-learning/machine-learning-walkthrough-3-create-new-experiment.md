<properties
	pageTitle="Etapa 3: Criar um novo teste de Aprendizado de Máquina | Microsoft Azure"
	description="Etapa 3 do desenvolvimento de um passo a passo de solução de previsão: criar um novo teste de treinamento no Estúdio de Aprendizado de Máquina do Azure."
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/09/2016" 
	ms.author="garye"/>


# Passo a passo Etapa 3: Criar um novo teste de Aprendizado de Máquina do Azure

Esta é a terceira etapa do passo a passo, [Desenvolver uma solução de análise preditiva com o Aprendizado de Máquina do Azure](machine-learning-walkthrough-develop-predictive-solution.md)


1.	[Criar um espaço de trabalho do Aprendizado de Máquina](machine-learning-walkthrough-1-create-ml-workspace.md)
2.	[Carregar dados existentes](machine-learning-walkthrough-2-upload-data.md)
3.	**Criar um novo teste**
4.	[Treinar e avaliar os modelos](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.	[Implantar o serviço Web](machine-learning-walkthrough-5-publish-web-service.md)
6.	[Acessar o serviço Web](machine-learning-walkthrough-6-access-web-service.md)

----------

A próxima etapa neste passo a passo é criar um novo teste no Estúdio de Aprendizado de Máquina que usa o conjunto de dados que carregamos.

1.	No Estúdio, clique em **+NOVO** na parte inferior da janela.
2.	Selecione **TESTE** e, em seguida, selecione "Teste em branco". Selecione o nome do teste padrão na parte superior da tela e renomeie-o para algo significativo

	> [AZURE.TIP] É uma boa prática preencher**Resumo**e**Descrição** para o experimento no painel **Propriedades**. Essas propriedades lhe dão a chance de documentar o experimento para que qualquer pessoa que olhe para ele mais tarde compreenda as suas metas e a metodologia.

3.	Na paleta do módulo à esquerda das telas de teste, expanda **Conjuntos de dados salvos**.
4.	Localize o conjunto de dados que você criou em**Meus Conjuntos de Dados**e arraste-o para a tela. Você também pode localizar o conjunto de dados inserindo o nome na caixa **Pesquisar** acima da paleta.  

##Preparar os dados
É possível exibir as 100 primeiras linhas dos dados, e algumas informações estatísticas de todo o conjunto de dados, clicando na porta de saída do conjunto de dados (o círculo pequeno na parte inferior) e selecionando **Visualizar**.

Como o arquivo de dados não foi fornecido com títulos de coluna, o Estúdio forneceu títulos genéricos (Col1, Col2 *etc.*). Bons títulos de coluna não são essenciais para criar um modelo, mas facilitam o trabalho com os dados no teste. Também, quando eventualmente publicarmos esse modelo em um serviço Web, os títulos ajudarão a identificar as colunas para o usuário do serviço.

Podemos adicionar títulos de coluna usando o módulo [Editor de metadados][metadata-editor]. Você usa o módulo [Editor de metadados][metadata-editor] para alterar os metadados associados a um conjunto de dados. Nesse caso, ele pode fornecer nomes mais amigáveis para títulos de coluna.

Para usar o [Editor de Metadados][metadata-editor], você deve determinar quais colunas deseja modificar (nesse caso, todas) e especificar a ação a ser executada nessas colunas (nesse caso, alterar os títulos).

1.	Na paleta de módulo, digite "metadados" na caixa **Pesquisar**. Você verá o [Editor de Metadados][metadata-editor] na lista de módulos.
2.	Clique e arraste o módulo [Editor de Metadados][metadata-editor] nas telas e arraste-o para baixo do conjunto de dados que adicionamos anteriormente.
3.	Conecte o conjunto de dados ao [Editor de Metadados][metadata-editor]\: clique na porta de saída do conjunto de dados (o círculo pequeno na parte inferior do conjunto de dados), arraste para a porta de entrada do [Editor de Metadados][metadata-editor] (o círculo pequeno na parte superior do módulo) e solte o botão do mouse. O conjunto de dados e o módulo permanecerão conectados mesmo se você mover um deles nas telas.

    O teste deve se parecer como o seguinte:

    ![Adicionando Editor de metadados][2]
    
    O ponto de exclamação vermelho indica que não definimos as propriedades deste módulo ainda. Faremos isso em seguida.
    
    > [AZURE.TIP] É possível adicionar um comentário em um módulo ao clicar duas vezes nele e inserir o texto. Isso pode ajudar a ver rapidamente o que o módulo está fazendo em seu experimento. Nesse caso, clique duas vezes no módulo [Editor de Metadados][metadata-editor] e digite o comentário "Adicionar títulos de coluna". Clique em qualquer lugar na tela para fechar a caixa de texto. Clique na seta para baixo no módulo para exibir o comentário.

4.	Selecione [Editor de Metadados][metadata-editor] e, no painel **Propriedades** à direita da tela, clique em **Iniciar seletor de colunas**.
5.	Na caixa de diálogo **Selecionar colunas**, configure o campo **Começar com** para "Todas as colunas".
6.	A linha abaixo **Começar com** permite incluir ou excluir colunas específicas para o [Editor de Metadados][metadata-editor] modificar. Como queremos modificar *todas* as colunas, exclua essa linha clicando no sinal de menos ("-") à direita da linha. A caixa de diálogo deve ter esta aparência: ![Seletor de coluna com todas as colunas selecionadas][4]
7.	Clique na marca de seleção **OK**.
8.	Volte ao painel **Propriedades**, procure o parâmetro **Novos nomes de coluna**. Neste campo, insira uma lista de nomes para as 21 colunas no conjunto de dados, separadas por vírgulas e na ordem da coluna. Você pode obter os nomes de colunas na documentação do conjunto de dados no site UCI ou, por conveniência, você pode copiar e colar a seguinte lista:  

		  Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  

    O painel de Propriedades ficará semelhante a este:

    ![Propriedades do Editor de metadados][1]

> [AZURE.TIP] Se quer verificar os títulos de coluna, execute o teste (clique em **EXECUTAR** abaixo da tela do teste). Quando ele terminar a execução (uma marca de seleção verde aparecerá na [Editor de Metadados][metadata-editor]), clique na porta de saída do módulo [Editor de Metadados][metadata-editor] e selecione **Visualizar**. Você pode exibir a saída de qualquer módulo da mesma maneira para exibir o progresso dos dados durante o teste.

##Criar conjuntos de dados de treinamento e teste
A próxima etapa do teste é gerar conjuntos de dados separados que serão utilizados para treinamento e teste de nosso modelo.

Para isso, usamos o módulo [Dividir Dados][split].

1.	Localize o módulo [Dividir Dados][split], arraste-o nas telas e conecte-o ao último módulo do [Editor de metadados][metadata-editor].
2.	Por padrão, a taxa de divisão é 0,5 e o parâmetro **Divisão aleatória** é definido. Isso significa que metade dos dados aleatórios sairá por uma porta do módulo de [Dividir Dados][split], e a outra metade sairá por outra porta. Você pode ajustar isso, bem como o parâmetro **Semente aleatória**, para alterar a divisão entre dados de treinamento e teste. Para este exemplo, deixaremos como está.
	> [AZURE.TIP] A propriedade **Fração de linhas no primeiro conjunto de dados de saída** determina a quantidade de dados que saem através da porta de saída à esquerda. Por exemplo, se você definir a taxa em 0,7, então, 70% dos dados sairão pela porta esquerda e 30% pela porta direita.  
3. Clique duas vezes no módulo [Dividir Dados][split] e insira o comentário, "Dividir dados de treinamento/teste em 50%". 

Podemos usar as saídas do módulo [Dividir Dados][split], entretanto, da forma como desejarmos, mas vamos escolher usar a saída à esquerda para dados de treinamento e a saída à direita para dados de teste.

Como mencionado no site UCI, o custo de uma classificação incorreta de um risco de crédito alto como baixo é cinco vezes maior do que o custo da classificação incorreta de um risco baixo como alto. Para isso, iremos gerar um novo conjunto de dados que reflita essa função de custo. No novo conjunto de dados, cada exemplo de alto risco é replicado cinco vezes, enquanto cada exemplo de baixo risco não será replicado.

Podemos fazer essa replicação usando o código R:

1.	Localizar e arrastar o módulo [Executar Script R][execute-r-script] para a tela do experimento e conectar-se à porta de saída à esquerda do módulo [Dividir Dados][split] para a primeira porta de entrada ("Dataset1") do módulo [Executar Script R][execute-r-script].
2. Clique duas vezes no módulo [Executar Script R][execute-r-script] e insira o comentário "Definir ajuste de custo".
2.	No painel **Propriedades**, exclua o texto padrão no parâmetro **Script** e insira esse script:

		  dataset1 <- maml.mapInputPort(1)
		  data.set<-dataset1[dataset1[,21]==1,]
		  pos<-dataset1[dataset1[,21]==2,]
		  for (i in 1:5) data.set<-rbind(data.set,pos)
		  maml.mapOutputPort("data.set")


Precisamos fazer essa mesma operação de replicação para cada saída do módulo [Dividir Dados][split] de forma que os dados de treinamento e teste tenham os mesmos ajustes de custo.

1.	Clique com o botão direito do mouse no módulo [Executar script R][execute-r-script] e selecione **Copiar**.
2.	Clique com o botão direito do mouse nas telas de teste e selecione **Colar**.
3.	Conecte a primeira porta de entrada do módulo [Executar script R][execute-r-script] à porta de saída à direita do módulo [Dividir Dados][split].  

> [AZURE.TIP] A cópia do módulo Executar script R contém o mesmo script que o módulo original. Quando você copia e cola um módulo nas telas, a cópia mantém todas as propriedades do original.

Nosso teste agora se parece com esse:

![Adicionando módulo Divisão e Scripts R][3]

Para obter mais informações sobre como usar scripts R em seus testes, consulte [Estender seu teste com R](machine-learning-extend-your-experiment-with-r.md).

**Em seguida: [Treinar e avaliar os modelos](machine-learning-walkthrough-4-train-and-evaluate-models.md)**


[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/create1.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/create2.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/create3.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/columnselector.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

<!---HONumber=AcomDC_0316_2016-->