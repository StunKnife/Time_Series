# Séries Temporais

- Neste repositório é armazenda algumas **funções** importantes para aplicação de **séries temporais**
  1. ARIMA, SARIMA e alisamento exponencial
  2. Teste de séries estacionárias
  3. Decomposição da série temporal em suas componentes: tendência e sazonalidade
  4. Análise de autocorrelação
  5. Análise de diagnóstico
---
# SARIMA (p,d,q) x (P,D,Q)

Quando lidamos com os modelos SARIMA estamos partindo do pressuposto que nossa série temporal apresenta
  1. Tendência: É necessário aplicar diferenças na série para torna-la estacionária. Ou seja, livre de tendência
  2. Sazonalidade: Existe um efeito sazonal da série que pode ser aditivo ou multiplicativo
  
Os efeitos de tendência e sazonalidade podem ser verificados a partir de análise gráfica e testes de hipóteses.
  1. Análise gráfica: Podemos aplicar uma decomposição estrutural da série temporal.
  2. Os testes de hipoteses: Dickey-Fuller (tendência) e Kruskal Wallis (sazonalidade)
  
  Em python existe duas funções **ndiffs** e **sndiffs**. Elas estimam a quantidade de diferenças necessárias para remover os efeitos de tenência e sazonalidade.
  
  # ETAPA DE AJUSTE
  
  Após verificar as componentes da série temporal podemos nos preocupar em estimar seus parâmetros. Os parâmetros auto regressivos AR(p) e médias móveis MA(q) podem
  ser identificados a partir de  análises gráficas. Para tanto podemos utilizar os gráficos de ACF e PACF. Para interpretar estes gráficos podemos utilizar a Figura a seguir.
![(1-3) Curriculo_saul_dnc.jpg](https://github.com/StunKnife/Time_Series/blob/main/guia_PACF_ACF.png)

  
  
