Vamos iniciar o projeto analisando indicadores climáticos como: Média de Precipitação Anual, Contagem de Dias Chuvosos por Mês e Correlação entre Temperatura e Umidade. Esses indicadores são fundamentais para entender os impactos do clima e como ele pode aetar o meio ambiente e a qualidade de vida das pessoas.

### Convertendo a coluna data_medição para o formato AAAA-MM-DD
``` sql
ALTER TABLE dadosmensais20202025 ADD COLUMN data_formatada DATE;
UPDATE dadosmensais20202025
SET data_formatada = DATE(substr(data_medicao, 7, 4) || '-' || 
                          substr(data_medicao, 4, 2) || '-' || 
                          substr(data_medicao, 1, 2));

```
### Média de precipitação anual (para verificar tendência de chuva)
Calcular a média de precipitação anual, nos ajuda a identificar se há tendência de aumento ou diminuição da chuva ao longo do tempo, sendo essencial para prever eventos climáticos extremos como secas ou tempestades.
``` sql
SELECT strftime('%Y', data_formatada) AS ano, 
       AVG(precipitacao_total_mensal) AS media_precipitacao
FROM dadosmensais20202025
WHERE data_formatada IS NOT NULL AND precipitacao_total_mensal IS NOT NULL
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
Os valores null serão excluídos, mas os valores iguais a zero foram mantidos, isso porque valores null são valores ausentes, ou seja, não possuem informação
``` sql
SELECT strftime('%Y-%m', data_formatada) AS mes,
      AVG(precip_pluv_mensal) AS media_dias_chuva
FROM dadosmensais20202025
WHERE data_formatada IS NOT NULL AND precip_pluv_mensal IS NOT NULL
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
SELECT data_formatada, temperatura_maxima_mediamensal, umidade_relativa_ar_mediamensal
FROM dadosmensais20202025
WHERE temperatura_maxima_mediamensal IS NOT NULL
AND umidade_relativa_ar_mediamensal IS NOT NULL;
```
Resultado
```markdown
|Data_Medicao	|TEMPERATURA_MAXIMA_MEDIAMENSAL	|UMIDADE_RELATIVA_AR_MEDIAMENSAL |
|-------------|-------------------------------|--------------------------------|
|2020-01-31  	|28.5                           |75.2
|2020-02-29  	|27                             |83.5
|2020-03-31  	|27.8                           |74.5
|2021-07-31  	|22.6                           |72
|2021-08-31  	|24.8                           |67.8
|2022-11-30  	|25.9                           |72.7
|2022-12-31  	|27.4                           |77.2
|2023-01-31  	|27.8                           |77.7
|2023-02-28  	|28.6                           |78.6
|2023-03-31  	|29.7                           |73.9
|2023-04-30  	|25.8                           |76.2
|2023-05-31  	|24.7                           |74.1
|2023-06-30  	|23.4                           |76.6
|2023-07-31  	|23.9                           |74
|2023-08-31  	|25.9                           |68.9
|2023-09-30  	|30.3                           |66.1
|2023-10-31  	|27.1                           |78
|2023-12-31  	|31                             |68
|2024-01-31  	|29.3                           |79.7
|2024-02-29  	|30.1                           |76.1
|2024-03-31  	|29.7                           |77.8
|2024-04-30  	|29.5                           |71.4
|2024-05-31  	|27.3                           |66.9
|2024-06-30  	|26.3                           |65.6
|2024-07-31  	|24                             |71.7
|2024-08-31  	|26.7                           |60.7
|2024-09-30  	|30.1                           |61.1
|2024-10-31  	|27.4                           |75.4
|2024-11-30  	|27.7                           |75.5
|2024-12-31  	|28.5                           |77.3
|2025-01-31  	|29.5                           |75.5
```
### Comparativo Precipitação Acumulada(mm)
```sql
SELECT
	 strftime('%Y', date(substr(data_medicao, 7, 4) || '-' || 
                         substr(data_medicao, 4, 2) || '-' || 
                         substr(data_medicao, 1, 2))) AS ano,
     CASE strftime('%m', date(substr(data_medicao, 7, 4) || '-' || 
                              substr(data_medicao, 4, 2) || '-' || 
                              substr(data_medicao, 1, 2))) 
        WHEN '01' THEN 'Jan' WHEN '02' THEN 'Fev' WHEN '03' THEN 'Mar'
        WHEN '04' THEN 'Abr' WHEN '05' THEN 'Mai' WHEN '06' THEN 'Jun'
        WHEN '07' THEN 'Jul' WHEN '08' THEN 'Ago' WHEN '09' THEN 'Set'
        WHEN '10' THEN 'Out' WHEN '11' THEN 'Nov' WHEN '12' THEN 'Dez'
        END as mes,
        SUM(precipitacao_total_mensal) AS precipitacao_total_mensal
FROM dadosmensais20202025
WHERE data_medicao IS NOT NULL and precipitacao_total_mensal IS NOT NULL
GROUP BY ano, mes
ORDER BY ano, mes;
```
Resultado
```markdown
|ano	|mês	|Precipitação Total |
|----   |----   |------------------ |
|2020   |Fev	|505,7              |
|2020	|Jan	|287,4              |
|2021	|Ago	|44,4               |
|2021	|Dez	|127,3              |
|2021	|Nov	|98,6               |
|2021	|Out	|91,9               |
|2021	|Set	|39,5               |
|2022	|Ago	|22,8               |
|2022	|Dez	|159,8              |
|2022	|Fev	|68,1               |
|2022	|Jan	|357,2              |
|2022	|Jul	|9,0                |
|2022	|Jun	|32,3               |
|2022	|Mai	|42,3               |
|2022	|Mar	|213,9              |
|2022	|Nov	|181,6              |
|2022	|Out	|95,9               |
|2022	|Set	|129,6              |
|2023	|Abr	|106,8              |
|2023	|Ago	|28,3               |
|2023	|Dez	|95,9               |
|2023	|Fev	|428,9              |
|2023	|Jan	|211,0              |
|2023	|Jul	|11,0               |
|2023	|Jun	|46,9               |
|2023	|Mai	|51,1               |
|2023	|Mar	|239,2              |
|2023	|Nov	|182,8              |
|2023	|Out	|356,0              |
|2023	|Set	|77,6               |
|2024	|Abr	|16,6               |
|2024	|Ago	|59,0               |
|2024	|Dez	|231,0              |
|2024	|Fev	|231,9              |
|2024	|Jan	|286,6              |
|2024	|Jul	|68,0               |
|2024	|Jun	|0,0                |
|2024	|Mai	|61,7               |
|2024	|Mar	|283,6              |
|2024	|Nov	|122,5              |
|2024	|Out	|177,4              |
|2024	|Set	|15,2               |
|2025	|Jan	|334,2              |
