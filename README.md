# 📊 Análise de Escola com Power BI

O objetivo desta análise foi visualizar o rendimento acadêmico dos alunos, utilizando dados retirados de um banco de dados MySQL conectado ao Power BI.

O dashboard foi construído com base nos dados tratados dentro do banco de dados, o que permite visualizar indicadores de aprovação, reprovação e notas.

## 1. Banco de Dados
### 1.1 Estrutura Geral

O banco de dados segue uma estrutura relacional básica, com o relacionamento das tabelas sendo feito por uma tabela de IDs gerais.

**Tabela: all_id**

Contém os IDs de cada tabela.

    id_principal (PK)
    pais_id      (FK → pais)
    escola_id    (FK → escola)
    estudante_id (FK → alunos)


**Tabela: alunos**

Contém informações dos alunos e suas notas.

    estudante_id         (PK)
    idade
    gênero               (ENUM: Menino, Menina)
    estudo_semanal_horas (DECIMAL 4,2)
    faltas
    gpa                  (DECIMAL 2,1)
    situacao             (ENUM: Aprovado, Reprovado, Recuperação)

**Tabela: escola**

Contém informações sobre a escola, como aulas extracurriculares e nota final dos alunos.

    escola_id          (PK)
    nota_final         (ENUM: A+, A, A-, B+ … F)
    aulas_particulares (ENUM: Sim, Não)
    extra_curricular   (ENUM: Sim, Não)
    esportes           (ENUM: Sim, Não)
    musica             (ENUM: Sim, Não)
    voluntario         (ENUM: Sim, Não)
    estudante_id       (FK → alunos)

**Tabela: pais**

Contém informações sobre os pais dos alunos, como o nível de escolaridade.

    pais_id            (PK)
    educacao_parental  (ENUM: Incompleto, Ensino Fundamental, Ensino Médio … Pós-graduação)
    estudante_id       (FK → alunos)

### 1.2 Regras de Negócio

- A situação do aluno é baseada na coluna gpa (0.0 a 4.0).

- Conversão dos valores numéricos para letras (exemplo: 4.0 = A+, ≥ 3.7 = A).

- Critérios:

    - **Aprovado**: ≥ 2.0

    - **Recuperação**: ≥ 1.0 e < 2.0

    - **Reprovado**: < 1.0

## 2. ETL e Dados

- **Fonte**: base de dados retirada do site Kaggle.

- **Limpeza aplicada**:

    - Remoção de colunas desnecessárias.

    - Alteração de valores numéricos para strings (exemplo: 1 = Sim, 0 = Não).

    - Padronização de valores de ponto flutuante (a coluna estudo_semanal_horas possuía até 14 casas decimais e gpa até 15).

    - Adição das colunas nota_final e situacao por meio de UPDATE CASE baseado nos valores da coluna gpa.

- Separação dos dados: feita em três tabelas.

- Inserção dos dados: realizada via LOAD DATA INFILE.

## 3. Dashboard
### 3.1 Ferramenta

O dashboard foi criado no **Power BI**.

### 3.2 Conteúdo do Dashboard

O dashboard possui apenas **1 página e 2 dicas de ferramentas**.

**Elementos incluídos**:

- Segmentação de dados: Esportes, Extra Curricular, Voluntário, Música, Aulas Particulares.

- Tabela com a porcentagem de alunos **Aprovados, em Recuperação e Reprovados**.

- Tabela com a quantidade de alunos por cada nota.

- Gráfico de linhas com a média de **GPA e tempo de estudo** entre meninos e meninas.

- Gráfico de linhas com o total de faltas em cada nota.

- Gráfico de linhas com a média de estudos em cada nota + dica de ferramenta com gráfico de pizza indicando a média de estudo por idade.

- Gráfico de área com a média de GPA por idade.

- Gráfico de pizza com a quantidade de alunos (meninos × meninas) + dica de ferramenta com cartão de total de faltas.

## 4. Futuras Melhorias

- Inclusão de frequência parental nos estudos dos alunos.

- Exportação automática em PDF.
