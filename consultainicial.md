Vamos iniciar o projeto analisando indicadores climáticos como: Média de Precipitação Anual, Contagem de Dias Chuvosos por Mês e Correlação entre Temperatura e Umidade. Esses indicadores são fundamentais para entender os impactos do clima e como ele pode aetar o meio ambiente e a qualidade de vida das pessoas.
### Média de precipitação anual (para verificar tendência de chuva)
Calcular a média de precipitação anual, nos ajuda a identificar se há tendência de aumento ou diminuição da chuva ao longo do tempo, sendo essencial para prever eventos climáticos extremos como secas ou tempestades.
``` sql
SELECT strftime('%Y', 
                date(substr(data_medicao, 7, 4) || '-' || 
                     substr(data_medicao, 4, 2) || '-' || 
                     substr(data_medicao, 1, 2))) AS ano,
       AVG(precipitacao_total_mensal) AS media_precipitacao
FROM dadosmensais20202025estacaoCONVMIRSANTANA_editado
WHERE data_medicao IS NOT NULL AND precipitacao_total_mensal IS NOT NULL
GROUP BY ano
ORDER BY ano;
```
Resultado
```markdown
|ano       |media_precipitacao |
|----------|-------------------|
|2020      |396.54999999999995 |
|2021      |80.34              |
|2022      |119.31818181818181 |
|2023      |152.9              |
|2024      |129.45833333333334 |
|2025      |334.2              |
```
### Contagem de Dias Chuvosos por Mês (Analisando Impacto na Drenagem)
Essa avaliação nos ajudará a identificar os meses com maior probabilidade de chuvas intensas, assim a cidade pode implementar alertas preventivos, reforçar a infraestrutura e tomar medidas de mitigação para reduzir os riscos de alagamentos.
``` sql
SELECT strftime('%Y-%m', 
                date(substr(data_medicao, 7, 4) || '-' || 
                     substr(data_medicao, 4, 2) || '-' || 
                     substr(data_medicao, 1, 2))) AS mes, 
       AVG(precip_pluv_mensal) AS media_dias_chuva
FROM dadosmensais20202025estacaoCONVMIRSANTANA_editado
WHERE data_medicao IS NOT NULL 
AND precip_pluv_mensal IS NOT NULL
GROUP BY mes
ORDER BY mes;
```
Resultado
```markdown
|mes          |media_dias_chuva  |
|-------------|------------------|
|2020-01      |16                |
|2020-02      |22                |
|2020-03      |12                |
|2021-03      |4                 |
|2021-06      |2                 |
|2021-07      |2                 |
|2021-08      |6                 |
|2021-09      |6                 |
|2021-10      |11                |
|2021-11      |7                 |
|2021-12      |11                |
|2022-01      |20                |
|2022-02      |10                |
|2022-03      |12                |
|2022-04      |3                 |  
|2022-05      |7                 |
|2022-06      |4                 |
|2022-07      |1                 |
|2022-08      |4                 |
|2022-09      |12                |
|2022-10      |10                |
|2022-11      |14                |
|2022-12      |16                |
|2023-01      |21                |
|2023-02      |25                |
|2023-03      |17                |
|2023-04      |12                |
|2023-05      |6                 |
|2023-06      |6                 |
|2023-07      |4                 |
|2023-08      |6                 |
|2023-09      |3                 |
|2023-10      |14                |
|2023-11      |11                |
|2023-12      |10                |
|2024-01      |17                |
|2024-02      |14                |
|2024-03      |13                |
|2024-04      |4                 |
|2024-05      |5                 |
|2024-06      |0                 |
|2024-07      |4                 |
|2024-08      |5                 |
|2024-09      |7                 |
|2024-10      |12                |
|2024-11      |10                |
|2024-12      |16                |
|2025-01      |17                |
|2025-02      |7                 |

```
### Correlação entre Temperatura e Umidade (Impacto do Calor)
Temperaturas elevadas combinadas com alta umidade podem agravar o desconforto térmico e aumentar riscos de problemas de saúde, como desidratação e golpes de calor. Além disso, uma  correlação crescente entre temperatura e umidade pode ser um indicativo das mudanças climáticas, mostrando um aumento do calor mais intensificado. Isso pode levar a medidas de mitigação para proteger a população.
```sql
SELECT data_medicao, temperatura_maxima_mediamensal, umidade_relativa_ar_mediamensal
FROM dadosmensais20202025estacaoCONVMIRSANTANA_editado
WHERE temperatura_maxima_mediamensal IS NOT NULL
AND umidade_relativa_ar_mediamensal IS NOT NULL;
```
Resultado
```markdown
|Data_Medicao	|TEMPERATURA_MAXIMA_MEDIAMENSAL	|UMIDADE_RELATIVA_AR_MEDIAMENSAL |
|-------------|-------------------------------|--------------------------------|
|31/01/2020   |28.5	                          |75.2                            |
|29/02/2020  	|27	                            |83.5                            |
|31/03/2020   |27.8	                          |74.5                            |
|31/07/2021	  |22.6	                          |72                              |
|31/08/2021	  |24.8	                          |67.8                            |
|30/11/2022	  |25.9	                          |72.7                            |
|31/12/2022	  |27.4	                          |77.2                            |
|31/01/2023	  |27.8	                          |77.7                            |
|28/02/2023	  |28.6	                          |78.6                            |
31/03/2023	  |29.7	                          |73.9                            |
30/04/2023	  |25.8	                          |76.2                            |
31/05/2023	  |24.7	                          |74.1                            |
30/06/2023	  |23.4	                          |76.6                            |
31/07/2023	  |23.9	                          |74                              |
31/08/2023	  |25.9	                          |68.9                            |
30/09/2023	  |30.3	                          |66.1                            |
31/10/2023	  |27.1	                          |78                              |
31/12/2023	  |31	                            |68                              |
31/01/2024	  |29.3	                          |79.7                            |
29/02/2024	  |30.1	                          |76.1                            |
31/03/2024	  |29.7	                          |77.8                            |
30/04/2024	  |29.5	                          |71.4                            |
31/05/2024	  |27.3	                          |66.9                            |
30/06/2024	  |26.3	                          |65.6                            |
31/07/2024	  |24	                            |71.7                            |
31/08/2024	  |26.7		                        |60.7                            |
30/09/2024	  |30.1	                          |61.1                            |
31/10/2024	  |27.4	                          |75.4                            |
30/11/2024	  |27.7	                          |75.5                            |
31/12/2024	  |28.5	                          |77.3                            |
31/01/2025	  |29.5	                          |75.5                            |
```
