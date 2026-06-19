RELATÓRIO TÉCNICO DA PIPELINE DE PLOTAGENS DA DISSERTAÇÃO
Qualificação I — PPGOceano/UFSC
1. OBJETIVO DO RELATÓRIO
Este relatório consolida o estado atual das rotinas em Python desenvolvidas para a geração de produtos gráficos, tabelares e arquivos de controle da dissertação no âmbito da Qualificação I do PPGOceano/UFSC.
O objetivo atual da etapa de plotagens é preparar uma apresentação voltada à comparação entre duas comparações numéricas:
CTRL_NOV2022_dis × CLIMSST_NOV2022_dis
CTRL_NOV2022_dis × SSTN2008_NOV2022_dis
A proposta central é avaliar como a precipitação simulada pelo WRF em NOV/2022 responde a duas alterações distintas da condição oceânica de superfície:
TSM climatológica;
TSM real de novembro de 2008.
Assim, a apresentação não deve focar apenas em uma comparação isolada entre CTRL e uma rodada de sensibilidade. O foco principal será comparar os impactos produzidos por CLIMSST e SSTN2008 em relação ao CTRL.
2. ORGANIZAÇÃO GERAL DA PIPELINE
2.1. Separação entre dissertação e artigo
A estrutura atual da pipeline foi corrigida para separar a lógica da dissertação da lógica associada ao artigo.
A dissertação utiliza como base:
04_DISSERTACAO/03_RODADAS
04_DISSERTACAO/04_DADOS
04_DISSERTACAO/06_OUTPUTS
A estrutura do artigo, baseada em quatro testes de sensibilidade, não foi adotada como referência da pipeline atual. A simulação QE0 também não integra a configuração vigente da dissertação.
2.2. Diretórios principais
A pasta principal das rodadas WRF da dissertação é:
04_DISSERTACAO/03_RODADAS
A pasta principal dos dados observacionais, climatológicos e de reanálise é:
04_DISSERTACAO/04_DADOS
A pasta principal dos produtos finais é:
04_DISSERTACAO/06_OUTPUTS
2.3. Organização dos outputs por escala
A estrutura de saída foi reduzida ao mínimo funcional, organizada por escala:
01_CLIMA
02_SINOTICA
03_MESO
A organização atual evita pastas intermediárias desnecessárias e mantém apenas produtos úteis para análise, documentação e apresentação.
Foram removidas da lógica da pipeline as pastas ou estruturas associadas a:
LATEX;
04_QUALIFICACAO1;
subpastas com nome de versão do notebook, como plotagens_v3;
organizações herdadas de testes anteriores que não representam a estrutura final da dissertação.
2.4. Organização dos produtos WRF por domínio
A organização dos produtos WRF passou a considerar a função física de cada domínio:
d01 → escala sinótica;
d02 → escala mesoescala;
d03 → escala mesoescala.
Assim, os produtos derivados de d01 são salvos em:
06_OUTPUTS/02_SINOTICA/WRF
Os produtos derivados de d02 e d03 são salvos em:
06_OUTPUTS/03_MESO/WRF
Essa separação corrige a interpretação anterior de que todo produto WRF seria necessariamente mesoescala. O domínio d01 é mantido como diagnóstico sinótico, pois representa a circulação de maior escala utilizada para contextualizar o evento.
3. RODADAS CONSIDERADAS
3.1. Rodadas principais
As rodadas atualmente consideradas na dissertação são:
CTRL_NOV2008_dis
CTRL_NOV2022_dis
CLIMSST_NOV2022_dis
SSTN2008_NOV2022_dis
3.2. Rodada controle de NOV/2022
A rodada CTRL_NOV2022_dis representa a simulação de referência do evento de NOV/2022.
Essa rodada mantém a situação sinótica de 2022 e a condição oceânica correspondente ao próprio evento.
3.3. Rodada CLIMSST_NOV2022_dis
A rodada CLIMSST_NOV2022_dis representa a simulação de NOV/2022 com substituição da TSM original por uma TSM climatológica.
A finalidade dessa rodada é avaliar a resposta atmosférica quando o sinal térmico oceânico específico do evento é removido ou suavizado.
Definição operacional:
CLIMSST_NOV2022_dis = sinótica de 2022 + TSM climatológica
3.4. Rodada SSTN2008_NOV2022_dis
A rodada SSTN2008_NOV2022_dis representa a simulação de NOV/2022 com a situação sinótica de 2022 e a TSM real de novembro de 2008.
A finalidade dessa rodada é avaliar a resposta atmosférica quando a condição oceânica de 2022 é substituída por uma configuração realista de outro ano, preservando estruturas espaciais de TSM mais físicas do que uma climatologia suavizada.
Definição operacional:
SSTN2008_NOV2022_dis = sinótica de 2022 + TSM real de novembro de 2008
4. NOTEBOOKS DA PIPELINE
4.1. Notebook principal v3
O notebook principal é:
02_PLOTAGENS_DIS_v3.ipynb
Essa versão organiza os produtos gerais da Qualificação I, incluindo:
MHW;
ASAS;
diagnósticos WRF da rodada controle;
diagnósticos de precipitação;
diagnósticos de aporte de umidade.
4.2. Notebook v3-1 CLIMSST
O notebook complementar para a comparação com TSM climatológica é:
02_PLOTAGENS_DIS_v3-1_CLIMSST.ipynb
Esse notebook compara:
CTRL_NOV2022_dis × CLIMSST_NOV2022_dis
A saída é organizada em:
06_OUTPUTS/02_SINOTICA/WRF/CLIMSST/d01
06_OUTPUTS/03_MESO/WRF/CLIMSST/d02
06_OUTPUTS/03_MESO/WRF/CLIMSST/d03
4.3. Notebook v3-2 SSTN2008
O notebook complementar para a comparação com TSM de novembro de 2008 é:
02_PLOTAGENS_DIS_v3-2_SSTN2008_NOV2022.ipynb
Esse notebook compara:
CTRL_NOV2022_dis × SSTN2008_NOV2022_dis
A saída é organizada em:
06_OUTPUTS/02_SINOTICA/WRF/SSTN2008/d01
06_OUTPUTS/03_MESO/WRF/SSTN2008/d02
06_OUTPUTS/03_MESO/WRF/SSTN2008/d03
5. PRODUTOS DA PIPELINE V3
5.1. Bloco climático: MHW
O bloco climático utiliza dados de OISST para diagnosticar ondas de calor marinhas no domínio de referência.
A análise considera climatologia e percentil 90 para identificação de aquecimento oceânico relevante.
Foram avaliados os eventos de NOV/2008 e NOV/2022.
A análise indicou:
menor expressão espacial de MHW em NOV/2008;
sinal mais amplo de aquecimento oceânico em NOV/2022.
As figuras de MHW foram revisadas com base no padrão visual das versões anteriores da pipeline, preservando títulos científicos reutilizáveis e removendo marcações administrativas.
5.2. Bloco sinótico: ASAS
O bloco sinótico utiliza ERA5 em domínio amplo do Atlântico Sul para caracterizar a Alta Subtropical do Atlântico Sul.
Os produtos principais incluem:
campo médio de pressão ao nível médio do mar;
vento médio a 10 m;
diagnóstico diário do máximo de pressão;
trajetória diária do centro de máxima pressão;
caixa de referência oceânica para análise da ASAS.
A lógica de movimentação da ASAS foi recuperada da versão v1, por permitir avaliar não apenas o campo médio, mas também a variação diária da posição do centro de alta pressão.
5.3. Bloco WRF controle
O bloco WRF da v3 foi redirecionado para a pasta:
04_DISSERTACAO/03_RODADAS
Foram geradas figuras de precipitação acumulada e vento médio a 10 m, com inspeção visual direta no Colab.
Também foi preparada a célula de IVT para os domínios d01, d02 e d03.
A formulação utilizada para o transporte integrado de vapor é:
IVT_x = (1/g) ∫ q u dp
IVT_y = (1/g) ∫ q v dp
|IVT| = sqrt(IVT_x² + IVT_y²)
As variáveis WRF utilizadas são:
QVAPOR;
U;
V;
P;
PB.
Os ventos U e V são desestaggerados antes da integração vertical.
6. PIPELINE V3-1: CTRL × CLIMSST
6.1. Finalidade
A versão 02_PLOTAGENS_DIS_v3-1_CLIMSST.ipynb foi reorganizada para comparar a rodada controle de NOV/2022 com a rodada CLIMSST_NOV2022_dis.
A comparação avalia o impacto da substituição da TSM original de 2022 por uma TSM climatológica.
6.2. Estrutura da v3-1
A pipeline foi reduzida a células operacionais principais:
Célula 0 — limpeza dos produtos CLIMSST anteriores;
Célula 1 — configuração, validação das rodadas e estrutura de saída;
Célula 2 — inventário leve e pareamento temporal CTRL × CLIMSST;
Célula 3 — comparação de TSM média CTRL × CLIMSST;
Célula 4 — comparação de precipitação acumulada CTRL × CLIMSST;
Células posteriores — comparação integrada e produtos finais para apresentação.
6.3. Inventário e pareamento temporal
O inventário dos arquivos wrfout foi feito de modo leve, evitando leitura massiva de arquivos NetCDF.
A variável interna Times foi usada apenas como verificação amostral, no primeiro e último arquivo de cada rodada/domínio.
A v3-1 identificou:
219 pares temporais tentados;
215 pares temporais válidos.
As falhas ocorreram em horários pontuais, especialmente no d01, devido à diferença no horário final disponível da rodada CLIMSST.
Por esse motivo, os produtos finais usam apenas pares temporais válidos.
6.4. Comparação de TSM na v3-1
A diferença média CLIMSST − CTRL indicou resfriamento médio em todos os domínios:
d01: aproximadamente −0,68 °C;
d02: aproximadamente −0,16 °C;
d03: aproximadamente −0,08 °C.
Esse resultado confirma que a rodada CLIMSST foi construída com campo oceânico diferente da rodada controle.
6.5. Comparação de precipitação acumulada na v3-1
A precipitação acumulada foi calculada por:
RAINNC + RAINC + RAINSH
O acumulado foi definido como:
precipitação acumulada = campo final − campo inicial
6.5.1. Média geral
Tabela 1 — Precipitação média geral na comparação CTRL × CLIMSST
Domínio
CTRL (mm)
CLIMSST (mm)
CLIMSST − CTRL (mm)
d01
13,4384
13,2875
−0,1508
d02
17,3948
16,7714
−0,6234
d03
34,3164
33,6043
−0,7121
6.5.2. Média sobre terra
Tabela 2 — Precipitação média sobre terra na comparação CTRL × CLIMSST
Domínio
CTRL (mm)
CLIMSST (mm)
CLIMSST − CTRL (mm)
d01
21,2323
20,6789
−0,5533
d02
22,2302
21,1077
−1,1226
d03
41,2614
39,3480
−1,9133
6.5.3. Média sobre oceano
Tabela 3 — Precipitação média sobre oceano na comparação CTRL × CLIMSST
Domínio
CTRL (mm)
CLIMSST (mm)
CLIMSST − CTRL (mm)
d01
11,0700
11,0414
−0,0285
d02
12,1956
12,1090
−0,0866
d03
27,6312
28,0755
+0,4442
6.6. Síntese da v3-1
A rodada CLIMSST reduziu a precipitação média em relação ao CTRL nos três domínios.
A redução foi mais evidente sobre terra, principalmente no domínio d03.
No oceano, a resposta foi mais fraca e, no d03, houve pequeno aumento médio em CLIMSST em relação ao CTRL.
7. PIPELINE V3-2: CTRL × SSTN2008
7.1. Finalidade
A versão 02_PLOTAGENS_DIS_v3-2_SSTN2008_NOV2022.ipynb foi criada para comparar a rodada controle de NOV/2022 com a rodada SSTN2008_NOV2022_dis.
A comparação avalia o impacto da substituição da TSM original de 2022 pela TSM real de novembro de 2008.
7.2. Estrutura da v3-2
A v3-2 segue a mesma lógica da v3-1:
validação das rodadas;
inventário leve dos wrfout;
pareamento temporal;
processamento com baixo consumo de memória;
salvamento de produtos intermediários;
reaproveitamento de cache;
figuras em PNG;
saídas organizadas por domínio e escala.
7.3. Otimização para Colab
A v3-2 foi ajustada para evitar estouro de RAM no Colab gratuito.
A estratégia adotada prioriza:
abrir um arquivo por vez;
evitar carregamento simultâneo de muitos NetCDF;
processar apenas pares temporais necessários;
salvar produtos intermediários em NetCDF e CSV;
reaproveitar produtos já existentes quando válidos.
7.4. Comparação de precipitação acumulada na v3-2
7.4.1. Média geral
Tabela 4 — Precipitação média geral na comparação CTRL × SSTN2008
Domínio
CTRL (mm)
SSTN2008 (mm)
SSTN2008 − CTRL (mm)
d01
13,6406
13,0205
−0,6201
d02
17,3948
15,2416
−2,1532
d03
34,3164
31,2516
−3,0648
7.4.2. Média sobre terra
Tabela 5 — Precipitação média sobre terra na comparação CTRL × SSTN2008
Domínio
CTRL (mm)
SSTN2008 (mm)
SSTN2008 − CTRL (mm)
d01
21,8554
21,2713
−0,5841
d02
22,2302
19,3196
−2,9106
d03
41,2614
39,0138
−2,2476
7.5. Síntese da v3-2
A rodada SSTN2008 reduziu a precipitação acumulada em relação ao CTRL nos três domínios.
A redução foi mais evidente em d02 e d03.
O sinal médio indica que a substituição pela TSM real de novembro de 2008 produziu impacto mais forte na precipitação do que a substituição por TSM climatológica.
8. COMPARAÇÃO ENTRE AS COMPARAÇÕES
8.1. Pergunta central da apresentação
A pergunta central da apresentação é:
Qual alteração oceânica produziu maior impacto na precipitação simulada em NOV/2022?
A comparação deve ser feita entre:
CLIMSST − CTRL;
SSTN2008 − CTRL.
8.2. Comparação pela média geral
Tabela 6 — Comparação dos impactos médios gerais
Domínio
CLIMSST − CTRL (mm)
SSTN2008 − CTRL (mm)
Maior redução
d01
−0,1508
−0,6201
SSTN2008
d02
−0,6234
−2,1532
SSTN2008
d03
−0,7121
−3,0648
SSTN2008
A redução média geral foi maior na comparação CTRL × SSTN2008 em todos os domínios.
A diferença é mais expressiva em d02 e d03.
8.3. Comparação pela média sobre terra
Tabela 7 — Comparação dos impactos médios sobre terra
Domínio
CLIMSST − CTRL (mm)
SSTN2008 − CTRL (mm)
Maior redução
d01
−0,5533
−0,5841
SSTN2008
d02
−1,1226
−2,9106
SSTN2008
d03
−1,9133
−2,2476
SSTN2008
Sobre terra, a rodada SSTN2008 também produziu maior redução média da precipitação em relação ao CTRL.
A maior diferença ocorreu no d02, onde SSTN2008 reduziu a precipitação média em aproximadamente −2,91 mm, enquanto CLIMSST reduziu em aproximadamente −1,12 mm.
8.4. Interpretação comparativa
As duas sensibilidades oceânicas reduziram a precipitação média em relação ao CTRL.
A redução foi mais fraca em CLIMSST e mais intensa em SSTN2008.
Esse resultado sugere que a resposta atmosférica não depende apenas da remoção do aquecimento oceânico de 2022. A estrutura espacial da TSM alternativa também parece influenciar a precipitação simulada.
A TSM climatológica suaviza o campo oceânico. A TSM real de novembro de 2008 preserva padrões espaciais mais realistas. A resposta mais intensa em SSTN2008 sugere que a organização espacial da TSM pode ter papel relevante na modulação da precipitação costeira.
9. CUIDADOS DE INTERPRETAÇÃO
9.1. Diferença temporal no d01 da CLIMSST
O domínio d01 da comparação CLIMSST requer cautela adicional.
Na v3-1, o d01 utilizou 71 pares temporais válidos, enquanto d02 e d03 utilizaram 72 pares.
Essa diferença ocorreu porque alguns horários da rodada CLIMSST não tiveram par temporal equivalente dentro da tolerância adotada.
Por esse motivo, o d01 deve ser usado principalmente como diagnóstico sinótico, com menor peso na comparação quantitativa direta.
9.2. Comparação mais robusta
A comparação quantitativa mais robusta deve priorizar d02 e d03, pois nesses domínios os valores de CTRL são diretamente equivalentes entre as duas pipelines.
Os domínios d02 e d03 são também os mais importantes para avaliar a resposta costeira e regional da precipitação simulada.
10. CLIMATOLOGIA GPM E ANOMALIAS
10.1. Pasta de climatologia GPM
Foi criada a pasta:
04_DADOS/01_CLIMATOLOGIA/02_CLIMPREC_GPM
Essa pasta armazena a climatologia de precipitação GPM/IMERG usada como referência para NOV/2022.
10.2. Definição da climatologia
A climatologia foi processada para os dias associados à janela do evento:
30/11;
01/12;
02/12;
03/12.
O objetivo é formar um acumulado climatológico compatível com a janela de precipitação do evento de NOV/2022.
10.3. Anomalias previstas
Para os dados observacionais GPM, a definição é:
anomalia GPM = precipitação acumulada do evento GPM − climatologia acumulada GPM
Para as simulações WRF, as definições previstas são:
anomalia CTRL = precipitação CTRL − climatologia GPM
anomalia CLIMSST = precipitação CLIMSST − climatologia GPM
anomalia SSTN2008 = precipitação SSTN2008 − climatologia GPM
10.4. Função das anomalias na apresentação
A comparação direta entre simulações mostra o impacto de cada sensibilidade em relação ao CTRL.
A comparação com a climatologia GPM permite avaliar se cada simulação se aproxima ou se afasta de um padrão climatológico de precipitação.
Para a apresentação, os dois tipos de diagnóstico são complementares:
CLIMSST − CTRL e SSTN2008 − CTRL indicam o impacto da alteração da TSM;
CTRL − CLIM GPM, CLIMSST − CLIM GPM e SSTN2008 − CLIM GPM indicam o comportamento das simulações em relação à climatologia observacional.
11. DIAGNÓSTICO DE IVT
11.1. Finalidade do diagnóstico
Além da precipitação acumulada, foi incluído o diagnóstico de transporte integrado de vapor d’água, ou IVT.
O objetivo do IVT é avaliar se as alterações na TSM modificam o aporte horizontal de umidade associado ao evento de NOV/2022. Esse diagnóstico é importante porque a precipitação costeira não depende apenas da instabilidade local ou da TSM, mas também da disponibilidade e do transporte de vapor d’água em direção ao continente.
A formulação utilizada foi:
IVT_x = (1/g) ∫ q u dp
IVT_y = (1/g) ∫ q v dp
|IVT| = sqrt(IVT_x² + IVT_y²)
As variáveis WRF utilizadas foram:
QVAPOR;
U;
V;
P;
PB.
Os ventos U e V foram desestaggerados antes da integração vertical. A integração foi feita domínio a domínio, com leitura sequencial dos arquivos wrfout, evitando abertura simultânea de grandes conjuntos NetCDF.
11.2. IVT na comparação CTRL × CLIMSST
A célula de IVT da v3-1 processou os três domínios da comparação CTRL_NOV2022_dis × CLIMSST_NOV2022_dis.
Foram processados:
d01: 71 pares temporais válidos;
d02: 72 pares temporais válidos;
d03: 72 pares temporais válidos.
A comparação foi calculada como:
CLIMSST − CTRL
11.2.1. IVT médio geral
Tabela 8 — IVT médio geral na comparação CTRL × CLIMSST
Domínio
CTRL (kg m⁻¹ s⁻¹)
CLIMSST (kg m⁻¹ s⁻¹)
CLIMSST − CTRL (kg m⁻¹ s⁻¹)
d01
309,1926
308,9098
−0,2829
d02
394,9290
394,6998
−0,2293
d03
391,7550
388,7515
−3,0035
11.2.2. IVT médio sobre terra
Tabela 9 — IVT médio sobre terra na comparação CTRL × CLIMSST
Domínio
CTRL (kg m⁻¹ s⁻¹)
CLIMSST (kg m⁻¹ s⁻¹)
CLIMSST − CTRL (kg m⁻¹ s⁻¹)
d01
162,5346
161,2578
−1,2768
d02
192,8736
190,4550
−2,4186
d03
221,2424
217,5527
−3,6896
11.2.3. IVT médio sobre oceano
Tabela 10 — IVT médio sobre oceano na comparação CTRL × CLIMSST
Domínio
CTRL (kg m⁻¹ s⁻¹)
CLIMSST (kg m⁻¹ s⁻¹)
CLIMSST − CTRL (kg m⁻¹ s⁻¹)
d01
353,7790
353,7982
+0,0193
d02
612,1843
614,3090
+2,1247
d03
555,8903
553,5474
−2,3430
11.2.4. Síntese do IVT na v3-1
A comparação CTRL × CLIMSST indicou mudanças pequenas no IVT médio geral em d01 e d02, e redução mais clara em d03.
Sobre terra, a redução do IVT foi mais consistente nos três domínios. Isso sugere que a TSM climatológica reduziu levemente o aporte médio de umidade integrado verticalmente sobre a porção continental, com maior intensidade no domínio de maior resolução.
Sobre oceano, a resposta foi menos uniforme. Houve pequeno aumento em d01 e d02 e redução em d03. Portanto, a interpretação física deve considerar não apenas o valor médio geral, mas também o contraste entre oceano e continente.
11.3. IVT na comparação CTRL × SSTN2008
A célula de IVT da v3-2 processou os três domínios da comparação CTRL_NOV2022_dis × SSTN2008_NOV2022_dis.
Foram processados:
d01: 73 pares temporais válidos;
d02: 73 pares temporais válidos;
d03: 73 pares temporais válidos.
A comparação foi calculada como:
SSTN2008 − CTRL
11.3.1. IVT médio geral
Tabela 11 — IVT médio geral na comparação CTRL × SSTN2008
Domínio
CTRL (kg m⁻¹ s⁻¹)
SSTN2008 (kg m⁻¹ s⁻¹)
SSTN2008 − CTRL (kg m⁻¹ s⁻¹)
d01
308,4393
305,7785
−2,6608
d02
395,0655
393,9897
−1,0759
d03
393,0336
388,8381
−4,1955
11.3.2. IVT médio sobre terra
Tabela 12 — IVT médio sobre terra na comparação CTRL × SSTN2008
Domínio
CTRL (kg m⁻¹ s⁻¹)
SSTN2008 (kg m⁻¹ s⁻¹)
SSTN2008 − CTRL (kg m⁻¹ s⁻¹)
d01
162,3672
161,4993
−0,8679
d02
192,5817
190,6922
−1,8895
d03
221,4704
218,0577
−3,4127
11.3.3. IVT médio sobre oceano
Tabela 13 — IVT médio sobre oceano na comparação CTRL × SSTN2008
Domínio
CTRL (kg m⁻¹ s⁻¹)
SSTN2008 (kg m⁻¹ s⁻¹)
SSTN2008 − CTRL (kg m⁻¹ s⁻¹)
d01
352,8475
349,6417
−3,2059
d02
612,7812
612,5803
−0,2010
d03
558,1802
553,2313
−4,9489
11.3.4. Síntese do IVT na v3-2
A comparação CTRL × SSTN2008 indicou redução do IVT médio geral nos três domínios.
A redução foi mais evidente em d01 e d03. Em d03, a diminuição média geral foi de aproximadamente −4,20 kg m⁻¹ s⁻¹, indicando enfraquecimento do transporte integrado de vapor na simulação com TSM de novembro de 2008.
Sobre terra, a redução também ocorreu nos três domínios, com maior magnitude em d03. Sobre oceano, a redução foi mais forte em d01 e d03, enquanto em d02 foi pequena.
11.4. Comparação entre os impactos no IVT
A comparação entre CLIMSST − CTRL e SSTN2008 − CTRL mostra que as duas alterações de TSM reduziram o IVT médio geral em relação ao CTRL, mas a redução foi mais intensa na rodada SSTN2008.
Tabela 14 — Comparação dos impactos médios gerais no IVT
Domínio
CLIMSST − CTRL (kg m⁻¹ s⁻¹)
SSTN2008 − CTRL (kg m⁻¹ s⁻¹)
Maior redução
d01
−0,2829
−2,6608
SSTN2008
d02
−0,2293
−1,0759
SSTN2008
d03
−3,0035
−4,1955
SSTN2008
Tabela 15 — Comparação dos impactos médios sobre terra no IVT
Domínio
CLIMSST − CTRL (kg m⁻¹ s⁻¹)
SSTN2008 − CTRL (kg m⁻¹ s⁻¹)
Maior redução
d01
−1,2768
−0,8679
CLIMSST
d02
−2,4186
−1,8895
CLIMSST
d03
−3,6896
−3,4127
CLIMSST
Tabela 16 — Comparação dos impactos médios sobre oceano no IVT
Domínio
CLIMSST − CTRL (kg m⁻¹ s⁻¹)
SSTN2008 − CTRL (kg m⁻¹ s⁻¹)
Maior redução
d01
+0,0193
−3,2059
SSTN2008
d02
+2,1247
−0,2010
SSTN2008
d03
−2,3430
−4,9489
SSTN2008
A leitura geral indica que SSTN2008 produziu maior redução do IVT médio geral e oceânico, enquanto CLIMSST produziu redução ligeiramente maior sobre terra em d01, d02 e d03.
Essa diferença sugere que o efeito da alteração da TSM sobre o transporte de umidade não é espacialmente homogêneo. A resposta sobre oceano e continente pode divergir, e a redistribuição do IVT pode ser tão importante quanto a mudança média no domínio completo.
12. COMPARAÇÃO INTEGRADA ENTRE PRECIPITAÇÃO E IVT
12.1. Relação geral entre precipitação e IVT
As duas sensibilidades oceânicas reduziram a precipitação acumulada em relação ao CTRL.
Na precipitação, a redução foi mais intensa na comparação CTRL × SSTN2008, principalmente em d02 e d03.
No IVT médio geral, a redução também foi mais intensa na comparação CTRL × SSTN2008, novamente com destaque para d03.
Isso indica coerência entre os dois diagnósticos: a rodada SSTN2008 apresentou menor precipitação média e menor transporte integrado médio de vapor d’água em relação ao CTRL.
12.2. Comparação integrada no d01
No d01, a precipitação média geral foi reduzida tanto em CLIMSST quanto em SSTN2008.
A redução de precipitação foi maior em SSTN2008.
No IVT médio geral, a redução também foi maior em SSTN2008.
Como o d01 representa o domínio sinótico, esse resultado sugere que a substituição pela TSM de 2008 alterou a circulação e/ou o transporte de umidade em escala mais ampla de forma mais clara do que a TSM climatológica.
12.3. Comparação integrada no d02
No d02, a precipitação média geral foi reduzida em ambas as sensibilidades.
A redução da precipitação foi muito maior em SSTN2008.
No IVT médio geral, SSTN2008 também apresentou maior redução do que CLIMSST. Entretanto, sobre terra, a redução do IVT foi ligeiramente maior em CLIMSST.
Esse resultado sugere que a precipitação não responde apenas ao valor médio do IVT sobre terra. A posição do fluxo, a convergência, a interação com a topografia e a distribuição espacial do transporte de umidade devem ser avaliadas nas figuras.
12.4. Comparação integrada no d03
No d03, a redução da precipitação foi mais intensa em SSTN2008.
O IVT médio geral também foi mais reduzido em SSTN2008.
Esse domínio é o mais importante para a resposta costeira e regional. A combinação entre menor precipitação e menor IVT médio sugere que a substituição pela TSM de novembro de 2008 reduziu a disponibilidade ou a organização do transporte de umidade no domínio de maior resolução.
12.5. Interpretação física preliminar
Os resultados indicam que a condição oceânica de NOV/2022 contribuiu para sustentar maior precipitação simulada no CTRL.
A substituição por TSM climatológica reduziu a precipitação e produziu mudanças modestas no IVT médio geral.
A substituição por TSM real de novembro de 2008 reduziu a precipitação de forma mais intensa e também reduziu mais claramente o IVT médio geral.
A interpretação preliminar é que a TSM de 2022 favoreceu uma configuração mais eficiente para aporte de umidade e precipitação no evento simulado. A TSM de 2008, além de modificar a temperatura média, preserva uma estrutura espacial oceânica realista diferente, o que pode alterar a organização do transporte de vapor.
13. CUIDADOS DE INTERPRETAÇÃO
13.1. Diferenças no pareamento temporal
As comparações CLIMSST e SSTN2008 não utilizaram exatamente o mesmo número de pares temporais.
Na v3-1 CLIMSST:
d01: 71 pares;
d02: 72 pares;
d03: 72 pares.
Na v3-2 SSTN2008:
d01: 73 pares;
d02: 73 pares;
d03: 73 pares.
Essa diferença deve ser mencionada na apresentação quando forem discutidos valores médios, principalmente no d01.
A comparação qualitativa dos padrões espaciais permanece válida, mas a comparação quantitativa direta deve ser feita com cautela.
13.2. Diferenças nos caminhos de saída
A estrutura real dos produtos não ficou idêntica nas duas pipelines.
Os PNGs do CLIMSST estão diretamente nas pastas dos domínios:
06_OUTPUTS/02_SINOTICA/WRF/CLIMSST/d01
06_OUTPUTS/03_MESO/WRF/CLIMSST/d02
06_OUTPUTS/03_MESO/WRF/CLIMSST/d03
Os PNGs do SSTN2008 estão dentro da subpasta FIGURAS:
06_OUTPUTS/02_SINOTICA/WRF/SSTN2008/d01/FIGURAS
06_OUTPUTS/03_MESO/WRF/SSTN2008/d02/FIGURAS
06_OUTPUTS/03_MESO/WRF/SSTN2008/d03/FIGURAS
Essa diferença deve ser respeitada na montagem do Overleaf. Não se deve presumir que as duas pipelines possuem a mesma estrutura interna de salvamento.
13.3. Comparação mais robusta
A comparação quantitativa mais robusta deve priorizar d02 e d03, pois esses domínios representam melhor a resposta costeira e regional.
O d01 deve ser mantido como diagnóstico sinótico, principalmente para interpretar o padrão amplo de circulação e aporte de umidade.
14. PRODUTOS A SEREM USADOS NA APRESENTAÇÃO
14.1. Pasta principal
A apresentação será construída a partir da pasta:
06_OUTPUTS
A pasta 06_OUTPUTS deverá ser baixada integralmente para o computador local e inserida no Overleaf, preservando sua estrutura interna.
14.2. Produtos CLIMSST
Os produtos PNG da comparação CTRL × CLIMSST estão em:
06_OUTPUTS/02_SINOTICA/WRF/CLIMSST/d01
06_OUTPUTS/03_MESO/WRF/CLIMSST/d02
06_OUTPUTS/03_MESO/WRF/CLIMSST/d03
Os arquivos de controle e tabelas estão em:
06_OUTPUTS/00_CONTROLE_GERAL/WRF/CLIMSST
Produtos principais:
tsm_media_ctrl_climsst_d01.png
tsm_media_ctrl_climsst_d02.png
tsm_media_ctrl_climsst_d03.png
precipitacao_acumulada_ctrl_climsst_d01.png
precipitacao_acumulada_ctrl_climsst_d02.png
precipitacao_acumulada_ctrl_climsst_d03.png
ivt_medio_ctrl_climsst_d01.png
ivt_medio_ctrl_climsst_d02.png
ivt_medio_ctrl_climsst_d03.png
14.3. Produtos SSTN2008
Os produtos PNG da comparação CTRL × SSTN2008 estão em:
06_OUTPUTS/02_SINOTICA/WRF/SSTN2008/d01/FIGURAS
06_OUTPUTS/03_MESO/WRF/SSTN2008/d02/FIGURAS
06_OUTPUTS/03_MESO/WRF/SSTN2008/d03/FIGURAS
Os arquivos de controle e tabelas estão em:
06_OUTPUTS/00_CONTROLE_GERAL/WRF/SSTN2008
Produtos principais:
tsm_media_ctrl_sstn2008_d01.png
tsm_media_ctrl_sstn2008_d02.png
tsm_media_ctrl_sstn2008_d03.png
precipitacao_acumulada_ctrl_sstn2008_d01.png
precipitacao_acumulada_ctrl_sstn2008_d02.png
precipitacao_acumulada_ctrl_sstn2008_d03.png
ivt_medio_ctrl_sstn2008_d01.png
ivt_medio_ctrl_sstn2008_d02.png
ivt_medio_ctrl_sstn2008_d03.png
14.4. Tabelas de resumo
As principais tabelas CSV para consulta estão em:
06_OUTPUTS/00_CONTROLE_GERAL/WRF/CLIMSST
06_OUTPUTS/00_CONTROLE_GERAL/WRF/SSTN2008
Tabelas principais:
resumo_precipitacao_acumulada_ctrl_climsst_02_PLOTAGENS_DIS_v3-1_CLIMSST.csv
resumo_ivt_medio_ctrl_climsst_02_PLOTAGENS_DIS_v3-1_CLIMSST.csv
resumo_precipitacao_acumulada_ctrl_sstn2008_02_PLOTAGENS_DIS_v3-2_SSTN2008_NOV2022.csv
resumo_ivt_medio_ctrl_sstn2008_02_PLOTAGENS_DIS_v3-2_SSTN2008_NOV2022.csv
15. ESTRUTURA RECOMENDADA PARA A APRESENTAÇÃO
15.1. Slide 1 — Objetivo
Comparar a resposta do WRF em NOV/2022 sob três configurações oceânicas:
CTRL;
CLIMSST;
SSTN2008.
15.2. Slide 2 — Rodadas
Apresentar a definição das rodadas:
CTRL_NOV2022_dis = sinótica de 2022 + TSM de 2022;
CLIMSST_NOV2022_dis = sinótica de 2022 + TSM climatológica;
SSTN2008_NOV2022_dis = sinótica de 2022 + TSM de novembro de 2008.
15.3. Slide 3 — Domínios e escalas
Apresentar a organização:
d01 = sinótica;
d02 = mesoescala;
d03 = mesoescala.
15.4. Slide 4 — Comparação de TSM
Mostrar os mapas de TSM média e diferença:
CLIMSST − CTRL;
SSTN2008 − CTRL.
Objetivo: demonstrar que as duas sensibilidades representam alterações oceânicas distintas.
15.5. Slide 5 — Precipitação acumulada CTRL × CLIMSST
Apresentar os mosaicos de precipitação acumulada e diferença para d01, d02 e d03.
15.6. Slide 6 — Precipitação acumulada CTRL × SSTN2008
Apresentar os mosaicos de precipitação acumulada e diferença para d01, d02 e d03.
15.7. Slide 7 — Comparação das diferenças médias de precipitação
Apresentar tabela com:
CLIMSST − CTRL;
SSTN2008 − CTRL.
Destacar que SSTN2008 produziu maior redução média da precipitação, principalmente em d02 e d03.
15.8. Slide 8 — IVT médio CTRL × CLIMSST
Apresentar os mapas de IVT médio e diferença para d01, d02 e d03.
Destacar que CLIMSST reduziu o IVT principalmente sobre terra e em d03.
15.9. Slide 9 — IVT médio CTRL × SSTN2008
Apresentar os mapas de IVT médio e diferença para d01, d02 e d03.
Destacar que SSTN2008 reduziu o IVT médio geral nos três domínios, principalmente em d03.
15.10. Slide 10 — Comparação entre precipitação e IVT
Apresentar a síntese conjunta:
SSTN2008 reduziu mais a precipitação média geral;
SSTN2008 reduziu mais o IVT médio geral;
CLIMSST reduziu mais o IVT sobre terra, mas com impacto menor na precipitação acumulada;
a resposta atmosférica depende da estrutura espacial da TSM e da organização do transporte de umidade.
15.11. Slide 11 — Comparação com climatologia GPM
Apresentar, quando disponível:
CTRL − CLIM GPM;
CLIMSST − CLIM GPM;
SSTN2008 − CLIM GPM.
Objetivo: avaliar se as simulações se afastam ou se aproximam de um padrão climatológico observacional.
15.12. Slide 12 — Síntese
Concluir que a comparação entre comparações indica maior impacto da rodada SSTN2008 na precipitação e no IVT médio geral.
16. DIRETRIZES PARA AS PRÓXIMAS ETAPAS
As próximas etapas devem manter as seguintes diretrizes:
verificar a estrutura real de pastas antes de escrever caminhos no relatório, no LaTeX ou no código;
não presumir que pipelines diferentes possuem a mesma organização interna;
evitar criar novas subpastas sem necessidade;
manter salvamento simples e direto;
usar apenas PNG para figuras finais;
manter CSV e NetCDF apenas quando forem produtos de controle ou reprocessamento;
abrir arquivos WRF um por vez;
reaproveitar produtos intermediários válidos;
evitar leitura massiva de NetCDF;
manter d01 em 02_SINOTICA;
manter d02 e d03 em 03_MESO;
evitar títulos administrativos nas figuras;
evitar sobreposição entre títulos, eixos, grades, legendas e colorbars.
17. SÍNTESE FINAL
A pipeline foi reorganizada para atender à lógica da dissertação e da Qualificação I.
A v3 consolidou os blocos de MHW, ASAS e diagnósticos WRF da rodada controle.
A v3-1 consolidou a comparação entre CTRL_NOV2022_dis e CLIMSST_NOV2022_dis.
A v3-2 consolidou a comparação entre CTRL_NOV2022_dis e SSTN2008_NOV2022_dis.
As duas sensibilidades oceânicas reduziram a precipitação média em relação ao CTRL.
A redução foi menor em CLIMSST e maior em SSTN2008.
O diagnóstico de IVT acrescentou uma camada física à interpretação. A rodada SSTN2008 também reduziu mais o IVT médio geral, especialmente no d03, enquanto CLIMSST apresentou redução mais clara do IVT sobre terra.
A interpretação preliminar é que a condição oceânica de NOV/2022 contribuiu para sustentar maior precipitação e maior transporte integrado de umidade no CTRL. A substituição por TSM climatológica reduz parte dessa resposta, mas a substituição por uma TSM real de novembro de 2008 produz impacto mais intenso na precipitação e no IVT médio geral.
A apresentação deve explorar justamente essa comparação entre comparações: não apenas CTRL contra uma rodada de sensibilidade, mas a diferença entre duas formas distintas de alterar a condição oceânica de superfície.
