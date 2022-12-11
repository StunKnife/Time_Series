# Séries Temporais

- Neste repositório é armazenada algumas **funções** para aplicações em **séries temporais**. Podemos destacar os seguintes tópicos:

  1. Modelos: ARIMA, SARIMA e alisamento exponencial
  2. Teste de séries estacionárias
  3. Decomposição da série temporal
  4. Análise de autocorrelação
  5. Análise de diagnóstico

# Dataset

Para treinar séries temporais é possível utilizar dois datasets disponibilizados nos seguintes repositórios:

* url1='https://raw.githubusercontent.com/PacktPublishing/Time-Series-Analysis-with-Python-Cookbook/main/datasets/Ch10/life_expectancy_birth.csv'

* url2='https://raw.githubusercontent.com/PacktPublishing/Time-Series-Analysis-with-Python-Cookbook/main/datasets/Ch10/milk_production.csv'

Agora, como chamar os dados? Podemos aplicar a seguinte função em linguagem **Python**:

  1. **pd.read_csv(url1, index_col='year',parse_dates=True,skipfooter=1)**

  2. **pd.read_csv(url2,index_col='month',parse_dates=True)**
                   
Podemos praticar um pouco de séries temporais utilizando outros dados já existentes em bibliotecas.

  1. **Dataset de co2**:  
  
    co2_df = co2.load_pandas().data

    co2_df = co2_df.ffill()

  2. **Dataset de AirPassengers**:
  
    air_passengers = get_rdataset("AirPassengers")

    airp_df = air_passengers.data

    airp_df.index = pd.date_range('1949', '1961', freq='M')

    airp_df.drop(columns=['time'], inplace=True)                  

# Decomposição estrutural da série temporal

A decomposição de uma série temporal é o processo de extrair as três componentes e representá-las como seus modelos. A modelagem das componentes decompostas pode ser aditivo ou multiplicativo.

    1. A tendência dá uma noção da direção de longo prazo da série temporal e pode ser ascendente, descendente ou horizontal.
    2. Sazonalidade são padrões repetidos ao longo do tempo. Por exemplo, uma série temporal de dados de vendas pode mostrar um aumento nas vendas na época do Natal.
    3. O residuo é simplesmente a parte restante ou inexplicável, uma vez que extraímos a tendência e a sazonalidade.

# Modelo Aditivo

Você tem um **modelo aditivo** quando a série temporal original pode ser reconstruída adicionando todas as três componentes.

    1. Um modelo de decomposição aditivo é razoável quando as variações sazonais não mudam ao longo do tempo. 
    2. Por outro lado, se a série temporal puder ser reconstruída multiplicando todas as três componentes, você terá um modelo multiplicativo. 
    
Um modelo multiplicativo é adequado quando a variação sazonal flutua ao longo do tempo.    

# Modelo SARIMA (p,d,q) x (P,D,Q)

Quando lidamos com os modelos SARIMA estamos partindo do pressuposto que nossa série temporal apresenta:

  1. Tendência: É necessário aplicar diferenças na série para torna-la estacionária. Ou seja, livre de tendência.
  2. Sazonalidade: Existe um efeito sazonal da série que pode ser aditivo ou multiplicativo.
  
Os efeitos de tendência e sazonalidade podem ser verificados a partir de análise gráfica e testes de hipóteses.

  1. Análise gráfica: Podemos aplicar uma decomposição estrutural da série temporal.
  2. Testes de hipoteses: Dickey-Fuller (tendência) e Kruskal Wallis (sazonalidade)
  
 Em python existe duas funções **ndiffs** e **sndiffs**. Elas estimam a quantidade de diferenças necessárias para remover os efeitos de tenência e sazonalidade.
  
 # Teste de estacionariedade
 
 Testes estatísticos utilizados e suas hipóteses nulas:
 
    1.  ADF: a hipótese nula afirma que existe uma raiz unitária na série temporal e, portanto, é não estacionária. 
    2.  KPSS: tem a hipótese nula oposta, que assume que a série temporal é estacionária.  
  
# Tornando uma série estacionária

Existem várias possibilidades para tornar uma série livre de tendência. Entre elas podemos citar:

     1. **Diferenciação de primeira ordem**: é calculada subtraindo uma observação no tempo t da observação anterior no tempo t-1
     2. **Diferenciação de segunda ordem**: Isso é útil se houver sazonalidade ou se a diferenciação de primeira ordem for insuficiente. Isso é basicamente diferenciar duas vezes - diferenciar para remover a sazonalidade seguida de diferenciar para remover a tendência.
     3. **Subtraindo a média móvel** (janela contínua) da série temporal 
     4. **A transformação de log**: usando np.log() é uma técnica comum para estabilizar a variação em uma série temporal e, às vezes, suficiente para tornar a série temporal estacionária. Simplesmente, tudo o que ele faz é substituir cada observação por seu valor logarítmico
     5. **Usando a decomposição da série temporal** para remover o componente de tendência, como sazonal_decompose. 
     6. **Usando o filtro Hodrick-Prescott** para remover o componente de tendência, por exemplo, usando hp_filter

# ACF e PACF 
  
Após verificar as componentes da série temporal podemos nos preocupar em estimar seus parâmetros. Os parâmetros auto regressivos AR(p) e médias móveis MA(q) podem
ser identificados a partir de  análises gráficas. Para tanto podemos utilizar os gráficos de ACF e PACF. Para interpretar estes gráficos podemos utilizar a Figura a seguir.
![(1-3) Curriculo_saul_dnc.jpg](https://github.com/StunKnife/Time_Series/blob/main/guia_PACF_ACF.png)

Os gráficos ACF e PACF ajudarão você a estimar os valores p e q para os modelos AR e MA, respectivamente. Use plot_acf e plot_pacf nos dados estacionários. Ou seja, nos dados livres de tendência.

    1. PACF para estimar a ordem AR 
    2. ACF para estimar a ordem MA
    3. Você precisará diferenciar a série temporal para torná-la estacionária antes de aplicar os gráficos ACF e PACF.

Tradução da Figura acima:

    1. AR(p)
        - ACF: depois do lag p cai gradualmente. Pode ser oscilatório
        - PACF: corte no lag p

    2. MA(q)
        - ACF: corte no lag q
        - PACF: depois do lag q cai gradualmente. Pode ser oscilatório

    3. ARMA(p,q): 
        - ACF: depois do lag p cai gradualmente. Pode ser oscilatório
        - PACF: depois do lag q cai gradualmente. Pode ser oscilatório

  
# Validação dos resíduos
  
Agora, você precisará validar os resíduos do modelo para determinar se o modelo ARIMA que você construiu capturou os sinais na série temporal. A suposição é que, se o modelo capturou todas as informações, os resíduos da previsão do modelo são aleatórios (ruído) e não seguem um padrão. 

O teste de **ljung-box** nos resíduos permiti verificar autocorrelações.

  1. **H0: resíduos não autocorrelacionados**    versus    **H1: resíduos autocorrelacionados**
  2. Um p-valor menor do que 5% de significância fornce evidências suficientes para a rejeição da hipótese nula. Ou seja, existe autocorrelação e o modelo não esta capturando todas as informações da série. Logo existe espaço para melhora do modelo.

Você também pode inspecionar a **distribuição dos resíduos**. Por exemplo, você esperaria resíduos **normalmente distribuídos** com média zero. Você pode usar o gráfico QQPlot e Kernel Density Estimation (KDE) para observar a distribuição e avaliar normalidade. 

# Suavização Exponencial

    1. Simple Exp Smoothing: A suavização exponencial simples é usada quando o processo de série temporal carece de sazonalidade e tendência. Isso também é conhecido como suavização exponencial única.
    2. Holt: a suavização exponencial de Holt é um aprimoramento da suavização exponencial simples, usada quando o processo de série temporal contém apenas tendência (mas sem sazonalidade). É referido como suavização exponencial dupla.
    3. Suavização exponencial: a suavização exponencial de Holt-Winters é um aprimoramento da suavização exponencial de Holt, usada quando o processo de série temporal tem sazonalidade e tendência. É referido como suavização exponencial tripla.
    
# Aplicando power transformations

Dependendo do modelo e da análise que você está buscando, pode ser necessário testar suposições adicionais em relação ao conjunto de dados observado ou aos resíduos do modelo.

    1. Mais especificamente, é a variância dos resíduos. Quando a variância não é constante, mudando ao longo do tempo, chamamos de heteroscedasticidade.
    2. Outra suposição que você precisará testar é a normalidade; a observação específica vem de um distribuição normal? 
    3. As vezes, você também pode querer verificar a normalidade dos resíduos, o que pode fazer parte do estágio de diagnóstico do modelo.
    4. A transformação Box-Cox pode ser usada para transformar os dados e satisfazer a normalidade e a homoscedasticidade.

**OBS**: Se você não fizer isso (checar), poderá acabar com um modelo falho ou um resultado excessivamente otimista ou excessivamente pessimista. 
    
# Testando normalidade

Convenientemente, os testes a seguir têm a mesma hipótese nula.  A hipótese nula afirma que os dados são normalmente distribuídos
        
        1. Teste de Shapiro-Wilks
        2. teste de Kolmogorov-Smirnov

# Testando Homocedasticidade dos resíduos

Você testará a estabilidade da variância em relação aos resíduos do modelo. 
  - Teste de Breusch–Pagan: A hipótese nula afirma que os dados são homocedásticos.
