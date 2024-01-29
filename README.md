# Repo: Trabalhando com Machine Learning na Prática no Azure ML
### Objetivo
Repositório criado para entregar o projeto final da etapa: Trabalhando com Machine Learning na Prática no Azure ML.

# Requerimentos do projeto
### Como Entregar esse projeto?

Chegou a hora de você construir um portfólio ainda mais rico e impressionar futuros recrutadores, para isso é sempre importante mostrar os resultados do seu esforço e como você os obteve deixando claro o seu racional, para isso faça da seguinte maneira:

1. Crie um novo repositório no github com um nome a sua preferência
2. Crie um modelo de previsão com seus devidos pontos de extremidade configurados
3. Escreva o passo a passo desse processo em um readme.md de como você chegou nessa etapa
4. Salve nesse repositório o readme.md e o arquivo .json de pontos de extremidade
5. Compartilhe conosco o link desse repositório através do botão 'entregar projeto'

# Escreva o passo a passo desse processo em um readme.md de como você chegou nessa etapa

### Links úteis

[https://aka.ms/ai900-auto-ml](https://aka.ms/ai900-auto-ml)<br>
[https://aka.ms/ai900-azure-ai-services](https://aka.ms/ai900-azure-ai-services)

### Passos para criar um **Azure Machine Learning workspace**

⭐️ A minha sugestão é deixar toda a plataforma em inglês ⭐️<br>
⚠️ É necessário se cadastrar em https://portal.azure.com com um cartão de crédito 💳

1. Na tela inicial do Portal, selecione **+ Create a resource**, digite na pesquisa *Machine Learning* e crie um novo recurso **Azure Machine Learning** com as seguintes configurações (as demais serão autopreenchidas):
    * Resource group: LABAI-900
    * Name: laboratorioai900 (Create New)
    * Region: East US

| 1.1 | 1.2 | 1.3 |
|:--------:|:--------:|:--------:|
|![Imagem 1](./imagens/tela%201.png)|![Imagem 2](./imagens/tela%202.png)|![Imagem 3](./imagens/tela%203.png)|

2. Selecione **Review + create**, depois **Create**.

| 2.1 |
|:--------:|
|![Imagem 4](./imagens/tela%204.png)|

3. Espere alguns minutos até que o workspace esteja criado. Clique em **Go to resource**. Clique sobre o botão **Launch studio**.

| 3.1 | 3.2 |
|:--------:|:--------:|
|![Imagem 5](./imagens/tela%205.png)|![Imagem 6](./imagens/tela%206.png)|

### Passos para treinar um modelo de Machine Learning

Após clicar no botão **Launch studio**, a plataforma do [Azure Machine Learning studio](https://ml.azure.com) já vai abrir dentro do workspace.

4. Clique em **Automated ML** no menu lateral. Depois crie um **+ New Automated ML job**.

| 4.1 | 4.2 |
|:--------:|:--------:|
|![Imagem 7](./imagens/tela%207.png)|![Imagem 8](./imagens/tela%208.png)|

5. Forneça as informações abaixo e clique em **Next** após preencher as informações como recomendado.

    * **Basic settings**:
        * **Job name**: mslearn-bike-automl
        * **New experiment name**: mslearn-bike-rental
        * **Description**: Automated machine learning for bike rental prediction
        * **Tags**: none
    * **Task type & data**:
        * **Select task type**: Regression
        * **Select dataset**: Create a new dataset with the following settings:
            * **Data type**:
                * **Name**: bike-rentals
                * **Description**: Historic bike rental data
                * **Type**: Tabular
            * **Data source**:
                * Select **From web files**
            * **Web URL**:
                * **Web URL**: https://aka.ms/bike-rentals
                * **Skip data validation**: do not select
            * **Settings**:
                * **File format**: Delimited
                * **Delimiter**: Comma
                * **Encoding**: UTF-8
                * **Column headers**: Only first file has headers
                * **Skip rows**: None
                * **Dataset contains multi-line data**: do not select
            * **Schema**:
                * Include all columns other than **Path**
                * Review the automatically detected types

        Selecione **Create** ao terminar as configurações acima. Após a criação do dataset, selecione o dataset **bike-rentals** para enviar o job Automated ML.
    
    * **Task settings**:
        * **Task type**: Regression
        * **Dataset**: bike-rentals
        * **Target column**: Rentals (integer)
        * **Additional configuration settings**:
            * **Primary metric**: Normalized root mean squared error
            * **Explain best model**: Unselected
            * **Use all supported models**: Unselected.
            * **Allowed models**: Select only **RandomForest** and **LightGBM**.
        * **Limits**: Expand this section
            * **Max trials**: 3
            * **Max concurrent trials**: 3
            * **Max nodes**: 3
            * **Metric score threshold**: 0.085 (so that if a model achieves a normalized root mean squared error metric score of 0. 085 or less, the job ends.)
            * **Timeout**: 15
            * **Iteration timeout**: 15
            * **Enable early termination**: Selected
        * **Validation and test**:
            * **Validation type**: Train-validation split
            * **Percentage of validation data**: 10
            * **Test dataset**: None
        * **Compute**:
            * **Select compute type**: Serverless
            * **Virtual machine type**: CPU
            * **Virtual machine tier**: Dedicated
            * **Virtual machine size**: Standard_DS3_V2
            * **Number of instances**: 1

| 5.1 - Resumo de configurações |
|:--------:|
|![Imagem 9](./imagens/tela%209.png)|

6. Clique em **Submit training job**.

7. Espere até que o job finalize. Pode demorar.

| 7.1 |
|:--------:|
|![Imagem 10](./imagens/tela%2010.png)|

### Revise o melhor modelo

Quando o job estiver finalizado, você poderá visualizar o melhor modelo treinado.

8. Na guia **Overview** do job automatizado, observe o resumo do melhor modelo treinado.
9. Selecione o link abaixo do nome **Algorithm name** para visualizar os detalhes do melhor modelo.

| 9.1 |
|:--------:|
|![Imagem 11](./imagens/tela%2011.png)|

10. Selecione a guia **Metrics** (clicar em **Refresh**), depois selecione os gráficos **residuals** e **predicted_true**.
    * Os gráficos revelam a performance do modelo.
    * **residuals** é a diferença entre os valores preditivos e os valores atuais.
    * **predicted_true** compara os valores preditivos contra os valores verdadeiros.

| 10.1 | 10.2 |
|:--------:|:--------:|
|![Imagem 12](./imagens/tela%2012.png)|![Imagem 13](./imagens/tela%2013.png)|

### Implemente e teste o modelo

11. Na guia **Model** do melhor modelo treinado, selecione **Deploy** e use a opção **Web service** para implementar o modelo com as seguintes configurações:

    * **Name**: predict-rentals
    * **Description**: Predict cycle rentals
    * **Compute type**: Azure Container Instance
    * **Enable authentication**: Selected

| 11.1 | 11.2 |
|:--------:|:--------:|
|![Imagem 14](./imagens/tela%2014.png)|![Imagem 15](./imagens/tela%2015.png)|

13. Espere o início da implementação (isso vai demorar um pouco), o **Deploy status** do endpoint **predict-rentals** vai aparecer *Running*. Assim que terminar, o status vai aparecer *Succeeded*. Leva aproximadamente 15 minutos todo o processo.

| 13.1 | 13.2 |
|:--------:|:--------:|
|![Imagem 16](./imagens/tela%2016.png)|![Imagem 17](./imagens/tela%2017.png)|

### Teste o serviço implementado

Agora você pode testar o serviço implementado.

14. Ainda no **Azure Machine Learning studio**, no menu lateral, selecione **Endpoints** e, na guia **Real-time endpoints**, clique sobre o **predict-rentals**.

| 14.1 |
|:--------:|
|![Imagem 18](./imagens/tela%2018.png)|

15. Na página do **predict-rentals**, selecione a guia **Test**. No painel **Input data to test endpoint**, substitua o template JSON, fornecido abaixo, depois clique no botão *Test*:
    ```json
    {
        "Inputs": { 
            "data": [
            {
                "day": 1,
                "mnth": 1,   
                "year": 2022,
                "season": 2,
                "holiday": 0,
                "weekday": 1,
                "workingday": 1,
                "weathersit": 2, 
                "temp": 0.3, 
                "atemp": 0.3,
                "hum": 0.3,
                "windspeed": 0.3 
            }
            ]    
        },   
        "GlobalParameters": 1.0
    }
    ```

| 15.1 |
|:--------:|
|![Imagem 19](./imagens/tela%2019.png)|

    A saída deve aparecer algo similar a isso:
![Imagem 20](./imagens/tela%2020.png)

### Revisão

Foi utilizado um conjunto de dados históricos de aluguel de bicicletas para treinar um modelo. O modelo prevê o número esperado de aluguéis de bicicletas num determinado dia, com base em características sazonais e meteorológicas.

### Deletar o o endpoint e o workspace

Para isso acesse o tutorial fornecido em links úteis no início deste tutorial.