# Criando Views no Firebird

## Pré-requisitos
Conhecimentos necessários:
* SQL
* Funções
* SGBD (opcional)
  
Dica: Alguns códigos podem ser difícil de entender, em um SGBD ou editor de código selecione os parenteses para entender onde começa e termina cada função e etc.

<br>

## Criando VIEW com UNION ALL

```sql
--cria a view CLI_VW_MES_FINDOCREC
CREATE
OR ALTER VIEW CLI_VW_MES_FINDOCREC(
    EMP_COD,
    CLI_COD,
    MES,
    ANO,
    SALDO
) AS

--seleciona colunas da tabela FINDOCRECEBER
SELECT a.EMP_COD,
    a.CLI_COD,
    EXTRACT( --extrai o mês de a.VENCTORIG
        MONTH
        FROM a.VENCTORIG
    ),
    EXTRACT( --extrai o ano de a.VENCTORIG
        YEAR
        FROM a.VENCTORIG
    ),
    a.SALDO
FROM FINDOCRECEBER a
WHERE(BAIXA IS NULL)
    AND(FAT_NUM = '')
    AND(VENCTO >= CURRENT_DATE)

--une o select anterior com o próximo select da tabela FINCHEQUESPRE
UNION ALL
SELECT b.EMP_COD,
    b.CLI_COD,
    EXTRACT(
        MONTH
        FROM b.CHQ_VENCTO
    ),
    EXTRACT(
        YEAR
        FROM b.CHQ_VENCTO
    ),
    b.CHQ_SALDO
FROM FINCHEQUESPRE b
WHERE(b.CHQ_DATABAI IS NULL)
    AND(CHQ_VENCTO >= CURRENT_DATE);
```

<br>

## Criando VIEW com dois LEFT OUTER JOIN

```sql
--cria a view VEN_VW_MES_MES
CREATE
OR ALTER VIEW VEN_VW_MES_MES(
    EMP_COD,
    CLI_COD,
    VEN_COD1,
    SUPERVISOR,
    MES,
    ANO,
    NFS_VALBRUT,
    NFS_CUSTO,
    NFS_TS
) AS

--seleciona as seguinprotes colunas da tabela PRONFSAIDA
SELECT nfs.EMP_COD,
    nfs.CLI_COD,
    nfs.VEN_COD1,
    nfs.SUPERVISOR,
    Extract(
        MONTH
        From nfs.NFS_EMISSAO
    ) AS MES,
    Extract(
        YEAR
        From NFS_EMISSAO
    ) AS ANO,
    (
        nfs.NFS_VALBRUT - nfs.NFS_DEVOLUCAO
    ),
    nfs.NFS_CUSTO,
    nfs.NFS_TS
FROM PRONFSAIDA nfs

--faz LEFT OUTER JOIN com a tabela CLICLIENTESEND
    LEFT OUTER JOIN CLICLIENTESEND cliendereco ON (
        nfs.EMP_COD = cliendereco.EMP_COD
    )
    AND (
        nfs.CLI_COD = cliendereco.CLI_COD
    )
    AND (
        nfs.TIT_COD_ENT = cliendereco.TIT_COD
    )

--faz LEFT OUTER JOIN com a tabela PROTES
    LEFT OUTER JOIN PROTES tes ON (nfs.NFS_TS = tes.TES_COD)
WHERE (nfs.NFS_STATUS = 'F')
    AND (nfs.NFS_ORC <> 'B')
    AND (nfs.NFS_TIPO = '0')
    AND (tes.TES_SUMVENDA = 1);
```

<br>

## Criando View com coluna do tipo DATE

```sql
--cria a view VW_AFV_AVISO
CREATE
OR ALTER VIEW VW_AFV_AVISO(
    ID_FILIAL,
    DESCRICAO,
    DT_INICIO,
    DT_FIM,
    TIPO
) AS

--seleciona as seguintes colunas da tabela VENPALMTABELAS
SELECT ven.EMP_COD,
    ven.TAB_DESCRICAO,
    CAST( --converte um valor para algum tipo

        EXTRACT( --extrai o ano de ven.DAT_ALT
            YEAR
            FROM ven.DAT_ALT
        ) || '-' || --concatena com LPAD

        LPAD( --preenche o valor na esquerda baseado nos parametros
            EXTRACT( --extrai o mês de ven.DAT_ALT | param[0] do LPAD
                MONTH
                FROM ven.DAT_ALT
            ),
            2, --numero de casas | param[1] do LPAD
            0  --o valor usado para ser preenchido | param[2] do LPAD
        ) || '-' ||

        LPAD(
            EXTRACT(
                DAY
                FROM ven.DAT_ALT
            ),
            2,
            0
        ) AS VARCHAR(10)
    ) || ' 00:00:00',
    '',
    0
FROM VENPALMTABELAS ven;
```

<br>

## Criando VIEW, usando CASE e fazendo JOIN e LEFT JOIN

```sql
--criando view VW_AFV_CAMPANHA_BENEFICIO
CREATE
OR ALTER VIEW VW_AFV_CAMPANHA_BENEFICIO(
    ID_FILIAL,
    ID_RETAGUARDA,
    ID_CAMPANHA,
    TIPO,
    CODIGO,
    QUANTIDADE,
    PERCENTUAL_DESCONTO,
    DESCONTO_AUTOMATICO,
    BONIFICACAO_AUTOMATICA,
    STATUS
) AS

--selecione as seguintes colunas da tabela PROCAMPANHABENEF
SELECT benef.emp_cod,
    benef.camp_ben_cod,
    benef.camp_cod,
    benef.camp_ben_tipo,
    CASE --CASE Statement
      --quando                       então
        WHEN benef.camp_ben_tipo = 2 THEN prod.pro_reduzi
        WHEN benef.camp_ben_tipo = 1 THEN benef.camp_ben_tipo_cod_n1 || '.' || benef.camp_ben_tipo_cod_n2
      --se nao
        ELSE benef.camp_BEN_tipo_cod_n1
    END, --fim do CASE
    benef.camp_ben_qtd,
    benef.camp_ben_per_descto,
    benef.camp_ben_aplica_desc_auto,
    benef.camp_ben_aplica_bon_auto,
    benef.camp_ben_status
FROM PROCAMPANHABENEF benef

    --faz um JOIN com a view VW_AFV_CAMPANHA_CABECALHO
    JOIN VW_AFV_CAMPANHA_CABECALHO cab on cab.id_filial = benef.emp_cod
    AND cab.id_retaguarda = benef.camp_cod

    --faz um LEFT JOIN com a tabela PROPRODUTOS
    LEFT JOIN PROPRODUTOS prod ON prod.EMP_COd = benef.EMP_COD
    AND prod.grupo_cod = benef.camp_ben_tipo_cod_n1
    AND prod.subgrupo_cod = benef.camp_ben_tipo_cod_n2
    AND prod.pro_cod = benef.camp_ben_tipo_cod_n3
WHERE benef.emp_cod in ('1', '3');
```

<br>

## Criando VIEW e usando as funções TRIM e REPLACE

```sql

```