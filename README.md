# Python Style Guide

O Python é uma linguagem de programação altamente flexível, caracterizada por convenções menos rígidas, o que a diferencia de linguagens 
como o Java, que impõe a utilização da orientação a objetos. Esse contraste pode tornar o desenvolvimento mais ágil, porém potencialmente 
desorganizado. Diante desse cenário, criei o presente repositório com o objetivo de promover boas práticas de estilo e organização de 
código, direcionado especialmente para novos desenvolvedores.
<br /><br /><br />



## 📁 Estruturação de diretórios

Ao organizar um projeto Python, você tem grande flexibilidade para criar e manipular vários diretórios. Dentre as possibilidades, é 
comum criar um arquivo contendo vários trechos de código, como classes e funções.

Sem enrolar com a apresentação, a abordagem central que recomendo para a organização de diretórios em seu projeto é a alocação intuitiva
de arquivos e pastas. Como exemplo, para um aplicativo de pequeno porte, é viável agrupar todas as funções de utilidade em um único 
arquivo chamado `utils.py`. À medida que o projeto cresce em escopo, talvez seja necessário transformar esse arquivo em um diretório e, 
dentro dele, criar subpacotes, como:

```
└─ utils/
   ├─ __init__.py
   ├─ data_transfer.py
   ├─ datetime_handler.py
   ├─ jinja_helpers.py
```

Há um arquivo .py que contém trechos de código úteis, cada um deles com um propósito específico. Adicionalmente, existe um arquivo 
`__init__.py` que, neste contexto, foi empregado para converter o diretório em um pacote que pode ser importado externamente,
transformado o diretório em um pacote.

```python
from .data_transfer import dict_to_form, form_to_obj, obj_to_form
from .datetime_handler import format_datetime
from .jinja_helpers import component, layout, render_page
```
<br /><br />


## 📄 Dicas para a codificação


### Responsabilidades de cada pacote

Conforme mencionado no tópico anterior, é sempre vantajoso adotar uma divisão intuitiva dos pacotes em seu projeto. É possível criar 
um arquivo que contenha diversas classes, desde que todas compartilhem o mesmo propósito. Essa mesma filosofia se aplica a variáveis, 
constantes, funções e até mesmo a outros pacotes, como é frequentemente observado no `__init__.py`.
<br /><br />


### Importações

Um ponto que considero bastante relevante é a forma como você organiza as importações ao longo do seu código. Seguindo o padrão PEP8, 
é possível dividir as importações em três categorias:

1. Bibliotecas externas.
2. Bibliotecas internas de outros diretórios.
3. Bibliotecas internas do mesmo diretório.

Ao adotar essa abordagem, você obtém uma lista de importações que é fácil de ler e compreender, como demonstrado no exemplo abaixo:

```python
from flask import Blueprint
from http import HTTPStatus
from typing import Tuple

from app.lib.utils import render_page

from .forms import LoginForm
from .services import find_all_users
```
<br />


### Type Hints

A atribuição de tipos não é uma prática muito comum entre programadores Python. Essa abordagem ganhou destaque apenas a partir de 2015, 
quando foi introduzida a capacidade de incorporar dicas de tipagem estática ao código, conhecida como Type Hints.

Com Type Hints, é possível descrever explicitamente TODOS os tipos de dados usados em sua aplicação, mas esta não é uma prática que eu 
recomendo. Na verdade, ao criar variáveis em Python, é comum que a IDE reconheça automaticamente o tipo apropriado, tornando a tipagem 
estática menos necessária.

O que eu realmente recomendo é a atribuição de tipos em **funções** e **métodos** para especificar os tipos de entrada e saída tratados. 
Isso proporciona maior clareza na documentação do código e faz com que a IDE trate corretamente o tipo dos itens.
<br /><br />

### Atribuição de nomes

Aqueles que cursaram uma faculdade na área de desenvolvimento de software podem ter encontrado a prática de utilizar variáveis com nomes 
de apenas uma letra, como `x = 15`. Embora essa prática não seja incorreta, é importante considerar que ela não é recomendada.

É aconselhável que todos os nomes atribuídos descrevam de forma concisa a funcionalidade ou propósito do item em questão. Não hesite em 
optar por nomes mais longos, desde que sejam descritivos e claros.

Um exemplo ilustrativo dessa abordagem é um pacote voltado para o tratamento de datas, no qual cada item foi nomeado para refletir a sua 
finalidade:

```python
from datetime import date, datetime


TIME_PATTERN = '%I:%M %p'
DATE_PATTERN = '{month} {day}, {year}'
MONTH = [
    'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
    'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
]


def format_datetime(dt: datetime, reference_date: date) -> str:
    formatted_dt = ''

    if dt.date() == reference_date:
        formatted_dt = dt.strftime(TIME_PATTERN)
    else:
        month_idx = dt.month - 1
        formatted_dt = DATE_PATTERN.format(
            month=MONTH[month_idx], day=dt.day, year=dt.year
        )

    return formatted_dt
```
<br /><br />


## 📓 Considerações adicionais

Os trechos de código que apresentei foram extraídos do projeto [Flask Arch](https://github.com/davidsantana06/flask-arch) e podem servir 
como exemplos concretos de cada conceito que abordei anteriormente. Optei por não incluir discussões sobre design patterns ou atribuições 
complexas, focando apenas em proporcionar uma visão resumida e simplificada de conceitos que podem contribuir significativamente para tornar 
o seu código mais limpo e organizado. Essas práticas facilitam o desenvolvimento, a leitura e a manutenção do código, tornando todo o 
processo mais coerente e até mesmo divertido, especialmente para aqueles que apreciam programação.