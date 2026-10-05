# Algoritmo de Processamento da Agenda

## Objetivo

Automatizar a agendas bruta exportada do EasyJur, aplicando regras, formatação, alinhamentos, padrões e a geração de 5 planilhas avulsas para envio para cada área. 

---

# Entrada

Arquivo Excel (`.xlsx`) exportado do EasyJur.

---

# Etapa 1 - Geração da Agenda Principal

## 1.1 Renomeação

Gerar a planilha principal utilizando o padrão:

```text
Agenda - DD.MM.AAAA
```

Exemplo:

```text
Agenda - 29.09.2026
```

---

## 1.2 Limpeza do Número do Processo

Na coluna **Número do Processo**:

- Remover aspas simples ou duplas.
- Padronizar o conteúdo para texto limpo.

---

## 1.3 Exclusão de Responsáveis

Excluir todos os registros cujo campo **Responsável 1** seja:

- Karen Xavier Cintra Freire
- Osmar Padilha
- Ingrid Toledo de Oliveira
- Brenda Naufal
- Milton Oliveira de Souza

---

## 1.4 Tratamento da Área

### Conversão de Área

Quando a área for:

```text
Penal-Criminal
```

converter automaticamente para:

```text
Cível
```

---

### Preenchimento Automático da Área

Quando a coluna **Área** estiver vazia, preencher com base no **Responsável 1**.

#### Área Cível

- Annelise Cavalcante de Almeida
- Beatriz Lourenco Berniz
- Denis Ranieri
- Fernanda Machado Melendez
- Guilherme Brandão Nunes Caldeira
- Mariana Riotinto Martins Silva
- Nicoly Nascimento Jorge
- Pedro Henrique Almeida Conceição
- Ricardo Alexandre Politi

#### Área Trabalhista

- Beatriz Julia Raial Mambrini
- Claudia Aguiar Racz
- Gabriella da Silva Caboclo
- Indyara Tome de Brito
- Joao Lucas Silva Barbosa
- Rafael Manta de Brito

#### Área Tributária

- Samuel Magalhaes Silva de Almeida
- Tatiana Sagula Machado Dias
- Victor Hugo Rocha Macedo

---

# Etapa 2 - Formatação da Planilha

## 2.1 Formatação como Tabela

Aplicar o estilo:

```text
Branco - Estilo de Tabela Clara 8
```

Com a opção:

```text
Minha tabela tem cabeçalho
```

habilitada.

---

## 2.2 Alinhamento

Aplicar em toda a tabela:

- Centralização horizontal
- Centralização vertical
- Quebra automática de texto

---

## 2.3 Ajustes de Layout

### Altura das Linhas

```text
120
```

### Largura das Colunas

Ajustar entre:

```text
16 e 20
```

podendo haver tratamentos específicos para colunas com textos maiores.

---

## 2.4 Formatação da Data Fatal

Na coluna **Data fatal**:

```text
DD/MM/AAAA
```

Utilizando a localidade:

```text
Português (Brasil)
```

---

# Etapa 3 - Destaques Visuais

## Cabeçalho da Data Fatal

Colorir apenas a célula do cabeçalho **Data fatal** com:

```text
#C00000
```

---

## D0 (Fatal)

Quando a Data Fatal for a data informada pelo usuário:

```text
#DA9694
```

---

## D-1

Quando a Data Fatal for o dia seguinte:

```text
#FFFF99
```

---

## D-2

Quando a Data Fatal for dois dias após a data informada:

```text
#FABF8F
```

---

# Etapa 4 - Ordenação

Após todos os tratamentos:

1. Remover filtros temporários.
2. Ordenar pela coluna **Data fatal**.
3. Ordenar do prazo mais antigo para o mais recente.

---

# Etapa 5 - Geração das Agendas por Área

Criar novas pastas de trabalho independentes:

```text
Agenda Cível - DD.MM.AAAA
Agenda Trabalhista - DD.MM.AAAA
Agenda Tributária - DD.MM.AAAA
```

Todas devem manter exatamente a mesma formatação aplicada à agenda principal:

- Tabela
- Fontes
- Cores
- Larguras
- Alinhamentos
- Destaques D0, D-1 e D-2

---

# Etapa 6 - Separação por Tipo

Dentro de cada agenda de área, criar abas conforme o conteúdo da coluna **Tipo**.

Possíveis valores:

```text
PRAZO
TAREFA
AUDIENCIA
JULGAMENTO
PERICIA
REUNIAO
LIGACAO
```

---

## Ordem das Abas

As abas devem sempre respeitar a seguinte sequência:

```text
Prazos
Tarefas
Audiências
Julgamentos
Perícias
Reunião
Ligação
Outras
```

Criar somente as abas que possuírem registros.

---

## Regra Especial da Agenda Cível

Na agenda cível:

1. Criar normalmente a aba **Prazos**.
2. Identificar registros cujo Workflow seja:

```text
Acompanhamento
```

3. Remover esses registros da aba **Prazos**.
4. Criar uma aba específica:

```text
Acompanhamentos
```

5. Mover esses registros para a nova aba.

---

# Etapa 7 - Geração da Agenda de Fatais

Criar uma nova pasta de trabalho:

```text
Agenda - Fatais - DD.MM.AAAA
```

---

## Critérios de Seleção

### Data Fatal

Manter apenas registros cuja Data Fatal seja igual à data informada pelo usuário.

---

### Tipo

Manter apenas:

```text
Prazo
```

Excluir todos os demais tipos.

---

### Evento

Manter apenas:

```text
Workflow Principal
```

Excluir:

```text
Etapa de Workflow
```

---

### Workflow

Excluir os seguintes workflows:

- Acompanhamento
- Providências Iniciais
- Publicação Não Capturada pelo EasyJur

---

# Arquivos Gerados

Ao final do processamento, gerar um arquivo ZIP contendo:

```text
Agenda - DD.MM.AAAA.xlsx

Agenda Cível - DD.MM.AAAA.xlsx

Agenda Trabalhista - DD.MM.AAAA.xlsx

Agenda Tributária - DD.MM.AAAA.xlsx

Agenda - Fatais - DD.MM.AAAA.xlsx
```

---

# Auditoria

O sistema registra automaticamente:

- Registros excluídos
- Campos alterados
- Áreas preenchidas automaticamente
- Alertas de inconsistência

Arquivos de auditoria gerados:

```text
Auditoria.xlsx
Resumo_Processamento.txt
```

Todos os arquivos são entregues ao usuário em um único arquivo ZIP.
`