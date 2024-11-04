## Tutorial Completo: Django com Autenticação Detalhada

### Introdução

Este tutorial irá guiá-lo passo a passo na criação de um projeto Django básico com um sistema de autenticação completo, incluindo a view de login e as funcionalidades essenciais para gerenciar usuários.

### Pré-requisitos

  * **Python:** Versão 3.6 ou superior.
  * **Django:** Instalado via `pip install django`.
  * **Editor de código:** Visual Studio Code, PyCharm ou similar.

### Criando o Projeto e Aplicativo

```bash
django-admin startproject myproject .
# OU
python3 -m django startproject myproject .
```

```bash
python manage.py startapp myapp
```

### Configurando o Projeto

1.  **`settings.py`:**

      * Adicione `'myapp'` a `INSTALLED_APPS`.
      * Configure o banco de dados (se necessário).

2.  **`urls.py` (nível do projeto):**

    ```python
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('myapp.urls')),
    ]
    ```

### Criando as Views e URLs do Aplicativo

1.  **`urls.py` (nível do aplicativo):**

    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.home, name='home'),
        path('login/', views.login_view, name='login'),
        path('logout/', views.logout_view, name='logout'),
    ]
    ```

2.  **`views.py`:**

    ```python
    from django.shortcuts import render, redirect
    from django.contrib.auth import authenticate, login, logout
    from django.contrib.auth.forms import AuthenticationForm

    def home(request):
        if request.user.is_authenticated:
            return render(request, 'home.html')
        return redirect('login')

    def login_view(request):
        if request.method == 'POST':
            form = AuthenticationForm(data=request.POST)
            if form.is_valid():
                user = form.get_user()
                login(request, user)
                return redirect('home')
        else:
            form = AuthenticationForm()
        return render(request, 'login.html', {'form': form})

    def logout_view(request):
        logout(request)
        return redirect('home')
    ```

### Criando os Templates

#### Dentro da pasta do seu app (myapp) crie a pasta `templates` e dentro desta crie os seguintes arquivos:

1.  **`home.html`:**

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Home</title>
    </head>
    <body>
        <h1>Olá, {{ user.username }}!</h1>
        <p>Seu email é: {{ user.email }}</p>
        <a href="{%url 'logout'%}">Sair</a>
    </body>
    </html>
    ```

2.  **`login.html`:**

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Login</title>
    </head>
    <body>
        <form method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <button type="submit">Login</button>
        </form>
    </body>
    </html>
    ```

## Executando o Projeto

#### Primeiro aplique as migrações
```bash
python manage.py makemigrations
python manage.py migrate
```
## Crie o usuário admin

```bash
python manage.py createsuperuser
```

#### E então inicie o servidor
```bash
python manage.py runserver
```
## Testes
#### Para testar faça login com os dados que você digitou junto ao comando createsuperuser.
#### Para criar outros usuários acesse `http://127.0.0.1:8000/admin/`, faça login com os mesmos dados.


### Explicação Detalhada

  * **`views.py`:**
      * **`home`:** Redireciona para a página de login se o usuário não estiver autenticado.
      * **`login_view`:** Processa o formulário de login, autentica o usuário e redireciona para a página inicial.
      * **`logout_view`:** Faz logout do usuário e redireciona para a página inicial.
  * **`login.html`:**
      * Renderiza o formulário de login usando o `AuthenticationForm` do Django.
  * **`home.html`:**
      * Exibe uma mensagem de boas-vindas ao usuário logado.

### Melhorias

  * **Registro de usuários:** Crie um formulário de registro para permitir que novos usuários se cadastrem.
  * **Recuperação de senha:** Implemente a funcionalidade de recuperação de senha esquecida.
  * **Permissões:** Utilize grupos e permissões para controlar o acesso a diferentes partes do seu aplicativo.
  * **Customização:** Personalize os templates e formulários de acordo com suas necessidades.

### Observações

  * **Segurança:** Sempre valide e sanitize todos os dados de entrada para evitar vulnerabilidades.
  * **Formulários:** O Django Forms pode ser usado para criar formulários personalizados.
  * **Mensagens:** Utilize o framework de mensagens do Django para exibir mensagens de sucesso, erro, etc.

**Recursos adicionais:**

  * **Documentação oficial:** [https://docs.djangoproject.com/en/4.2/topics/auth/](https://www.google.com/url?sa=E&source=gmail&q=https://docs.djangoproject.com/en/4.2/topics/auth/)

Com este tutorial, você estará apto a criar projetos Django com autenticação de forma eficiente e segura.
