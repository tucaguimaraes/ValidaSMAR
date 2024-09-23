### ValidaSMAR - Algoritmo de Validação de Dados da SMAR do Jovem de Futuro - Instituto Unibanco

O **ValidaSMAR** é um algoritmo corrobora para assegurar a qualidade dos dados no Sistema de Monitoramento e Avaliação de Resultados (SMAR) do programa JF. Ele busca melhorar a integridade dos dados educacionais, reduzindo o retrabalho e aumentando a confiabilidade das análises.

#### Etapas do ValidaSMAR
1. Instalação de pacotes
2. Carregamento dos dados
3. Pergunta 1: Existem dados de Notas do INEP: 22014306, 22140115, 22014470, 22015221?
4. Pergunta 2:Existem dados de para as turmas dessas escolas?
5. Não possuem dados para as turmas dessas escolas.

- By: Carlos Artur Guimarães - Análise de Dados 

# 1) Instalação e Importação de pacotes


```python
!pip install missingno
```

    Collecting missingno
      Using cached missingno-0.5.2-py3-none-any.whl.metadata (639 bytes)
    Requirement already satisfied: numpy in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from missingno) (1.22.4)
    Requirement already satisfied: matplotlib in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from missingno) (3.8.4)
    Requirement already satisfied: scipy in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from missingno) (1.14.0)
    Requirement already satisfied: seaborn in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from missingno) (0.13.2)
    Requirement already satisfied: contourpy>=1.0.1 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from matplotlib->missingno) (1.2.1)
    Requirement already satisfied: cycler>=0.10 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from matplotlib->missingno) (0.12.1)
    Requirement already satisfied: fonttools>=4.22.0 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from matplotlib->missingno) (4.53.1)
    Requirement already satisfied: kiwisolver>=1.3.1 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from matplotlib->missingno) (1.4.5)
    Requirement already satisfied: packaging>=20.0 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from matplotlib->missingno) (21.3)
    Requirement already satisfied: pillow>=8 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from matplotlib->missingno) (10.4.0)
    Requirement already satisfied: pyparsing>=2.3.1 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from matplotlib->missingno) (3.1.2)
    Requirement already satisfied: python-dateutil>=2.7 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from matplotlib->missingno) (2.9.0)
    Collecting numpy (from missingno)
      Using cached numpy-2.1.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (60 kB)
    Requirement already satisfied: pandas>=1.2 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from seaborn->missingno) (2.2.2)
    Requirement already satisfied: pytz>=2020.1 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from pandas>=1.2->seaborn->missingno) (2024.1)
    Requirement already satisfied: tzdata>=2022.7 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from pandas>=1.2->seaborn->missingno) (2024.1)
    Requirement already satisfied: six>=1.5 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from python-dateutil>=2.7->matplotlib->missingno) (1.16.0)
    Using cached missingno-0.5.2-py3-none-any.whl (8.7 kB)
    Using cached numpy-2.1.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (16.3 MB)
    Installing collected packages: numpy, missingno
      Attempting uninstall: numpy
        Found existing installation: numpy 1.22.4
        Uninstalling numpy-1.22.4:
          Successfully uninstalled numpy-1.22.4
    [31mERROR: pip's dependency resolver does not currently take into account all the packages that are installed. This behaviour is the source of the following dependency conflicts.
    mkl-fft 1.3.10 requires mkl, which is not installed.
    hdijupyterutils 0.21.0 requires pandas<2.0.0,>=0.17.1, but you have pandas 2.2.2 which is incompatible.
    numba 0.60.0 requires numpy<2.1,>=1.22, but you have numpy 2.1.1 which is incompatible.
    sagemaker 2.231.0 requires numpy<2.0,>=1.9.0, but you have numpy 2.1.1 which is incompatible.
    sparkmagic 0.21.0 requires pandas<2.0.0,>=0.17.1, but you have pandas 2.2.2 which is incompatible.[0m[31m
    [0mSuccessfully installed missingno-0.5.2 numpy-2.1.1



```python
pip install --upgrade s3fs
```

    Requirement already satisfied: s3fs in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (0.4.2)
    Collecting s3fs
      Downloading s3fs-2024.9.0-py3-none-any.whl.metadata (1.6 kB)
    Collecting aiobotocore<3.0.0,>=2.5.4 (from s3fs)
      Downloading aiobotocore-2.15.1-py3-none-any.whl.metadata (23 kB)
    Collecting fsspec==2024.9.0.* (from s3fs)
      Downloading fsspec-2024.9.0-py3-none-any.whl.metadata (11 kB)
    Requirement already satisfied: aiohttp!=4.0.0a0,!=4.0.0a1 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from s3fs) (3.9.5)
    Requirement already satisfied: botocore<1.35.24,>=1.35.16 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from aiobotocore<3.0.0,>=2.5.4->s3fs) (1.35.16)
    Requirement already satisfied: wrapt<2.0.0,>=1.10.10 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from aiobotocore<3.0.0,>=2.5.4->s3fs) (1.16.0)
    Collecting aioitertools<1.0.0,>=0.5.1 (from aiobotocore<3.0.0,>=2.5.4->s3fs)
      Downloading aioitertools-0.12.0-py3-none-any.whl.metadata (3.8 kB)
    Requirement already satisfied: aiosignal>=1.1.2 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from aiohttp!=4.0.0a0,!=4.0.0a1->s3fs) (1.3.1)
    Requirement already satisfied: attrs>=17.3.0 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from aiohttp!=4.0.0a0,!=4.0.0a1->s3fs) (23.2.0)
    Requirement already satisfied: frozenlist>=1.1.1 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from aiohttp!=4.0.0a0,!=4.0.0a1->s3fs) (1.4.1)
    Requirement already satisfied: multidict<7.0,>=4.5 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from aiohttp!=4.0.0a0,!=4.0.0a1->s3fs) (6.0.5)
    Requirement already satisfied: yarl<2.0,>=1.0 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from aiohttp!=4.0.0a0,!=4.0.0a1->s3fs) (1.9.4)
    Requirement already satisfied: async-timeout<5.0,>=4.0 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from aiohttp!=4.0.0a0,!=4.0.0a1->s3fs) (4.0.3)
    Requirement already satisfied: jmespath<2.0.0,>=0.7.1 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from botocore<1.35.24,>=1.35.16->aiobotocore<3.0.0,>=2.5.4->s3fs) (1.0.1)
    Requirement already satisfied: python-dateutil<3.0.0,>=2.1 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from botocore<1.35.24,>=1.35.16->aiobotocore<3.0.0,>=2.5.4->s3fs) (2.9.0)
    Requirement already satisfied: urllib3!=2.2.0,<3,>=1.25.4 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from botocore<1.35.24,>=1.35.16->aiobotocore<3.0.0,>=2.5.4->s3fs) (2.2.2)
    Requirement already satisfied: idna>=2.0 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from yarl<2.0,>=1.0->aiohttp!=4.0.0a0,!=4.0.0a1->s3fs) (3.7)
    Requirement already satisfied: six>=1.5 in /home/ec2-user/anaconda3/envs/python3/lib/python3.10/site-packages (from python-dateutil<3.0.0,>=2.1->botocore<1.35.24,>=1.35.16->aiobotocore<3.0.0,>=2.5.4->s3fs) (1.16.0)
    Downloading s3fs-2024.9.0-py3-none-any.whl (29 kB)
    Downloading fsspec-2024.9.0-py3-none-any.whl (179 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m179.3/179.3 kB[0m [31m18.4 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading aiobotocore-2.15.1-py3-none-any.whl (77 kB)
    [2K   [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m77.4/77.4 kB[0m [31m14.1 MB/s[0m eta [36m0:00:00[0m
    [?25hDownloading aioitertools-0.12.0-py3-none-any.whl (24 kB)
    Installing collected packages: fsspec, aioitertools, aiobotocore, s3fs
      Attempting uninstall: fsspec
        Found existing installation: fsspec 2024.6.1
        Uninstalling fsspec-2024.6.1:
          Successfully uninstalled fsspec-2024.6.1
      Attempting uninstall: s3fs
        Found existing installation: s3fs 0.4.2
        Uninstalling s3fs-0.4.2:
          Successfully uninstalled s3fs-0.4.2
    Successfully installed aiobotocore-2.15.1 aioitertools-0.12.0 fsspec-2024.9.0 s3fs-2024.9.0
    Note: you may need to restart the kernel to use updated packages.



```python
# Importar pacotes necessários
import pandas as pd  # Para leitura de CSV e manipulação de DataFrames
import pyarrow.parquet as pq  # Para leitura de arquivos Parquet
```

# 2) Carregamento das bases


```python
# Carregamento das bases da SMAR PI 
aulas = pd.read_csv("s3://sgpiu-sftp/FTP/FTP_PI/SMAR/2024/iu_aula_202409101346.csv", 
                    sep=';', encoding='utf-8', on_bad_lines='skip', engine='python')

# Carregar arquivo Parquet refrente a frequência, foi transformado em parquet do original para melhorar o tempo de carregamento.
frequencia = pd.read_parquet("s3://sgpiu-sftp/FTP/FTP_PI/SMAR/2024/iu_frequencia_202409101350.parquet", 
                             engine='pyarrow')

disciplina = pd.read_csv("s3://sgpiu-sftp/FTP/FTP_PI/SMAR/2024/iu_disciplina_202409101347.csv", 
                         sep=';', encoding='utf-8', on_bad_lines='skip', engine='python')

notas = pd.read_csv("s3://sgpiu-sftp/FTP/FTP_PI/SMAR/2024/iu_notas_202409101349.csv", 
                    sep=';', encoding='utf-8', on_bad_lines='skip', engine='python')

turma = pd.read_csv("s3://sgpiu-sftp/FTP/FTP_PI/SMAR/2024/iu_turma_202409101348.csv", 
                    sep=';', encoding='utf-8', on_bad_lines='skip', engine='python')

```

# 3) Análise exploratória


```python
# Criando um dicionário para armazenar informações de cada DataFrame
dfs = {'aulas': aulas, 'frequencia': frequencia, 'disciplina': disciplina, 'notas': notas, 'turma': turma}

# Dicionário para armazenar os resultados da análise
analysis_results = {}

# Função para analisar um DataFrame
def analyze_dataframe(df_name, df):
    missing_values = df.isnull().sum()  # Contagem de valores faltantes por coluna
    data_types = df.dtypes  # Tipos de dados por coluna
    unique_values = df.nunique()  # Contagem de valores únicos por coluna
    
    # Armazenar os resultados no dicionário
    analysis_results[df_name] = {
        'missing_values': missing_values,
        'data_types': data_types,
        'unique_values': unique_values
    }

# Analisando cada DataFrame
for df_name, df in dfs.items():
    analyze_dataframe(df_name, df)

# Exibindo os resultados da análise para cada DataFrame
for df_name, analysis in analysis_results.items():
    print(f"Análise do DataFrame: {df_name}")
    print("Valores faltantes:")
    print(analysis['missing_values'])
    print("\nTipos de dados:")
    print(analysis['data_types'])
    print("\nValores únicos:")
    print(analysis['unique_values'])
    print("-" * 50)

```

    Análise do DataFrame: aulas
    Valores faltantes:
    idaula              0
    idturma             0
    iddisciplina        0
    idtipoaula          0
    horainicio      14125
    horafinal       14126
    dataaula            0
    anoletivo        2974
    dtype: int64
    
    Tipos de dados:
    idaula            int64
    idturma           int64
    iddisciplina      int64
    idtipoaula        int64
    horainicio       object
    horafinal        object
    dataaula         object
    anoletivo       float64
    dtype: object
    
    Valores únicos:
    idaula          2093875
    idturma            4789
    iddisciplina         16
    idtipoaula           10
    horainicio         1055
    horafinal          1125
    dataaula            258
    anoletivo          9699
    dtype: int64
    --------------------------------------------------
    Análise do DataFrame: frequencia
    Valores faltantes:
    inep                             0
    anoletivo                        0
    nomeprofessor                    0
    cpfprofessor                     0
    idturma                          0
    turno                            0
    iddisciplina                     0
    disciplina                       0
    matriculaaluno                   0
    nomealuno                        0
    data                             0
    data2                            0
    idstatusenturmacao               0
    tipofrequencia                   0
    qualificacaofrequencia    41944848
    statusaula                       0
    idaula                           0
    dtype: int64
    
    Tipos de dados:
    inep                        int64
    anoletivo                   int64
    nomeprofessor              object
    cpfprofessor                int64
    idturma                     int64
    turno                      object
    iddisciplina                int64
    disciplina                 object
    matriculaaluno              int64
    nomealuno                  object
    data                       object
    data2                      object
    idstatusenturmacao          int64
    tipofrequencia             object
    qualificacaofrequencia    float64
    statusaula                  int64
    idaula                      int64
    dtype: object
    
    Valores únicos:
    inep                          558
    anoletivo                       1
    nomeprofessor                9266
    cpfprofessor                 9281
    idturma                      4764
    turno                           4
    iddisciplina                   16
    disciplina                     16
    matriculaaluno             146583
    nomealuno                  133858
    data                          212
    data2                         212
    idstatusenturmacao              1
    tipofrequencia                  2
    qualificacaofrequencia          0
    statusaula                      1
    idaula                    1489000
    dtype: int64
    --------------------------------------------------
    Análise do DataFrame: disciplina
    Valores faltantes:
    inep                0
    codigodisciplina    0
    nomedisciplina      0
    mod_ensino          0
    dtype: int64
    
    Tipos de dados:
    inep                 int64
    codigodisciplina     int64
    nomedisciplina      object
    mod_ensino          object
    dtype: object
    
    Valores únicos:
    inep                562
    codigodisciplina     16
    nomedisciplina       16
    mod_ensino           10
    dtype: int64
    --------------------------------------------------
    Análise do DataFrame: notas
    Valores faltantes:
    iddisciplina       0
    idturma            0
    idtiponota         0
    idmatricula        0
    valornota       9780
    dtype: int64
    
    Tipos de dados:
    iddisciplina     int64
    idturma          int64
    idtiponota       int64
    idmatricula      int64
    valornota       object
    dtype: object
    
    Valores únicos:
    iddisciplina       16
    idturma          3278
    idtiponota          4
    idmatricula     87839
    valornota         101
    dtype: int64
    --------------------------------------------------
    Análise do DataFrame: turma
    Valores faltantes:
    inep                           0
    anoletivo                      0
    idturma                        0
    turmafinalizada                0
    nometurma                      0
    modalidadeensino               0
    modalidadeensinocenso          0
    etapa                          0
    turno                          0
    quantidadetotalmatriculados    0
    dtype: int64
    
    Tipos de dados:
    inep                            int64
    anoletivo                       int64
    idturma                         int64
    turmafinalizada                 int64
    nometurma                      object
    modalidadeensino               object
    modalidadeensinocenso          object
    etapa                          object
    turno                          object
    quantidadetotalmatriculados     int64
    dtype: object
    
    Valores únicos:
    inep                            558
    anoletivo                         1
    idturma                        4690
    turmafinalizada                   1
    nometurma                       735
    modalidadeensino                 11
    modalidadeensinocenso             6
    etapa                            32
    turno                             4
    quantidadetotalmatriculados      58
    dtype: int64
    --------------------------------------------------



```python
#dfs = {'aulas': aulas, 'frequencia': frequencia, 'disciplina': disciplina, 'notas': notas, 'turma': turma}
notas.shape
```




    (3903988, 5)




```python
turma.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>inep</th>
      <th>anoletivo</th>
      <th>idturma</th>
      <th>turmafinalizada</th>
      <th>nometurma</th>
      <th>modalidadeensino</th>
      <th>modalidadeensinocenso</th>
      <th>etapa</th>
      <th>turno</th>
      <th>quantidadetotalmatriculados</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22068368</td>
      <td>2024</td>
      <td>282407</td>
      <td>0</td>
      <td>ERFAF-7º ANO EF-M-A</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>7º ANO EF</td>
      <td>MANHÃ</td>
      <td>30</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22068368</td>
      <td>2024</td>
      <td>282408</td>
      <td>0</td>
      <td>ERFAF-7º ANO EF-T-A</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>7º ANO EF</td>
      <td>TARDE</td>
      <td>27</td>
    </tr>
    <tr>
      <th>2</th>
      <td>22068368</td>
      <td>2024</td>
      <td>282409</td>
      <td>0</td>
      <td>ERFAF-8º ANO EF-M-A</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>8º ANO EF</td>
      <td>MANHÃ</td>
      <td>28</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22068368</td>
      <td>2024</td>
      <td>282410</td>
      <td>0</td>
      <td>ERFAF-8º ANO EF-M-B</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>8º ANO EF</td>
      <td>MANHÃ</td>
      <td>27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22028250</td>
      <td>2024</td>
      <td>282411</td>
      <td>0</td>
      <td>ERFAI-5º ANO EF-T-A</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>EDUCAÇÃO REGULAR - ENSINO FUNDAMENTAL</td>
      <td>5º ANO EF</td>
      <td>TARDE</td>
      <td>21</td>
    </tr>
  </tbody>
</table>
</div>




```python
turma.columns
```




    Index(['inep', 'anoletivo', 'idturma', 'turmafinalizada', 'nometurma',
           'modalidadeensino', 'modalidadeensinocenso', 'etapa', 'turno',
           'quantidadetotalmatriculados'],
          dtype='object')




```python
# Supondo que o DataFrame 'turmas' já esteja carregado e contenha a coluna 'inep'
# Lista de INEPs pelos quais queremos filtrar o DataFrame
ineps_procurados = [22014306, 22140115, 22014470, 22015221]

# Filtrando o DataFrame 'turmas' pelos INEPs especificados
turmas_filtradas = turma[turma['inep'].isin(ineps_procurados)]

# Exibindo o resultado filtrado
print(turmas_filtradas)

```

              inep  anoletivo  idturma  turmafinalizada  \
    825   22014306       2024   283564                0   
    827   22014306       2024   283571                0   
    828   22014306       2024   283572                0   
    835   22014470       2024   283581                0   
    838   22014470       2024   283584                0   
    839   22014470       2024   283585                0   
    840   22014470       2024   283587                0   
    843   22014470       2024   283592                0   
    849   22015221       2024   283598                0   
    851   22015221       2024   283600                0   
    854   22015221       2024   283603                0   
    857   22015221       2024   283608                0   
    859   22015221       2024   283611                0   
    860   22015221       2024   283612                0   
    862   22015221       2024   283614                0   
    865   22015221       2024   283618                0   
    867   22015221       2024   283620                0   
    869   22015221       2024   283623                0   
    872   22015221       2024   283626                0   
    876   22015221       2024   283632                0   
    878   22015221       2024   283634                0   
    882   22015221       2024   283638                0   
    885   22015221       2024   283641                0   
    1394  22140115       2024   283983                0   
    1397  22140115       2024   283986                0   
    1399  22140115       2024   283988                0   
    1866  22015221       2024   284193                0   
    3187  22014470       2024   286301                0   
    3197  22014306       2024   286311                0   
    3198  22014306       2024   286312                0   
    3204  22014470       2024   286318                0   
    3205  22014470       2024   286319                0   
    3206  22014470       2024   286320                0   
    3207  22015221       2024   286321                0   
    3208  22015221       2024   286322                0   
    3209  22015221       2024   286323                0   
    3210  22015221       2024   286324                0   
    3211  22015221       2024   286325                0   
    3212  22015221       2024   286326                0   
    3213  22015221       2024   286327                0   
    3214  22015221       2024   286328                0   
    3293  22014306       2024   287482                0   
    3863  22014470       2024   287237                0   
    3864  22015221       2024   287239                0   
    4241  22014470       2024   288222                0   
    4443  22015221       2024   290070                0   
    4445  22014306       2024   290075                0   
    4446  22014306       2024   290076                0   
    4613  22015221       2024   289337                0   
    4636  22015221       2024   289363                0   
    4683  22140115       2024   291744                0   
    
                                              nometurma  \
    825                        EMI 2023INF-2ª SERIE-I-A   
    827                           EMIRINFO-3ª SERIE-M-A   
    828                           EMIRINFO-3ª SERIE-T-A   
    835                        EMI-INTINFO-2ª SERIE-I-A   
    838                        EMI-INTINFO-3ª SERIE-I-A   
    839                        EMIRREC HUM-2ª SERIE-M-A   
    840                        EMIRREC HUM-2ª SERIE-M-B   
    843                        EMIRREC HUM-3ª SERIE-M-A   
    849                       EMIRANA CLIN-2ª SERIE-T-A   
    851                       EMIRANA CLIN-3ª SERIE-T-A   
    854                           EMIRENFE-2ª SERIE-M-A   
    857                          EMIREVENT-2ª SERIE-M-A   
    859                          EMIREVENT-3ª SERIE-M-A   
    860                           EMIRFARM-2ª SERIE-T-A   
    862                           EMIRHOSP-2ª SERIE-T-A   
    865                     EMIRNUTRI DIET-3ª SERIE-T-A   
    867                       EMIRSEG TRAB-2ª SERIE-M-A   
    869                       EMIRREST BAR-2ª SERIE-M-A   
    872                       EMIRREST BAR-3ª SERIE-M-A   
    876                       EMIRTRAN iMO-2ª SERIE-T-A   
    878                    EMIRTRANS IMOBI-3ª SERIE-T-A   
    882                        EMIRVIG SAU-2ª SERIE-M-A   
    885                        EMIRVIG SAU-3ª SERIE-M-A   
    1394                       EMIAAGRONEG-2ª SERIE-I-A   
    1397                        EMIAAGROPE-2ª SERIE-I-A   
    1399                          EMIAZOOT-3ª SERIE-I-A   
    1866             EMIR 2024CT REST BAR-1ª SÉRIE -M-A   
    3187                       EMI-INTADMI-2ª SERIE-I-A   
    3197          EMI-INT 2024CT DES SIST-1ª SÉRIE -I-A   
    3198           EMI-INT 2024CT GUI TUR-1ª SÉRIE -I-A   
    3204          EMI-INT 2024CT DES SIST-1ª SÉRIE -I-B   
    3205           EMI-INT 2024CT GUI TUR-1ª SÉRIE -I-A   
    3206  EMI-INT 2024CT SIST ENERG RENOV-1ª SÉRIE -I-A   
    3207              EMIR 2024CT ANA CLI-1ª SÉRIE -T-A   
    3208              EMIR 2024CT ANA CLI-1ª SÉRIE -T-B   
    3209                  EMIR 2024CT ENF-1ª SÉRIE -M-A   
    3210              EMIR 2024CT GUI TUR-1ª SÉRIE -M-A   
    3211             EMIR 2024CT SEG TRAB-1ª SÉRIE -M-A   
    3212             EMIR 2024CT SEG TRAB-1ª SÉRIE -T-A   
    3213             EMIR 2024CT REST BAR-1ª SÉRIE -T-A   
    3214     EMIR 2024CT SIST ENERG RENOV-1ª SÉRIE -T-A   
    3293          EMI-INT 2024CT DES SIST-1ª SÉRIE -I-B   
    3863          EMI-INT 2024CT DES SIST-1ª SÉRIE -I-A   
    3864                          EMIRENFE-3ª SERIE-M-A   
    4241  EMI-INT 2024CT SIST ENERG RENOV-1ª SÉRIE -I-B   
    4443                      EMIRSEG TRAB-3ª SERIE-M-A   
    4445                       EMI 2023ADM-2ª SERIE-I-A   
    4446                       EMI 2023ADM-2ª SERIE-I-B   
    4613                 EMIRSIS ENER RENO-2ª SERIE-T-A   
    4636                          EMIRHOSP-3ª SERIE-T-A   
    4683                        EMIAAGROPE-1ª SERIE-I-A   
    
                                modalidadeensino  \
    825   ENSINO INTEGRADO INTEGRAL - NOVAS 2023   
    827                 ENSINO INTEGRADO PARCIAL   
    828                 ENSINO INTEGRADO PARCIAL   
    835                ENSINO INTEGRADO INTEGRAL   
    838                ENSINO INTEGRADO INTEGRAL   
    839                 ENSINO INTEGRADO PARCIAL   
    840                 ENSINO INTEGRADO PARCIAL   
    843                 ENSINO INTEGRADO PARCIAL   
    849                 ENSINO INTEGRADO PARCIAL   
    851                 ENSINO INTEGRADO PARCIAL   
    854                 ENSINO INTEGRADO PARCIAL   
    857                 ENSINO INTEGRADO PARCIAL   
    859                 ENSINO INTEGRADO PARCIAL   
    860                 ENSINO INTEGRADO PARCIAL   
    862                 ENSINO INTEGRADO PARCIAL   
    865                 ENSINO INTEGRADO PARCIAL   
    867                 ENSINO INTEGRADO PARCIAL   
    869                 ENSINO INTEGRADO PARCIAL   
    872                 ENSINO INTEGRADO PARCIAL   
    876                 ENSINO INTEGRADO PARCIAL   
    878                 ENSINO INTEGRADO PARCIAL   
    882                 ENSINO INTEGRADO PARCIAL   
    885                 ENSINO INTEGRADO PARCIAL   
    1394    ENSINO INTEGRADO - ALTERNÂNCIA / EFA   
    1397    ENSINO INTEGRADO - ALTERNÂNCIA / EFA   
    1399    ENSINO INTEGRADO - ALTERNÂNCIA / EFA   
    1866         ENSINO INTEGRADO PARCIAL - 2024   
    3187               ENSINO INTEGRADO INTEGRAL   
    3197  ENSINO INTEGRADO INTEGRAL - NOVAS 2024   
    3198  ENSINO INTEGRADO INTEGRAL - NOVAS 2024   
    3204  ENSINO INTEGRADO INTEGRAL - NOVAS 2024   
    3205  ENSINO INTEGRADO INTEGRAL - NOVAS 2024   
    3206  ENSINO INTEGRADO INTEGRAL - NOVAS 2024   
    3207         ENSINO INTEGRADO PARCIAL - 2024   
    3208         ENSINO INTEGRADO PARCIAL - 2024   
    3209         ENSINO INTEGRADO PARCIAL - 2024   
    3210         ENSINO INTEGRADO PARCIAL - 2024   
    3211         ENSINO INTEGRADO PARCIAL - 2024   
    3212         ENSINO INTEGRADO PARCIAL - 2024   
    3213         ENSINO INTEGRADO PARCIAL - 2024   
    3214         ENSINO INTEGRADO PARCIAL - 2024   
    3293  ENSINO INTEGRADO INTEGRAL - NOVAS 2024   
    3863  ENSINO INTEGRADO INTEGRAL - NOVAS 2024   
    3864                ENSINO INTEGRADO PARCIAL   
    4241  ENSINO INTEGRADO INTEGRAL - NOVAS 2024   
    4443                ENSINO INTEGRADO PARCIAL   
    4445  ENSINO INTEGRADO INTEGRAL - NOVAS 2023   
    4446  ENSINO INTEGRADO INTEGRAL - NOVAS 2023   
    4613                ENSINO INTEGRADO PARCIAL   
    4636                ENSINO INTEGRADO PARCIAL   
    4683    ENSINO INTEGRADO - ALTERNÂNCIA / EFA   
    
                               modalidadeensinocenso      etapa     turno  \
    825   EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL   2ª SERIE  INTEGRAL   
    827            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     MANHÃ   
    828            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     TARDE   
    835   EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL   2ª SERIE  INTEGRAL   
    838   EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL   3ª SERIE  INTEGRAL   
    839            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     MANHÃ   
    840            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     MANHÃ   
    843            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     MANHÃ   
    849            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     TARDE   
    851            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     TARDE   
    854            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     MANHÃ   
    857            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     MANHÃ   
    859            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     MANHÃ   
    860            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     TARDE   
    862            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     TARDE   
    865            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     TARDE   
    867            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     MANHÃ   
    869            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     MANHÃ   
    872            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     MANHÃ   
    876            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     TARDE   
    878            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     TARDE   
    882            EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     MANHÃ   
    885            EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     MANHÃ   
    1394           EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE  INTEGRAL   
    1397           EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE  INTEGRAL   
    1399           EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE  INTEGRAL   
    1866           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      MANHÃ   
    3187  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL   2ª SERIE  INTEGRAL   
    3197  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL  1ª SÉRIE   INTEGRAL   
    3198  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL  1ª SÉRIE   INTEGRAL   
    3204  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL  1ª SÉRIE   INTEGRAL   
    3205  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL  1ª SÉRIE   INTEGRAL   
    3206  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL  1ª SÉRIE   INTEGRAL   
    3207           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      TARDE   
    3208           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      TARDE   
    3209           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      MANHÃ   
    3210           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      MANHÃ   
    3211           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      MANHÃ   
    3212           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      TARDE   
    3213           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      TARDE   
    3214           EDUCAÇÃO PROFISSIONAL - INTEGRADA  1ª SÉRIE      TARDE   
    3293  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL  1ª SÉRIE   INTEGRAL   
    3863  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL  1ª SÉRIE   INTEGRAL   
    3864           EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     MANHÃ   
    4241  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL  1ª SÉRIE   INTEGRAL   
    4443           EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     MANHÃ   
    4445  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL   2ª SERIE  INTEGRAL   
    4446  EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL   2ª SERIE  INTEGRAL   
    4613           EDUCAÇÃO PROFISSIONAL - INTEGRADA   2ª SERIE     TARDE   
    4636           EDUCAÇÃO PROFISSIONAL - INTEGRADA   3ª SERIE     TARDE   
    4683           EDUCAÇÃO PROFISSIONAL - INTEGRADA   1ª SERIE  INTEGRAL   
    
          quantidadetotalmatriculados  
    825                            43  
    827                            41  
    828                            39  
    835                            35  
    838                            36  
    839                            25  
    840                            26  
    843                            37  
    849                            32  
    851                            44  
    854                            41  
    857                            33  
    859                            31  
    860                            35  
    862                            16  
    865                            25  
    867                            32  
    869                            38  
    872                            26  
    876                            23  
    878                            25  
    882                            37  
    885                            27  
    1394                           10  
    1397                           15  
    1399                           13  
    1866                           40  
    3187                           20  
    3197                           35  
    3198                           39  
    3204                           39  
    3205                           38  
    3206                           33  
    3207                           40  
    3208                           35  
    3209                           43  
    3210                           38  
    3211                           45  
    3212                           33  
    3213                           23  
    3214                           39  
    3293                           35  
    3863                           36  
    3864                           38  
    4241                           37  
    4443                           30  
    4445                           25  
    4446                           20  
    4613                           29  
    4636                           29  
    4683                           21  



```python
turmas_filtradas.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>inep</th>
      <th>anoletivo</th>
      <th>idturma</th>
      <th>turmafinalizada</th>
      <th>nometurma</th>
      <th>modalidadeensino</th>
      <th>modalidadeensinocenso</th>
      <th>etapa</th>
      <th>turno</th>
      <th>quantidadetotalmatriculados</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>825</th>
      <td>22014306</td>
      <td>2024</td>
      <td>283564</td>
      <td>0</td>
      <td>EMI 2023INF-2ª SERIE-I-A</td>
      <td>ENSINO INTEGRADO INTEGRAL - NOVAS 2023</td>
      <td>EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL</td>
      <td>2ª SERIE</td>
      <td>INTEGRAL</td>
      <td>43</td>
    </tr>
    <tr>
      <th>827</th>
      <td>22014306</td>
      <td>2024</td>
      <td>283571</td>
      <td>0</td>
      <td>EMIRINFO-3ª SERIE-M-A</td>
      <td>ENSINO INTEGRADO PARCIAL</td>
      <td>EDUCAÇÃO PROFISSIONAL - INTEGRADA</td>
      <td>3ª SERIE</td>
      <td>MANHÃ</td>
      <td>41</td>
    </tr>
    <tr>
      <th>828</th>
      <td>22014306</td>
      <td>2024</td>
      <td>283572</td>
      <td>0</td>
      <td>EMIRINFO-3ª SERIE-T-A</td>
      <td>ENSINO INTEGRADO PARCIAL</td>
      <td>EDUCAÇÃO PROFISSIONAL - INTEGRADA</td>
      <td>3ª SERIE</td>
      <td>TARDE</td>
      <td>39</td>
    </tr>
    <tr>
      <th>835</th>
      <td>22014470</td>
      <td>2024</td>
      <td>283581</td>
      <td>0</td>
      <td>EMI-INTINFO-2ª SERIE-I-A</td>
      <td>ENSINO INTEGRADO INTEGRAL</td>
      <td>EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL</td>
      <td>2ª SERIE</td>
      <td>INTEGRAL</td>
      <td>35</td>
    </tr>
    <tr>
      <th>838</th>
      <td>22014470</td>
      <td>2024</td>
      <td>283584</td>
      <td>0</td>
      <td>EMI-INTINFO-3ª SERIE-I-A</td>
      <td>ENSINO INTEGRADO INTEGRAL</td>
      <td>EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL</td>
      <td>3ª SERIE</td>
      <td>INTEGRAL</td>
      <td>36</td>
    </tr>
  </tbody>
</table>
</div>




```python
turmas_filtradas.columns
```




    Index(['inep', 'anoletivo', 'idturma', 'turmafinalizada', 'nometurma',
           'modalidadeensino', 'modalidadeensinocenso', 'etapa', 'turno',
           'quantidadetotalmatriculados'],
          dtype='object')




```python
# Supondo que o DataFrame 'turmas_filtradas' já esteja carregado
# Colunas para análise
colunas_analise = ['inep', 'idturma','turmafinalizada', 'nometurma', 'modalidadeensino', 
                   'modalidadeensinocenso', 'etapa']

# Obter os valores únicos por coluna
valores_unicos_turmas_filtradas = {coluna: turmas_filtradas[coluna].unique() for coluna in colunas_analise}

# Exibir os resultados
for coluna, valores in valores_unicos_turmas_filtradas.items():
    print(f"Coluna: {coluna}")
    print(valores)
    print("-" * 50)

```

    Coluna: inep
    [22014306 22014470 22015221 22140115]
    --------------------------------------------------
    Coluna: idturma
    [283564 283571 283572 283581 283584 283585 283587 283592 283598 283600
     283603 283608 283611 283612 283614 283618 283620 283623 283626 283632
     283634 283638 283641 283983 283986 283988 284193 286301 286311 286312
     286318 286319 286320 286321 286322 286323 286324 286325 286326 286327
     286328 287482 287237 287239 288222 290070 290075 290076 289337 289363
     291744]
    --------------------------------------------------
    Coluna: turmafinalizada
    [0]
    --------------------------------------------------
    Coluna: nometurma
    ['EMI 2023INF-2ª SERIE-I-A' 'EMIRINFO-3ª SERIE-M-A'
     'EMIRINFO-3ª SERIE-T-A' 'EMI-INTINFO-2ª SERIE-I-A'
     'EMI-INTINFO-3ª SERIE-I-A' 'EMIRREC HUM-2ª SERIE-M-A'
     'EMIRREC HUM-2ª SERIE-M-B' 'EMIRREC HUM-3ª SERIE-M-A'
     'EMIRANA CLIN-2ª SERIE-T-A' 'EMIRANA CLIN-3ª SERIE-T-A'
     'EMIRENFE-2ª SERIE-M-A' 'EMIREVENT-2ª SERIE-M-A' 'EMIREVENT-3ª SERIE-M-A'
     'EMIRFARM-2ª SERIE-T-A' 'EMIRHOSP-2ª SERIE-T-A'
     'EMIRNUTRI DIET-3ª SERIE-T-A' 'EMIRSEG TRAB-2ª SERIE-M-A'
     'EMIRREST BAR-2ª SERIE-M-A' 'EMIRREST BAR-3ª SERIE-M-A'
     'EMIRTRAN iMO-2ª SERIE-T-A' 'EMIRTRANS IMOBI-3ª SERIE-T-A'
     'EMIRVIG SAU-2ª SERIE-M-A' 'EMIRVIG SAU-3ª SERIE-M-A'
     'EMIAAGRONEG-2ª SERIE-I-A' 'EMIAAGROPE-2ª SERIE-I-A'
     'EMIAZOOT-3ª SERIE-I-A' 'EMIR 2024CT REST BAR-1ª SÉRIE -M-A'
     'EMI-INTADMI-2ª SERIE-I-A' 'EMI-INT 2024CT DES SIST-1ª SÉRIE -I-A'
     'EMI-INT 2024CT GUI TUR-1ª SÉRIE -I-A'
     'EMI-INT 2024CT DES SIST-1ª SÉRIE -I-B'
     'EMI-INT 2024CT SIST ENERG RENOV-1ª SÉRIE -I-A'
     'EMIR 2024CT ANA CLI-1ª SÉRIE -T-A' 'EMIR 2024CT ANA CLI-1ª SÉRIE -T-B'
     'EMIR 2024CT ENF-1ª SÉRIE -M-A' 'EMIR 2024CT GUI TUR-1ª SÉRIE -M-A'
     'EMIR 2024CT SEG TRAB-1ª SÉRIE -M-A' 'EMIR 2024CT SEG TRAB-1ª SÉRIE -T-A'
     'EMIR 2024CT REST BAR-1ª SÉRIE -T-A'
     'EMIR 2024CT SIST ENERG RENOV-1ª SÉRIE -T-A' 'EMIRENFE-3ª SERIE-M-A'
     'EMI-INT 2024CT SIST ENERG RENOV-1ª SÉRIE -I-B'
     'EMIRSEG TRAB-3ª SERIE-M-A' 'EMI 2023ADM-2ª SERIE-I-A'
     'EMI 2023ADM-2ª SERIE-I-B' 'EMIRSIS ENER RENO-2ª SERIE-T-A'
     'EMIRHOSP-3ª SERIE-T-A' 'EMIAAGROPE-1ª SERIE-I-A']
    --------------------------------------------------
    Coluna: modalidadeensino
    ['ENSINO INTEGRADO INTEGRAL - NOVAS 2023' 'ENSINO INTEGRADO PARCIAL'
     'ENSINO INTEGRADO INTEGRAL' 'ENSINO INTEGRADO - ALTERNÂNCIA / EFA'
     'ENSINO INTEGRADO PARCIAL - 2024'
     'ENSINO INTEGRADO INTEGRAL - NOVAS 2024']
    --------------------------------------------------
    Coluna: modalidadeensinocenso
    ['EDUCAÇÃO PROFISSIONAL - INTEGRADA INTEGRAL'
     'EDUCAÇÃO PROFISSIONAL - INTEGRADA']
    --------------------------------------------------
    Coluna: etapa
    ['2ª SERIE' '3ª SERIE' '1ª SÉRIE ' '1ª SERIE']
    --------------------------------------------------



```python
# Verificar os valores faltantes por coluna no dataframe 'notas'
faltantes_por_coluna = notas.isnull().sum()

# Exibir os valores faltantes por coluna
print(faltantes_por_coluna)

```

    iddisciplina       0
    idturma            0
    idtiponota         0
    idmatricula        0
    valornota       9780
    dtype: int64



```python
notas
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>iddisciplina</th>
      <th>idturma</th>
      <th>idtiponota</th>
      <th>idmatricula</th>
      <th>valornota</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>283304</td>
      <td>350</td>
      <td>1601352</td>
      <td>6,60</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12</td>
      <td>283298</td>
      <td>348</td>
      <td>1601298</td>
      <td>5,00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>285576</td>
      <td>350</td>
      <td>1600615</td>
      <td>7,80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
      <td>285557</td>
      <td>351</td>
      <td>1600387</td>
      <td>9,00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>283302</td>
      <td>350</td>
      <td>1601303</td>
      <td>7,00</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3903983</th>
      <td>2</td>
      <td>284313</td>
      <td>350</td>
      <td>1679623</td>
      <td>6,00</td>
    </tr>
    <tr>
      <th>3903984</th>
      <td>3</td>
      <td>288465</td>
      <td>349</td>
      <td>1881511</td>
      <td>4,50</td>
    </tr>
    <tr>
      <th>3903985</th>
      <td>8</td>
      <td>285079</td>
      <td>348</td>
      <td>1737490</td>
      <td>8,00</td>
    </tr>
    <tr>
      <th>3903986</th>
      <td>3</td>
      <td>287547</td>
      <td>349</td>
      <td>1634946</td>
      <td>7,00</td>
    </tr>
    <tr>
      <th>3903987</th>
      <td>5</td>
      <td>282760</td>
      <td>348</td>
      <td>1759205</td>
      <td>4,00</td>
    </tr>
  </tbody>
</table>
<p>3903988 rows × 5 columns</p>
</div>




```python
notas.columns
```




    Index(['iddisciplina', 'idturma', 'idtiponota', 'idmatricula', 'valornota'], dtype='object')




```python
notas
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>iddisciplina</th>
      <th>idturma</th>
      <th>idtiponota</th>
      <th>idmatricula</th>
      <th>valornota</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>283304</td>
      <td>350</td>
      <td>1601352</td>
      <td>6,60</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12</td>
      <td>283298</td>
      <td>348</td>
      <td>1601298</td>
      <td>5,00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>285576</td>
      <td>350</td>
      <td>1600615</td>
      <td>7,80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
      <td>285557</td>
      <td>351</td>
      <td>1600387</td>
      <td>9,00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>283302</td>
      <td>350</td>
      <td>1601303</td>
      <td>7,00</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3903983</th>
      <td>2</td>
      <td>284313</td>
      <td>350</td>
      <td>1679623</td>
      <td>6,00</td>
    </tr>
    <tr>
      <th>3903984</th>
      <td>3</td>
      <td>288465</td>
      <td>349</td>
      <td>1881511</td>
      <td>4,50</td>
    </tr>
    <tr>
      <th>3903985</th>
      <td>8</td>
      <td>285079</td>
      <td>348</td>
      <td>1737490</td>
      <td>8,00</td>
    </tr>
    <tr>
      <th>3903986</th>
      <td>3</td>
      <td>287547</td>
      <td>349</td>
      <td>1634946</td>
      <td>7,00</td>
    </tr>
    <tr>
      <th>3903987</th>
      <td>5</td>
      <td>282760</td>
      <td>348</td>
      <td>1759205</td>
      <td>4,00</td>
    </tr>
  </tbody>
</table>
<p>3903988 rows × 5 columns</p>
</div>




```python
# Listando valores de 'idturma' 
idturma_valores = [
    283564, 283571, 283572, 283581, 283584, 283585, 283587, 283592, 283598, 283600,
    283603, 283608, 283611, 283612, 283614, 283618, 283620, 283623, 283626, 283632,
    283634, 283638, 283641, 283983, 283986, 283988, 284193, 286301, 286311, 286312,
    286318, 286319, 286320, 286321, 286322, 286323, 286324, 286325, 286326, 286327,
    286328, 287482, 287237, 287239, 288222, 290070, 290075, 290076, 289337, 289363,
    291744
]

# Filtrando 'notas' por 'idturma'
notas_filtradas = notas[notas['idturma'].isin(idturma_valores)]

# Exibindo o resultado filtrado
notas_filtradas
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>iddisciplina</th>
      <th>idturma</th>
      <th>idtiponota</th>
      <th>idmatricula</th>
      <th>valornota</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>




```python
notas
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>iddisciplina</th>
      <th>idturma</th>
      <th>idtiponota</th>
      <th>idmatricula</th>
      <th>valornota</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>283304</td>
      <td>350</td>
      <td>1601352</td>
      <td>6,60</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12</td>
      <td>283298</td>
      <td>348</td>
      <td>1601298</td>
      <td>5,00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>285576</td>
      <td>350</td>
      <td>1600615</td>
      <td>7,80</td>
    </tr>
    <tr>
      <th>3</th>
      <td>8</td>
      <td>285557</td>
      <td>351</td>
      <td>1600387</td>
      <td>9,00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>283302</td>
      <td>350</td>
      <td>1601303</td>
      <td>7,00</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3903983</th>
      <td>2</td>
      <td>284313</td>
      <td>350</td>
      <td>1679623</td>
      <td>6,00</td>
    </tr>
    <tr>
      <th>3903984</th>
      <td>3</td>
      <td>288465</td>
      <td>349</td>
      <td>1881511</td>
      <td>4,50</td>
    </tr>
    <tr>
      <th>3903985</th>
      <td>8</td>
      <td>285079</td>
      <td>348</td>
      <td>1737490</td>
      <td>8,00</td>
    </tr>
    <tr>
      <th>3903986</th>
      <td>3</td>
      <td>287547</td>
      <td>349</td>
      <td>1634946</td>
      <td>7,00</td>
    </tr>
    <tr>
      <th>3903987</th>
      <td>5</td>
      <td>282760</td>
      <td>348</td>
      <td>1759205</td>
      <td>4,00</td>
    </tr>
  </tbody>
</table>
<p>3903988 rows × 5 columns</p>
</div>




```python
# Listando valores de 'idturma' 
idturma_valores = [22137947,22014306,22014535,22015841,
                   22015892,22136002,22162607,22012508,
                   22015159,22014314,22015469,22013601,
                   22015787,22140115,22014470,22015221]

# Filtrando 'notas' por 'idturma'
notas_filtradas = notas[notas['idturma'].isin(idturma_valores)]

# Exibindo o resultado filtrado
notas_filtradas

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>iddisciplina</th>
      <th>idturma</th>
      <th>idtiponota</th>
      <th>idmatricula</th>
      <th>valornota</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>
</div>



## Conclusão 


```python
Não existem dados de notas para essas turmas. Ao se filtrar o inep das escolas e coletar os dados das turmas. Ficam em branco o campo de notas.
```
