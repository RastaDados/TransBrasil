# Dashboard Utilizando a Biblioteca Dash do Python

```python
import dash
from dash import dcc, html
from dash.dependencies import Input, Output
import plotly.express as px

#Criando o aplicativo Dash
app = dash.Dash(__name__)

#Preparando os dados para o dashboard
#Criando uma lista de opções para os dropdowns
status_options = [{'label': 'Todos', 'value': 'All'}] + [{'label': status, 'value': status} for status in df['on_time_status'].unique()]
material_options = [{'label': 'Todos', 'value': 'All'}] + [{'label': material, 'value': material} for material in df['Material Shipped'].unique()]

#Layout do dashboard
app.layout = html.Div([
    html.H1("Dashboard de Análise de Viagens de Caminhões", style={'textAlign': 'center'}),
    
    html.Div([
        html.Div([
            dcc.Dropdown(
                id='status-dropdown',
                options=status_options,
                value='All',
                multi=False,
                placeholder="Filtrar por status"
            )
        ], style={'width': '48%', 'display': 'inline-block'}),
        
        html.Div([
            dcc.Dropdown(
                id='material-dropdown',
                options=material_options,
                value='All',
                multi=False,
                placeholder="Filtrar por material"
            )
        ], style={'width': '48%', 'float': 'right', 'display': 'inline-block'})
    ]),
    
    dcc.Graph(id='distance-duration-scatter'),
    
    html.Div([
        html.Div([
            dcc.Graph(id='status-pie')
        ], style={'width': '48%', 'display': 'inline-block'}),
        
        html.Div([
            dcc.Graph(id='material-bar')
        ], style={'width': '48%', 'float': 'right', 'display': 'inline-block'})
    ]),
    
    dcc.Graph(id='time-heatmap')
])

#Callbacks 
@app.callback(
    [Output('distance-duration-scatter', 'figure'),
     Output('status-pie', 'figure'),
     Output('material-bar', 'figure'),
     Output('time-heatmap', 'figure')],
    [Input('status-dropdown', 'value'),
     Input('material-dropdown', 'value')]
)
def update_dashboard(status_filter, material_filter):
    #Criando uma cópia do dataframe para filtragem
    filtered_df = df.copy()
    
    #Aplicando os filtros
    if status_filter != 'All':
        filtered_df = filtered_df[filtered_df['on_time_status'] == status_filter]
    
    if material_filter != 'All':
        filtered_df = filtered_df[filtered_df['Material Shipped'] == material_filter]
```

#### Relação dentre Distância e Duração da Viagem

```python
 #Gráfico de dispersão
    scatter_fig = px.scatter(
        filtered_df, 
        x='TRANSPORTATION_DISTANCE_IN_KM', 
        y='actual_duration',
        color='on_time_status',
        title='Relação entre Distância e Duração da Viagem',
        labels={
            'TRANSPORTATION_DISTANCE_IN_KM': 'Distância (km)',
            'actual_duration': 'Duração Real (horas)',
            'on_time_status': 'Status'
        }
    )
```

![01](https://github.com/user-attachments/assets/b2ff43ae-60da-4ad1-a38c-6f3134ed3f57)

<hr>

#### Distribuição de Status de Pontualidade

```python
 #Gráfico de pizza - status
    status_counts = filtered_df['on_time_status'].value_counts().reset_index()
    status_counts.columns = ['status', 'count']
    pie_fig = px.pie(
        status_counts,
        values='count',
        names='status',
        title='Distribuição de Status de Pontualidade'
    )
```

![2](https://github.com/user-attachments/assets/018810fd-76a0-4709-901b-a52cb8645aa6)

<hr> 

#### Top 10 Materiais Mais Transportados

```python
#Gráfico de barras - materiais
    material_counts = filtered_df['Material Shipped'].value_counts().head(10).reset_index()
    material_counts.columns = ['material', 'count']
    bar_fig = px.bar(
        material_counts,
        x='count',
        y='material',
        orientation='h',
        title='Top 10 Materiais Mais Transportados',
        labels={
            'count': 'Número de Viagens',
            'material': 'Material'
        }
    )
```

![3](https://github.com/user-attachments/assets/546b0e69-a113-4e99-a010-bb885e25ded6)

<hr>

#### Distribuição de Viagens por Dia e Hora

```python
#Heatmap temporal
    time_df = filtered_df.groupby(['trip_start_day', 'trip_start_hour']).size().reset_index(name='counts')
    #Ordenando por dias da semana
    days_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
    time_df['trip_start_day'] = pd.Categorical(time_df['trip_start_day'], categories=days_order, ordered=True)
    time_df = time_df.sort_values('trip_start_day')
    
    heat_fig = px.density_heatmap(
        time_df,
        x='trip_start_day',
        y='trip_start_hour',
        z='counts',
        title='Distribuição de Viagens por Dia e Hora',
        labels={
            'trip_start_day': 'Dia da Semana',
            'trip_start_hour': 'Hora do Dia',
            'counts': 'Número de Viagens'
        }
    )
```

![4](https://github.com/user-attachments/assets/ea7720d4-f7ad-49e9-9f1b-721ddcc081f8)





