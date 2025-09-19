# MANET-UAV Two-Ray Channel (OMNeT++ / INET)

Este repositório contém o código e os arquivos de simulação (C/C++ e configuração) usados no OMNeT++ (Eclipse) para avaliar a propagação entre **drones (UAVs)** e **sensores em solo** em uma rede ad-hoc. Partimos de **módulos existentes do INET** (mobilidade, PHY, rádio, antenas) e os combinamos/estendemos para usar **modelo de dois raios** como base — por ter sido o que **melhor aderiu à realidade experimental** — e **aperfeiçoamos** a simulação com um fator de **espalhamento/rugeza de superfície** (atenuando a reflexão de Fresnel), além de um **modelo de antena dipolo** com ganho angular.

## Visão Geral

- **Objetivo:** modelar o **enlace físico** ar-terra (UAV ↔ solo) considerando **múltiplos percursos** (linha de visada + reflexão no solo) e comparar com **medições de campo**.
- **Por que dois raios?** O cenário com antenas em alturas diferentes mostrou aderência ao **Modelo de Dois Raios**, que explica bem as interferências construtivas/destrutivas observadas e o decaimento com a distância. 
- **Aperfeiçoamento:** adicionamos **espalhamento por rugosidade** (coeficiente ρₛ aplicado ao termo refletido de Fresnel) para aproximar a simulação das condições reais de solo/grama; também utilizamos **dipolo** como antena de referência (padrão de radiação analítico). 
- **Ferramentas:** OMNeT++ + INET (módulos existentes) e código C/C++ de canal/antena; MATLAB foi usado para validação dos gráficos de potência com dados **experimentais** (CIAA) obtidos com **PlutoSDR** e antenas **whip**. 

## Principais Componentes do Modelo

- **Canal de Rádio (Two-Ray + Rugosidade):**
  - Raio direto e refletido com **fase** em função da diferença de percurso;
  - **Reflexão de Fresnel** com correção por **rugosidade do terreno** (ρₛ = exp(−½·Δϕᵣ));
  - Parâmetros de solo (permissividade efetiva) e **polarização** (vertical/horizontal).
- **Antenas:**
  - **Dipolo** com ganho angular (maior irradiação no plano perpendicular, nulo axial).
- **Mobilidade:**
  - UAV com trajetória linear e velocidade constante; receptor no solo. 
- **Medições de Campo (para validação):**
  - Ensaios no **CIAA** com drone ~5 m de altura, varrendo distâncias em 2,4 GHz; registro de potência e comparação com simulação. 

## Resultados (resumo)

- As curvas **simuladas** e **medidas** mostraram **comportamento semelhante**, especialmente em **polarização horizontal** (picos/vales condizentes com interferência de dois raios). 
- Em **polarização vertical**, ocorreram discrepâncias de potência (valores medidos maiores), atribuídas a **reflexões adicionais** de objetos/veículos e limitações do cenário 3D simplificado no simulador — reforçando a necessidade do termo de **espalhamento**. 
- O modelo com **dois raios** serviu como base robusta e os ajustes de **rugosidade** aproximaram a simulação da **realidade experimental**. 

