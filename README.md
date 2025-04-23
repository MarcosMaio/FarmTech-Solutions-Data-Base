# FarmTech-Solutions-Data-Base – Modelo Entidade-Relacionamento (MER)

## Visão Geral
Este documento apresenta o modelo entidade-relacionamento (MER) para o banco de dados da fase 2 do projeto FarmTech, detalhando entidades, atributos e relacionamentos necessários para suportar o gerenciamento de culturas, áreas de plantio, sensores e coletas de dados ambientais.  
Fonte: Documentação FarmTech (MER) :contentReference[oaicite:0]{index=0}&#8203;:contentReference[oaicite:1]{index=1}

---

## Entidades

### Culturas
- **Chave Primária:** `ID_cultura`  
- **Resumo:** Armazena dados referentes à aquisição de culturas para plantio futuro.  
| Coluna         | Tipo             | Descrição                                             | Obrigatório |
|----------------|------------------|-------------------------------------------------------|-------------|
| `ID_cultura`   | INTEGER          | Código de registro único da cultura adquirida         | Sim         |
| `Nome_cultura` | VARCHAR(50)      | Nome da cultura adquirida (ex.: café, cana, milho…)   | Sim         |
| `Quantidade`   | DECIMAL(10,2)    | Quantidade adquirida da cultura para plantio         | Sim         |
| `Unidade`      | VARCHAR(20)      | Unidade de medida da cultura (grão, Kg, Toneladas)    | Não         |

### Áreas de Plantio
- **Chave Primária:** `ID_area`  
- **Resumo:** Registra as áreas disponíveis e suas dimensões.  
| Coluna      | Tipo             | Descrição                                               | Obrigatório |
|-------------|------------------|---------------------------------------------------------|-------------|
| `ID_area`   | INTEGER          | Código de registro único da área de plantio             | Sim         |
| `Geometria` | VARCHAR(50)      | Formato descritivo da área (Retângulo, Triângulo…)      | Sim         |
| `Base`      | DECIMAL(10,2)    | Valor numérico da base                                  | Sim         |
| `Altura`    | DECIMAL(10,2)    | Valor numérico da altura                                | Sim         |
| `Area`      | DECIMAL(10,2)    | Cálculo da área total dessa local                       | Não         |
| `Unidade`   | VARCHAR(20)      | Unidade das métricas de base e altura (cm, m², …)       | Sim         |

### Cultivos
- **Chave Primária:** `ID_cultivo`  
- **Chaves Estrangeiras:** `ID_area` → Áreas de Plantio, `ID_cultura` → Culturas  
- **Resumo:** Registra quais culturas estão sendo cultivadas em quais áreas e em que quantidade.  
| Coluna               | Tipo             | Descrição                                               | Obrigatório |
|----------------------|------------------|---------------------------------------------------------|-------------|
| `ID_cultivo`         | INTEGER          | Código de registro único do cultivo                     | Sim         |
| `ID_area`            | INTEGER          | Código da área em que está sendo cultivado              | Sim         |
| `ID_cultura`         | INTEGER          | Código da cultura cultivada nessa área                  | Sim         |
| `Quantidade_plantio` | DECIMAL(10,2)    | Quantidade de cultura utilizada para o plantio         | Sim         |
| `Unidade`            | VARCHAR(20)      | Unidade da cultura cultivada (grão, Kg, Toneladas)      | Sim         |

### Quadrantes
- **Chave Primária:** `ID_quadra`  
- **Chaves Estrangeiras:** `ID_area` → Áreas de Plantio, `ID_cultivo` → Cultivos  
- **Resumo:** Divide um cultivo em sub-áreas para maior precisão nas métricas de monitoramento.  
| Coluna             | Tipo             | Descrição                                                     | Obrigatório |
|--------------------|------------------|---------------------------------------------------------------|-------------|
| `ID_quadra`        | INTEGER          | Código de registro único do quadrante                         | Sim         |
| `ID_area`          | INTEGER          | Código da área em que está o quadrante                        | Sim         |
| `ID_cultivo`       | INTEGER          | Código da cultura cultivada nesse quadrante                   | Sim         |
| `Descricao_quadra` | VARCHAR(100)     | Descrição livre do quadrante                                  | Não         |
| `Acesso`           | VARCHAR(100)     | Método de acesso ao quadrante (ex.: pelas ruas X e Y)         | Não         |

### Produtos
- **Chave Primária:** `ID_produto`  
- **Resumo:** Armazena produtos adquiridos para uso no plantio e manutenção.  
| Coluna               | Tipo             | Descrição                                           | Obrigatório |
|----------------------|------------------|-----------------------------------------------------|-------------|
| `ID_produto`         | INTEGER          | Código de registro único do produto                 | Sim         |
| `Nome_produto`       | VARCHAR(100)     | Nome do produto adquirido                           | Sim         |
| `Marca`              | VARCHAR(100)     | Marca do produto                                    | Sim         |
| `Modelo`             | VARCHAR(100)     | Modelo do produto                                   | Sim         |
| `Quantidade`         | INTEGER          | Quantidade adquirida/possuída                       | Sim         |
| `Responsável_Técnico`| VARCHAR(100)     | Responsável técnico pelo produto                    | Sim         |
| `Observação`         | VARCHAR(100)     | Anotações ou observações adicionais                 | Não         |

### Sensores
- **Chave Primária:** `ID_sensor`  
- **Chaves Estrangeiras:** `ID_produto` → Produtos, `ID_area` → Áreas de Plantio, `ID_quadra` → Quadrantes  
- **Resumo:** Controle patrimonial e de manutenção de cada unidade de sensor instalada.  
| Coluna          | Tipo          | Descrição                                                     | Obrigatório |
|-----------------|---------------|---------------------------------------------------------------|-------------|
| `ID_sensor`     | INTEGER       | Código de registro único do sensor                            | Sim         |
| `ID_produto`    | INTEGER       | Código do produto associado ao sensor                         | Sim         |
| `ID_area`       | INTEGER       | Código da área de plantio onde está instalado                 | Sim         |
| `ID_quadra`     | INTEGER       | Código do quadrante onde o sensor está instalado              | Sim         |
| `Dt_aquisicao`  | DATE          | Data e hora de cadastro do sensor                             | Sim         |
| `Dt_manutencao` | DATE          | Data da última manutenção realizada                           | Não         |

### Coletas de Umidade
- **Chave Primária:** `ID_coleta_umidade`  
- **Chaves Estrangeiras:** `ID_sensor`, `ID_cultura`, `ID_area`, `ID_quadra`  
- **Resumo:** Logs de coletas de umidade e temperatura do solo.  
| Coluna       | Tipo             | Descrição                                             | Obrigatório |
|--------------|------------------|-------------------------------------------------------|-------------|
| `ID_coleta_umidade` | INTEGER    | Código de registro único da coleta                   | Sim         |
| `ID_sensor`         | INTEGER    | Sensor responsável pela coleta                       | Sim         |
| `ID_cultura`        | INTEGER    | Cultura relacionada à coleta                         | Sim         |
| `ID_area`           | INTEGER    | Área relacionada à coleta                            | Sim         |
| `ID_quadra`         | INTEGER    | Quadrante relacionado à coleta                       | Sim         |
| `Dt_log`            | DATE       | Data da coleta                                       | Sim         |
| `Tipo_desc`         | VARCHAR(100)| Descrição ou observação                              | Não         |
| `Umidade`           | DECIMAL(10,2)| Umidade do solo coletada                            | Sim         |
| `Temperatura`       | DECIMAL(10,2)| Temperatura do solo coletada                        | Sim         |
| `Unidades`          | VARCHAR(150)| Unidades de medida dos registros                    | Sim         |

### Coletas de pH
- **Chave Primária:** `ID_coleta_ph`  
- **Chaves Estrangeiras:** `ID_sensor`, `ID_cultura`, `ID_area`, `ID_quadra`  
- **Resumo:** Logs de coletas de pH e outras métricas do solo.  
| Coluna         | Tipo             | Descrição                                               | Obrigatório |
|----------------|------------------|---------------------------------------------------------|-------------|
| `ID_coleta_ph` | INTEGER          | Código de registro único da coleta                      | Sim         |
| `ID_sensor`    | INTEGER          | Sensor responsável pela coleta                          | Sim         |
| `ID_cultura`   | INTEGER          | Cultura relacionada à coleta                            | Sim         |
| `ID_area`      | INTEGER          | Área relacionada à coleta                               | Sim         |
| `ID_quadra`    | INTEGER          | Quadrante relacionado à coleta                          | Sim         |
| `Dt_log`       | DATE             | Data da coleta                                          | Sim         |
| `Tipo_desc`    | VARCHAR(100)     | Descrição ou observação                                 | Não         |
| `Ph`           | DECIMAL(10,2)    | Nível de pH registrado                                  | Sim         |
| `Temperatura`  | DECIMAL(10,2)    | Temperatura no momento da coleta                        | Sim         |
| `NTU`          | DECIMAL(10,2)    | Métrica de turbidez (NTU)                               | Sim         |
| `Condutividade`| DECIMAL(10,2)    | Condutividade elétrica registrada                       | Sim         |
| `Unidades`     | VARCHAR(150)     | Unidades de medida dos registros                       | Sim         |

### Coletas de NPK
- **Chave Primária:** `ID_coleta_nutriente`  
- **Chaves Estrangeiras:** `ID_sensor`, `ID_cultura`, `ID_area`, `ID_quadra`  
- **Resumo:** Logs de coletas de elementos químicos (N, P, K) do solo.  
| Coluna                 | Tipo             | Descrição                                                  | Obrigatório |
|------------------------|------------------|------------------------------------------------------------|-------------|
| `ID_coleta_nutriente`  | INTEGER          | Código de registro único da coleta                        | Sim         |
| `ID_sensor`            | INTEGER          | Sensor responsável pela coleta                            | Sim         |
| `ID_cultura`           | INTEGER          | Cultura relacionada à coleta                              | Sim         |
| `ID_area`              | INTEGER          | Área relacionada à coleta                                 | Sim         |
| `ID_quadra`            | INTEGER          | Quadrante relacionado à coleta                            | Sim         |
| `Dt_log`               | DATE             | Data da coleta                                            | Sim         |
| `Tipo_desc`            | VARCHAR(100)     | Descrição ou observação                                   | Não         |
| `Nitrogenio`           | DECIMAL(10,2)    | Nível de nitrogênio registrado                            | Sim         |
| `Fosforo`              | DECIMAL(10,2)    | Nível de fósforo registrado                               | Sim         |
| `Potassio`             | DECIMAL(10,2)    | Nível de potássio registrado                              | Sim         |
| `Unidades`             | VARCHAR(150)     | Unidades de medida dos registros                          | Sim         |

---

## Relacionamentos

| Entidades Envolvidas      | Relacionamento     | Cardinalidade    | Descrição                                                                        |
|---------------------------|--------------------|------------------|----------------------------------------------------------------------------------|
| Culturas → Cultivos       | Cultiva            | 1:0..N           | Uma cultura pode ser cultivada várias vezes. Cada cultivo deve referenciar uma cultura.          |
| Áreas_plantio → Cultivos  | Ocupa              | 1:0..N           | Uma área de plantio pode conter diversos cultivos ao longo do tempo.                              |
| Cultivos → Quadrantes     | Pertence           | 1:1..N           | Um cultivo pode ser dividido em vários quadrantes.                                               |
| Áreas_plantio → Quadrantes| Contém             | 1:1..N           | Todo quadrante deve estar em uma área. Uma área possui um ou mais quadrantes.                   |
| Produtos → Sensores       | Pode conter        | 1:0..N           | Um produto pode ter vários sensores associados.                                                    |
| Áreas_plantio → Sensores  | Está alocado       | 1:0..N           | Sensores precisam estar em uma área, mas uma área não precisa ter sensores.                       |
| Quadrantes → Sensores     | Posicionado em     | 0..1:0..N        | Sensores podem ou não estar posicionados em um quadrante.                                         |
| Sensores → Umidade_coleta | Coleta_umidade     | 1:0..N           | Um sensor pode gerar várias coletas de umidade.                                                  |
| Sensores → PH_coleta      | Coleta_pH         | 1:0..N           | Um sensor pode gerar várias coletas de pH.                                                         |
| Sensores → NPK_coleta     | Coleta_nutriente  | 1:0..N           | Um sensor pode gerar várias coletas de NPK.                                                        |
# FarmTech-Solutions-Data-Base
