# projeto_python_prazoLM
Script Python para processar planilhas de prazos logísticos, removendo duplicidades por código IBGE, priorizando tipos de rota (LOC > INT > INTB), escolhendo os menores prazos e padronizando a coluna "PRAZO EXP" com o valor "Apenas STD". O resultado é exportado para um novo arquivo Excel pronto para análise logística.

# Filtro e Organização de Prazos Logísticos

Este projeto contém um script Python que automatiza o processamento de planilhas Excel contendo informações de prazos logísticos por município.

## Funcionalidades

- **Leitura de dados:** Carrega uma planilha Excel com códigos IBGE, cidades, UF, base, tipo de rota e prazos.
- **Remoção de duplicidades:** Mantém apenas uma linha por código IBGE, com prioridade para o tipo de rota na seguinte ordem: `LOC` (local), `INT` (interior), `INTB` (interior B).
- **Seleção do menor prazo:** Quando houver mais de uma opção para o mesmo tipo de rota, seleciona a de menor valor em `PRAZO STD`.
- **Padronização do campo de expresso:** Substitui todos os valores da coluna `PRAZO EXP` para `'Apenas STD'`, padronizando os dados de saída.
- **Exportação:** Salva o resultado final filtrado e padronizado em um novo arquivo Excel.

## Exemplo de uso

```python
import pandas as pd

CAMINHO_ARQUIVO = r'C:\Users\rodrigo.franco\OneDrive - TEX COURIER SA\Documents\Prazos LM.xlsx'
df = pd.read_excel(CAMINHO_ARQUIVO)

# Ordenação e filtro conforme a prioridade desejada
ordem_rota = {'LOC': 1, 'INT': 2, 'INTB': 3}
df['PRIORIDADE_ROTA'] = df['TIPO ROTA'].map(ordem_rota).fillna(4).astype(int)
df['PRAZO STD'] = pd.to_numeric(df['PRAZO STD'], errors='coerce')
df_ordenado = df.sort_values(['IBGE', 'PRIORIDADE_ROTA', 'PRAZO STD'], ascending=[True, True, True])
df_filtrado = df_ordenado.drop_duplicates(subset=['IBGE'], keep='first').drop(columns=['PRIORIDADE_ROTA'])

# Padronização da coluna PRAZO EXP
df_filtrado['PRAZO EXP'] = 'Apenas STD'

# Exportação do resultado
df_filtrado.to_excel('resultado_filtrado.xlsx', index=False)

# Visualização das primeiras linhas
print(df_filtrado.head())
```

## Requisitos

- Python 3.7 ou superior
- pandas
- openpyxl

Instale as dependências com:

```bash
pip install pandas openpyxl
```

## Objetivo

Este script foi criado para otimizar a seleção e padronização de prazos logísticos por município, facilitando análises e tomadas de decisão baseadas em dados consistentes e formatados conforme as regras de negócio da empresa.

---

Sinta-se à vontade para adaptar este script conforme a necessidade do seu fluxo logístico.
