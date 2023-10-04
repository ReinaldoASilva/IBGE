# README

Este é um código Python que realiza web scraping no site do IBGE (Instituto Brasileiro de Geografia e Estatística) para obter informações estatísticas sobre um determinado estado brasileiro.

## Pré-requisitos

Antes de executar o código, você deve ter instalado as seguintes bibliotecas Python:

- pandas
- requests
- beautifulsoup4

Você pode instalá-las usando o gerenciador de pacotes pip. Execute o seguinte comando no terminal:

```
pip install pandas requests beautifulsoup4
```

## Como usar

1. Importe as bibliotecas necessárias:

```python
import pandas as pd
import requests
from bs4 import BeautifulSoup
```

2. Defina a função `scraping_uf(uf:str)` para realizar o web scraping. Essa função recebe como parâmetro a sigla de um estado brasileiro (por exemplo, 'sp' para São Paulo) e retorna um dicionário com os indicadores estatísticos encontrados no site do IBGE para aquele estado.

```python
def scraping_uf(uf:str):
    uf_url = f'https://www.ibge.gov.br/cidades-e-estados/{uf}.html'
    browser = browsers = {'User-Agent': "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 \(KHTML, like Gecko) Chrome / 86.0.4240.198Safari / 537.36"}
    page = requests.get(uf_url, headers = browser)
    
    soup = BeautifulSoup(page.content, 'html.parser')
    indicador = soup.select('.indicador')
   
    uf_dict = {
        dado.select('.ind-label')[0].text: dado.select('.ind-value')[0].text
        for dado in indicador
   }
   
    return uf_dict
```

3. Execute o web scraping para um determinado estado. No exemplo abaixo, estamos obtendo os indicadores estatísticos para o estado de São Paulo:

```python
estado = scraping_uf('sp')
```

4. Faça o tratamento dos dados obtidos, removendo caracteres indesejados dos valores dos indicadores. No exemplo abaixo, estamos removendo trechos de texto entre colchetes e espaços em branco no final dos valores:

```python
for indicador in estado:
    if ']' in estado[indicador]:
        estado[indicador] = estado[indicador].split(']')[0][:-8]
    else:
        estado [indicador] = estado[indicador]
```

5. Crie um DataFrame do pandas com os indicadores obtidos:

```python
df = pd.DataFrame(estado.values(), index=estado.keys())
```

## Contribuição

Contribuições para aprimorar este código são bem-vindas. Se você encontrar problemas, tiver sugestões de melhorias ou quiser adicionar novos recursos, fique à vontade para abrir uma issue ou enviar um pull request.


