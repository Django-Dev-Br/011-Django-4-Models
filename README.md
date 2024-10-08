
# 011 Django 5 - Models

## O que são Models no Django?

Models no Django são classes Python que representam a estrutura e o comportamento dos dados na aplicação. Eles são responsáveis por definir as tabelas, colunas e relacionamentos do banco de dados de maneira abstrata, permitindo que os desenvolvedores interajam com os dados de forma mais intuitiva e consistente. Cada model corresponde a uma tabela no banco de dados, e cada atributo da classe representa um campo nessa tabela.

Os models são a espinha dorsal da arquitetura MTV (Model-Template-View) do Django, onde:

- **Model**: Representa a estrutura de dados da aplicação, definindo como os dados são armazenados e manipulados.
- **Template**: Define como os dados são apresentados ao usuário, usando HTML para a renderização.
- **View**: Controla a lógica que liga o model ao template, processando as requisições e determinando quais dados devem ser apresentados.

Ao usar models, o Django automaticamente cria um banco de dados relacional baseado nessas definições, gerencia migrações para atualizar a estrutura do banco de dados e oferece uma interface de alto nível para interagir com os dados.

## Links Úteis

- **[Django ORM CookBook](https://books.agiliq.com/projects/django-orm-cookbook/en/latest/)**: Um recurso excelente para aprender mais sobre o ORM do Django e como trabalhar eficientemente com modelos e consultas.

- **[DB Browser for SQLite](https://sqlitebrowser.org/)**: Uma ferramenta gráfica para visualizar e manipular bancos de dados SQLite. Útil para inspecionar diretamente a estrutura e os dados do banco de dados criado pelo Django.

- **[Python Orientado a Objetos](https://www.w3schools.com/python/python_classes.asp)**: Um tutorial para entender a sintaxe e os conceitos de classes em Python, fundamentais para trabalhar com models no Django, já que eles são definidos como classes.

## COMO RODAR ESSE PROJETO EM SEU COMPUTADOR:

### Requisitos

- **Python 3.12 com PIP e venv**
- **o Django 5 requer Python 3.10 ou superior.**

- **No [repositório 001](https://github.com/Django-Dev-Br/001-django5-basic-project) há explicações sobre PIP e venv**
  
  [Baixar Python 3.12](https://www.python.org/downloads/release/python-3122/)

   Confira o vídeo para saber como trabalhar com múltiplas versões do Python e com venv (ambiente virtual):
  [![Watch the video](https://img.youtube.com/vi/eetDeQrv0Rs/0.jpg)](https://youtu.be/eetDeQrv0Rs)


### 7 passos simples para executar

1. **Clone o repositório**:
    ```bash
    git clone https://github.com/Django-Dev-Br/011-Django5-Models.git
    ```

2. **Crie um ambiente virtual**:
   
    **Windows**
    ```bash
     python -m venv myvenv  
    ```
   **Linux**
    ```bash
     python3 -m venv myvenv  
    ```

3. **Ative o ambiente virtual criado**:
   
    **Windows**
    ```bash
    myvenv\Scripts\activate  
    ```

   **Linux**
    ```bash
    source myvenv/bin/activate  
    ```
    
4. **Acesse a pasta do repositório**:
   
    ```bash
    cd 011-Django5-Models
    ```
    
6. **Instale o Django**:

   Fazer a instalação após a ativação da virtual env fará com que a instalação seja feita nessa pasta ao invés do computador. Isso significa que o pacote Django não estará disponivel para todos os usuários do computador, mas apenas para o contexto no qual essa venv esteja ativada. Veremos sua ativação logo abaixo.

    **Instalação manualmente via gerenciador de dependências PIP**
    ```bash
    pip install django
    ```
    - use, preferencialmente, a versão 5.1. Para tanto, execute o comando:

     ```bash
    pip install  "django>=5.1,<=5.2"
    ```

    ----- **OU** -----

    **Instalação via arquivo requirements**
    ```bash
    pip install -r requirements.txt
    ```
    O arquivo requirements.txt é um arquivo de texto que contém uma lista de pacotes a ser instalado em uma venv. É uma boa prática de programação do ecossistema Python.

7. **Crie migrações para o modelo**:
    ```bash
    python manage.py makemigrations
    ```

   Este comando irá analisar as mudanças no modelo do seu aplicativo e criar um arquivo de migração (por exemplo, `0001_initial.py`) na pasta `migrations`. Esse arquivo de migração define como as tabelas do banco de dados devem ser criadas ou alteradas.

8. **Aplique as migrações ao banco de dados**:
    ```bash
    python manage.py migrate
    ```

   O comando `migrate` aplica todas as migrações pendentes, criando as tabelas e aplicando as alterações necessárias ao banco de dados. Isso garante que a estrutura do banco de dados corresponda aos modelos definidos no seu projeto.
   
   O Django, por padrão, utiliza o SQLite como seu banco de dados. Quando você cria e aplica migrações, um arquivo `db.sqlite3` é gerado na raiz do seu projeto. Esse arquivo contém todas as tabelas e dados gerados a partir dos seus models, funcionando como o banco de dados da sua aplicação.

9. **Execute o servidor de desenvolvimento**:
    ```bash
    python manage.py runserver
    ```
10. Após criar e aplicar as migrações para o banco de dados, você pode querer visualizar a estrutura e os dados do banco. Aqui estão duas opções simples para visualizar o banco de dados SQLite utilizado pelo Django:

**DB Browser for SQLite**
[https://sqlitebrowser.org/](https://sqlitebrowser.org/)

Você pode usar o DB Browser for SQLite, que é uma ferramenta gráfica gratuita e fácil de usar. Com ele, você poderá abrir o arquivo db.sqlite3 localizado no diretório raiz do projeto e explorar as tabelas, incluindo o conteúdo e a estrutura delas.

**Extensão DB Viewer para VS Code**
[https://marketplace.visualstudio.com/items?itemName=qwtel.sqlite-viewer](https://marketplace.visualstudio.com/items?itemName=qwtel.sqlite-viewer)

Outra opção é utilizar a extensão DB Viewer para VS Code. Se você já está utilizando o VS Code para desenvolver, essa extensão é uma ótima maneira de acessar e visualizar o conteúdo do banco de dados sem sair do ambiente de desenvolvimento. Basta instalá-la e abrir o arquivo db.sqlite3 diretamente no editor.


### Banco de Dados SQLite

- A configuração do banco de dados em `settings.py` é feita da seguinte forma:

    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': BASE_DIR / 'db.sqlite3',
        }
    }
    ```

   Esta configuração define o uso do SQLite, mas outros Sistemas de Gerenciamento de Banco de Dados (SGBDs) podem ser adicionados posteriormente. O PostgreSQL é frequentemente a escolha mais utilizada no ecossistema do Django devido à sua robustez e recursos avançados.

### Código do arquivo models.py 


```
from django.db import models

class Profile(models.Model):
    nome_completo = models.CharField(max_length=100, verbose_name="Nome Completo")
    data_nascimento = models.DateField(verbose_name="Data de Nascimento", blank=True)
    bio = models.TextField(verbose_name="Bio", blank=True)
    linkedin_url = models.URLField(max_length=200, verbose_name="URL do LinkedIn", blank=True)
    email = models.EmailField(verbose_name="Email")
    numero_telefonico = models.IntegerField(verbose_name="Número Telefônico")

    def __str__(self):
        return self.nome_completo
```
Explicações dos Tipos de Campo Utilizados:

1. CharField: 
   - Utilizado para armazenar strings de tamanho fixo.
   - O parâmetro max_length define o número máximo de caracteres que o campo pode ter.
   - Exemplo: nome_completo é um CharField com um máximo de 100 caracteres.

2. DateField: 
   - Usado para armazenar datas (ano, mês, dia).
   - A opção blank=True permite que o campo seja opcional, ou seja, não é obrigatório preencher este campo.
   - Exemplo: data_nascimento é um DateField que permite datas de nascimento.

3. TextField: 
   - Ideal para armazenar grandes blocos de texto.
   - Não possui limite de caracteres.
   - Exemplo: bio é um TextField usado para armazenar uma biografia, permitindo mais conteúdo textual.

4. URLField: 
   - Usado para armazenar URLs, garantindo que os dados inseridos sejam validados como URLs.
   - max_length define o comprimento máximo permitido para a URL.
   - Exemplo: linkedin_url é um URLField para armazenar a URL do perfil do LinkedIn.

5. EmailField: 
   - Um campo específico para armazenar endereços de e-mail.
   - Valida automaticamente o formato do e-mail para garantir que os dados inseridos estejam corretos.
   - Exemplo: email é um EmailField usado para armazenar endereços de e-mail.

6. IntegerField: 
   - Utilizado para armazenar números inteiros.
   - Exemplo: numero_telefonico é um IntegerField para armazenar números telefônicos como inteiros.

### Estrutura de Diretórios do Projeto

```
011-Django5-Profile-Model/
├── manage.py
├── myapp/
│   ├── __init__.py
│   ├── apps.py
│   ├── models.py        # Contém a definição do modelo Profile
│   ├── migrations/
│       ├── __init__.py
│       └── 0001_initial.py  # Criado após executar o comando 'makemigrations'
│   └── views.py
├── myproject/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py      # Configurações do Django
│   ├── urls.py
│   └── wsgi.py
├── db.sqlite3           # Banco de dados SQLite
└── README.md
└── requirements.txt    # Lista de pacotes Python necessários para o projeto
```

### Sobre Nosso Treinamento Prático-Profissional com projeto real para iniciantes e avançados em web DevOps Full-stack com Python, Django, Bootstrap e Linux.

[Django Developers Brasil - Aprenda programando enquanto programa aprendendo!](https://django.dev.br/)

Nosso treinamento oferece uma experiência prática de aprendizado de programação, adequada tanto para iniciantes quanto para desenvolvedores avançados. Você participará de um projeto real de desenvolvimento de software em um ambiente corporativo autêntico, onde pessoas com diferentes níveis de conhecimento irão colaborar, aprendendo umas com as outras.

**Junte-se a nós!** E desenvolva as habilidades necessárias para o mercado de trabalho, aprimorando tanto seus conhecimentos técnicos quanto suas soft skills em um ambiente colaborativo e realista.

