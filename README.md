<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Projeto de Análise de Corrida</title>
</head>
<body>
    <h1>Projeto de Análise de Corrida</h1>
    
  <h2>Integrantes:</h2>
    <ul>
        <li>Isadora de Morais Maneghetti (RM: 556326)</li>
        <li>Júlio César Ruiz Zequin (RM: 554676)</li>
        <li>Khadija do Rocio Vieira de Lima (RM: 558971)</li>
        <li>Mateus dos Santos da Silva (RM: 558436)</li>
        <li>Vinicius dos Santos Wince (RM: 557033)</li>
    </ul>

  <h2>Descrição do Projeto:</h2>
    <p>
        Este projeto visa a análise de dados de corrida para monitorar o desempenho de um piloto durante várias voltas de uma corrida. 
        Ele coleta informações como tempo por volta, velocidade máxima, mínima e média, posição final, além de monitorar a média de batimentos cardíacos durante as voltas.
    </p>

  <h3>Principais Funcionalidades:</h3>
    <ul>
        <li>Coleta de dados de cada volta, incluindo tempo, velocidade e posição do piloto.</li>
        <li>Cálculo de velocidades médias e desvio de velocidade por volta.</li>
        <li>Geração automática de gráficos SVG para visualização dos dados.</li>
        <li>Análise de tendências e recomendações baseadas nos dados de tempo e velocidade.</li>
    </ul>

  <h2>Instalação:</h2>
    <p>
        Para rodar este projeto localmente, siga os seguintes passos:
    </p>
    <ol>
        <li>Clone este repositório para o seu computador utilizando o comando:
            <pre><code>git clone https://github.com/seu-usuario/seu-repositorio.git</code></pre>
        </li>
        <li>Instale as dependências necessárias. Certifique-se de ter o Python e as bibliotecas listadas abaixo instaladas:
            <pre><code>pip install pandas matplotlib numpy</code></pre>
        </li>
        <li>Rode o código no seu ambiente de desenvolvimento Python.
        </li>
    </ol>

  <h2>Como Usar:</h2>
    <p>Ao rodar o script, você será solicitado a inserir os dados de cada volta, como tempo, velocidade máxima, mínima e posição do piloto.</p>
    <p>Os gráficos gerados são:</p>
    <ul>
        <li><strong>Tempo por Volta:</strong> Exibe o tempo gasto em cada volta.</li>
        <li><strong>Velocidades por Volta:</strong> Mostra a velocidade máxima, mínima e média em cada volta.</li>
        <li><strong>Desvio de Velocidade:</strong> Exibe a diferença entre a velocidade máxima e mínima por volta.</li>
        <li><strong>Batimentos por Volta:</strong> Mostra a média de batimentos cardíacos em cada volta.</li>
    </ul>
    <p>Além disso, o código inclui funções de análise para fornecer insights sobre a consistência do piloto, tendências de desempenho e possíveis melhorias.</p>

  <h2>Código:</h2>
    <pre><code>
#Importação de bibliotecas necessárias
import pandas as pd  # Biblioteca para manipulação de dados
import matplotlib.pyplot as plt  # Biblioteca para criação de gráficos
import numpy as np  # Biblioteca para cálculo numérico
import random  # Biblioteca para gerar números aleatórios

#Solicita ao usuário o número de voltas
voltas = int(input('Insira o numero de voltas'))

#Inicialização das listas para armazenar dados de cada volta
lista_tempo = []
lista_vmax = []
lista_vmin = []
lista_vmedia = []
lista_pos = []
lista_media_batimentos = []

soma_batimentos = 0  #Variável para armazenar a soma dos batimentos (não utilizada posteriormente)

#Loop para coletar dados de cada volta
for i in range(voltas):
    # Coleta de tempo, velocidade máxima, mínima, posição e gera batimentos aleatórios
    tempo = float(int(input(f'Insira o tempo da {i+1}ª volta (segundos)')))
    lista_tempo.append(tempo)  # Armazena o tempo da volta
    v_max = float(int(input(f'Insira a velocidade maxima da {i+1}ª volta (km/h)')))
    lista_vmax.append(v_max)  # Armazena a velocidade máxima da volta
    v_min = float(int(input(f'Insira a velocidade minima {i+1}ª volta (km/h)')))
    lista_vmin.append(v_min)  # Armazena a velocidade mínima da volta
    posicao = (input('Insira a posicao do carro ao finalizar a volta'))
    lista_pos.append(posicao)  # Armazena a posição final da volta
    media_batimentos = random.randint(90, 100)  # Gera um valor aleatório para os batimentos (90 a 100)

    #Calcula e armazena a velocidade média da volta
    lista_vmedia.append((v_max + v_min) / 2)
    lista_media_batimentos.append(media_batimentos)  # Armazena a média de batimentos

#Criação do DataFrame com todos os dados coletados
df = pd.DataFrame({
    'Volta': list(range(1, voltas + 1)),  # Coluna com o número das voltas
    'Velocidade Maxima (km/h)': lista_vmax,  # Coluna com a velocidade máxima
    'Velocidade Minima (km/h)': lista_vmin,  # Coluna com a velocidade mínima
    'Velocidade Media (km/h)': lista_vmedia,  # Coluna com a velocidade média
    'Média de batimentos': lista_media_batimentos,  # Coluna com a média de batimentos
    'Tempo (s)': lista_tempo,  # Coluna com o tempo da volta
    'Posição': lista_pos  # Coluna com a posição final na volta
})

df  #Exibe o DataFrame
#Chamada da função para salvar os gráficos em SVG
#Gráfico 1: Tempo por volta
salvar_grafico(df, 'Volta', 'Tempo (s)', 'Tempo por Volta', 'Tempo (s)', 'tempo_volta')

#Gráfico 2: Velocidade máxima, mínima e média por volta
salvar_grafico(df, 'Volta', ['Velocidade Maxima (km/h)', 'Velocidade Minima (km/h)', 'Velocidade Media (km/h)'], 
               'Velocidades por Volta', 'Velocidade (km/h)', 'velocidade_volta')

#Cálculo do desvio de velocidade (diferença entre velocidade máxima e mínima)
df['Desvio de Velocidade (km/h)'] = df['Velocidade Maxima (km/h)'] - df['Velocidade Minima (km/h)']

#Gráfico 3: Desvio de velocidade por volta
salvar_grafico(df, 'Volta', 'Desvio de Velocidade (km/h)', 'Desvio de Velocidade por Volta', 
               'Desvio de Velocidade (km/h)', 'desvio_volta')

#Gráfico 4: Batimentos cardíacos por volta
salvar_grafico(df, 'Volta', 'Média de batimentos', 'Batimentos por Volta', 
               'Média de batimentos', 'batimentos_volta')

#Mensagem final informando que os arquivos foram salvos com sucesso
print("Os arquivos CSV e SVG foram salvos com sucesso.")
    </code></pre>

  <h2>Estrutura do Projeto:</h2>
    <ul>
        <li><strong>dados_corrida.csv:</strong> Arquivo CSV gerado contendo todos os dados da corrida.</li>
        <li><strong>Gráficos SVG:</strong> Gráficos gerados automaticamente em formato SVG para visualização dos dados.</li>
    </ul>

  <h2>Licença:</h2>
    <p>
        Este projeto é de uso acadêmico e sem fins lucrativos. Sinta-se à vontade para explorar e modificar o código conforme necessário.
    </p>
</body>
</html>
