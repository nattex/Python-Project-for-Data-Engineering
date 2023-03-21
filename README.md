## Python Project for Data Engineering


## Objetivos
- Executar o processo ETL
- Extrair dados bancários e de capitalização de mercado do arquivo JSON bank_market_cap.json
- Transformar a moeda da capitalização de mercado usando os dados da taxa de câmbio
- Carregar os dados transformados em um CSV separado
## Installation
```bash
  #!pip install glob
  #!pip install pandas
  #!pip install requests
  #!pip install datetime
```
## Import
```bash
  import glob
  import pandas as pd
  from datetime import datetime
```

## Comando wget
```bash
- bank_market_cap_1.json: um arquivo JSON contendo dados de capitalização de mercado de alguns bancos.

- bank_market_cap_2.json: outro arquivo JSON contendo dados de capitalização de mercado de outros bancos.

- exchange_rates.csv: um arquivo CSV contendo dados de taxas de câmbio.

!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0221EN-SkillsNetwork/labs/module%206/Lab%20-%20Extract%20Transform%20Load/data/bank_market_cap_1.json
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0221EN-SkillsNetwork/labs/module%206/Lab%20-%20Extract%20Transform%20Load/data/bank_market_cap_2.json
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-PY0221EN-SkillsNetwork/labs/module%206/Final%20Assignment/exchange_rates.csv
```

## Função extract()
```bash
    def extract_from_json(file_to_process):
       dataframe = pd.read_json(file_to_process)
       return dataframe
```

## Função extract() para o arquivo "bank_bmarket_cap_1.json".
```bash
    def extract():
        df = extract_from_json('bank_market_cap_1.json')
        df.columns = columns
        return df
```
Com a extração do arquivo, se tem o código da criação da coluna para a crianção do dataframe.

## Função convert()
- Para a capitalização de mercado convertida para libras esterlinas (GBP$ bilhões).

```bash
  def convert(n, data):
    n['Market Cap (GBP$ Billion)'] = n['Market Cap (US$ Billion)']
    n['Market Cap (GBP$ Billion)']*=data['Rates'].tolist()
    n.drop('Market Cap (US$ Billion)',axis=1, inplace=True)
    return n
```

## Função log()
- Dentro da função, o código define um formato de data e hora para o carimbo de data/hora, usando o formato "HH:MM:SS-Mês-Dia-Ano". 
- Ele obtém a data e hora atuais usando o método "datetime.now()" do pacote datetime e, em seguida, formata a data e hora de acordo com o formato definido.
```bash
    def log(message):
        timestamp_format = '%H:%M:%S-%h-%d-%Y' 
        now = datetime.now() 
        timestamp = now.strftime(timestamp_format)
        with open("dealership_logfile.txt","a") as f:
            f.write(timestamp + ',' + message + '\n')
```

## Função transform()
```bash
  def transform(data):
      data["Market Cap (US$ Billion)"] = round(data["Market Cap (US$ Billion)"] * exchange_rate, 3)
      data = data.rename(columns = {"Market Cap (US$ Billion)" : "Market Cap (GBP$ Billion)"}, inplace = False)
          return data

```

## Função load()
- Responsável por carregar os dados transformados em um banco de dados ou em outro sistema de armazenamento permanente para que possam ser usados posteriormente para análise ou outras tarefas
```bash
  def load(data): 
      log("Load phase Started")
      data.to_csv('bank_market_cap_gbp.csv')
      load(transformed_data,"bank_market_cap_gbp.csv")

```
