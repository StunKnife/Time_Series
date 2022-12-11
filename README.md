# Séries Temporais

- Neste repositório é armazenda algumas **funções** importantes para aplicação de **séries temporais**
  1. ARIMA, SARIMA e alisamento exponencial
  2. Teste de séries estacionárias
  3. Decomposição da série temporal em suas componentes: tendência e sazonalidade
  4. Análise de autocorrelação
  5. Análise de diagnóstico
---
# Dataset

Para treinar séries temporais é possível utilizar dois datasets disponibilizados nos seguintes repositórios:

url1='https://raw.githubusercontent.com/PacktPublishing/Time-Series-Analysis-with-Python-Cookbook/main/datasets/Ch10/life_expectancy_birth.csv'

url2='https://raw.githubusercontent.com/PacktPublishing/Time-Series-Analysis-with-Python-Cookbook/main/datasets/Ch10/milk_production.csv'

 - Como chamar os dados? Podemos aplicar a seguinte função:

  life = pd.read_csv(url1, index_col='year',parse_dates=True,skipfooter=1)

  milk = pd.read_csv(url2,index_col='month',parse_dates=True)
                   
- Podemos praticar utilizando dados existentes em algumas bibliotecas.

  1. **Dataset de co2**:  
  
  co2_df = co2.load_pandas().data
  co2_df = co2_df.ffill()

  2. **Dataset de AirPassengers**:
  
  air_passengers = get_rdataset("AirPassengers")
  airp_df = air_passengers.data
  
  airp_df.index = pd.date_range('1949', '1961', freq='M')
  
  airp_df.drop(columns=['time'], inplace=True)                  

# Decomposição

A decomposição de uma série temporal é o processo de extrair as três componentes e representá-las como seus modelos. A modelagem das componentes decompostos pode ser aditivo ou multiplicativo.

    1. A tendência dá uma noção da direção de longo prazo da série temporal e pode ser ascendente, descendente ou horizontal.
    2. Sazonalidade são padrões repetidos ao longo do tempo. Por exemplo, uma série temporal de dados de vendas pode mostrar um aumento nas vendas na época do Natal.
    3. O residuo é simplesmente a parte restante ou inexplicável, uma vez que extraímos a tendência e a sazonalidade.

Você tem um **modelo aditivo** quando a série temporal original pode ser reconstruída adicionando todos os três componentes.
    1. Um modelo de decomposição aditivo é razoável quando as variações sazonais não mudam ao longo do tempo. 
    2. Por outro lado, se a série temporal puder ser reconstruída multiplicando todos os três componentes, você terá um modelo multiplicativo. Um modelo multiplicativo é adequado quando a variação sazonal flutua ao longo do tempo    

# SARIMA (p,d,q) x (P,D,Q)

Quando lidamos com os modelos SARIMA estamos partindo do pressuposto que nossa série temporal apresenta
  1. Tendência: É necessário aplicar diferenças na série para torna-la estacionária. Ou seja, livre de tendência
  2. Sazonalidade: Existe um efeito sazonal da série que pode ser aditivo ou multiplicativo
  
Os efeitos de tendência e sazonalidade podem ser verificados a partir de análise gráfica e testes de hipóteses.
  1. Análise gráfica: Podemos aplicar uma decomposição estrutural da série temporal.
  2. Os testes de hipoteses: Dickey-Fuller (tendência) e Kruskal Wallis (sazonalidade)
  
  Em python existe duas funções **ndiffs** e **sndiffs**. Elas estimam a quantidade de diferenças necessárias para remover os efeitos de tenência e sazonalidade.
  
  # ACF e PACF 
  
  Após verificar as componentes da série temporal podemos nos preocupar em estimar seus parâmetros. Os parâmetros auto regressivos AR(p) e médias móveis MA(q) podem
  ser identificados a partir de  análises gráficas. Para tanto podemos utilizar os gráficos de ACF e PACF. Para interpretar estes gráficos podemos utilizar a Figura a seguir.
![(1-3) Curriculo_saul_dnc.jpg](https://github.com/StunKnife/Time_Series/blob/main/guia_PACF_ACF.png)

 * Os gráficos ACF e PACF ajudarão você a estimar os valores p e q para os modelos AR e MA, respectivamente. Use plot_acf e plot_pacf nos dados estacionários. Ou seja, nos dados livres de tendência.

    1. PACF para estimar a ordem AR 
    2. ACF para estimar a ordem MA
    3. Você precisará diferenciar a série temporal para torná-la estacionária antes de aplicar os gráficos ACF e PACF.

# Guia de exemplo para identificar as ordens AR e MA estacionárias dos gráficos PACF e ACF
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
  
Agora, você precisará validar os resíduos do modelo para determinar se o modelo ARIMA que você construiu capturou os sinais na série temporal.
  - A suposição é que, se o modelo capturou todas as informações, os resíduos da previsão do modelo são aleatórios (ruído) e não seguem um padrão. 

O teste de **ljung-box** nos resíduos permiti verificar autocorrelações.
  1.H0: resíduos não autocorrelacionados    versus    H1: resíduos autocorrelacionados
  2. Um p-valor menor do que 5% de significância fornce evidências suficientes para a rejeição da hipótese nula. Ou seja, existe autocorrelação e o modelo não esta capturando todas as informações da série. Logo existe espaço para melhora do modelo.

- Você também pode inspecionar a **distribuição dos resíduos**. Por exemplo, você esperaria resíduos **normalmente distribuídos** com média zero. Você pode usar o gráfico QQPlot e Kernel Density Estimation (KDE) para observar a distribuição e avaliar normalidade. 

