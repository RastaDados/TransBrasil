## Estatísticas Descritivas

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from datetime import datetime

#Carregando os dados
df = pd.read_excel('dados/Delivery truck trip data.xlsx', sheet_name='VTS Data 280820')

#Estatísticas descritivas
print(df.describe())

#Verificando as informações 
print(df.info())

#Verificando se há valores nulos
print(df.isnull().sum())
```

![01](https://github.com/user-attachments/assets/59fbdeec-3661-4bc3-95b9-13348a5ea03c)

<hr>

## Análise Exploratória

#### Distribuição de viagens por status de pontualidade

```python
plt.figure(figsize=(8, 6))
sns.countplot(data=df, x='on_time_status')
plt.title('Distribuição de Viagens por Status de Pontualidade')
plt.show()
```

![01](https://github.com/user-attachments/assets/fccfdf60-dbd2-45ff-bc3d-63d45d354025)

<hr>

#### Distribuição de distâncias de transporte

```python
plt.figure(figsize=(10, 6))
sns.histplot(data=df, x='TRANSPORTATION_DISTANCE_IN_KM', bins=30)
plt.title('Distribuição de Distâncias de Transporte')
plt.xlabel('Distância (km)')
plt.show()
```

![01](https://github.com/user-attachments/assets/6bc87389-8339-4e01-a743-336dbebb18e1)

<hr>

#### Correlação entre distância e duração

```python
plt.figure(figsize=(10, 6))
sns.scatterplot(data=df, x='TRANSPORTATION_DISTANCE_IN_KM', y='actual_duration', hue='on_time_status')
plt.title('Relação entre Distância e Duração da Viagem')
plt.xlabel('Distância (km)')
plt.ylabel('Duração (horas)')
plt.show()
```

![01](https://github.com/user-attachments/assets/d55f7cf0-72bd-4240-8ab5-839d5fac430e)

<hr>

#### Materiais mais transportados

```pyhon
top_materials = df['Material Shipped'].value_counts().head(10)
plt.figure(figsize=(12, 6))
sns.barplot(x=top_materials.values, y=top_materials.index)
plt.title('Top 10 Materiais Mais Transportados')
plt.xlabel('Quantidade de Viagens')
plt.show()
```

![01](https://github.com/user-attachments/assets/f48ffba5-fd26-4476-91e6-44b548c0564e)

<hr>

## Análise temporal

####Viagens Por Dia da Semana

```python
#Viagens por dia da semana
plt.figure(figsize=(10, 6))
sns.countplot(data=df, x='trip_start_day', order=['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'])
plt.title('Distribuição de Viagens por Dia da Semana')
plt.show()
```

![01](https://github.com/user-attachments/assets/57e922e3-1421-45da-ae57-88e8010586f9)

<hr>

#### Viagens Por Hora do Dia

```python
#Viagens por hora do dia
plt.figure(figsize=(12, 6))
sns.countplot(data=df, x='trip_start_hour')
plt.title('Distribuição de Viagens por Hora do Dia')
plt.show()
```

![01](https://github.com/user-attachments/assets/7af38c02-5247-4ae7-be0d-89a83ff09991)






