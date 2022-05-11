**UNIVERSIDADE LUSÓFONA DE HUMANIDADES E TECNOLOGIAS**

# Lab 10: Portfolio III 🌤️

### Objetivo 

* Implementar as bases de dados e formularios paa alimentar com dados
* criar pagina de autenticação que permita introduzir conteúdos


## 1. Implementação da base de dados 🛢
* Implemente as classes que identificou no DER que fez no lab9
* deverá usar entre outros campos de FileField e [ImageField](#ImageField).
* Deverá garantir relaçoes 1:1, 1:N e N:M
* Veja o exemplo feito na [aula](https://github.com/ULHT-PW/pw-aula-django-02-simples/blob/main/flights/models.py)
* Passos:

#### 1. **criar cada classe** no `models.py`, pode exemplo `class Cadeira(models.Model)`

```Python
# models.py

class Cadeira(models.Model):
   nome = models.CharField(max_length=20)
   ano = models.IntegerField()
   descricao = models.TextField()
   linguagens = models.ManyToMany(Linguagem)
   docente_teorica = models.ForeignKey(Professor, on_delete=models.CASCADE)
   docentes_praticas = models.ManyToMany(Professor, related_name='caderias')
   projetos = models.ManyToMany(Projeto)
...
   
```

#### 2. **registar em admin** cada classe, em `admin.py` da seguinte forma (para poder manipular na aplicação Admin):

```Python
# admin.py

from .models import Cadeira

admin.site.register(Cadeira)
```

#### 3. **migrar**:
```bash
> python manage.py makemigrations
> python manage.py migrate
```

#### 4. Abrir a aplicação em admin
* Entre em `127.0.0.1:8000/admin`
* Deve ter uma conta de superuser criada (descrito em lab9, se não tiver)

#### 5. Inserir dados na aplicação admin. 
Atenção que deverá primeiro criar todas as classses, e só depois começar a inserir dados na base de dados.

## 2. Autenticação 👦👧
* Crie uma pagina de autenticação. Só utilizadores autenticados poderão criar novos conteúdos, à excepção das páginas inerentemente públicas (quizz, blog, etc).
* sempre que necessário, valide se o utilizador está [autenticado](https://github.com/ULHT-PW/pw-aula-django-02-simples/blob/73aca0dca612a04999c52c773e672f3027154b02/tarefas/views.py#L46)
* utilize o decorador [`@login_required`](https://github.com/ULHT-PW/pw-aula-django-02-simples/blob/73aca0dca612a04999c52c773e672f3027154b02/tarefas/views.py#L58) nas views que requerem autenticação. e utilize 

## 2. Formulario
* Para cada página que lista conteúdos, deverá criar um formulário para criar uma nova entrada (uma nova cadeira, projeto, TFC, etc). 
* Deverá verificar se está autenticado no html ([exemplo](https://github.com/ULHT-PW/pw-aula-django-02-simples/blob/73aca0dca612a04999c52c773e672f3027154b02/tarefas/templates/tarefas/tarefas.html#L22)). Se estiver em modo autenticado, deverá inserir um botão editar, que permitirá editar o conteúdo. Veja exemplo que foi feito na [aula](https://github.com/ULHT-PW/pw-aula-django-02-simples/tree/main/tarefas)




## 3. Submissão 🏁

Mantenha o seu projeto sincronizado com o GitHub assim como o Heroku



## <a name="ImageField"></a> campo ImageField

[How to manage static files](https://docs.djangoproject.com/en/4.0/howto/static-files/)

Passos para ter um campo para carregar corretamente uma imagem para uma pasta que queiramos:

1. Primeiro devemos dar instruções para criar uma pasta (MEDIA) onde guardar as imagens. Colocar em settings.py:

```Python
# settings.py

import os
MEDIA_ROOT = os.path.join(BASE_DIR, "media")
MEDIA_URL = "/media/"
```

2. no app/urls.py   (funciona no config/urls.py ?!): 

```Python
# config/urls.py

from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

Depois podemos utilizar na definição do atributo da classe. Podemos especificar  no `upload_to` a pasta, dentro da pasta MEDIA, onde queremos guardar. Por exemplo, em baixo queremos guardar uma imagem duma resposta duma resolução (com id 3) de um teste feito por um paciente (com id 1) em `users/1/resolutions/3`:

```Python
# views.py

def resolution_path(instance, filename):
    return f'users/{instance.resolution.patient.id}/resolutions/{instance.resolution.id}'
    
    
class Answer(models.Model):
    question = models.ForeignKey('Question',
                                 on_delete=models.CASCADE)
    resolution = models.ForeignKey('Resolution',on_delete=models.CASCADE)
    submitted_answer = models.ImageField(upload_to=resolution_path)
```


Garanta que tem submetido no formulário disponivel no Moodle:
* o link para o repo do seu portfolio
* o link para o repo do material recolhido
* o link para a aplicação a correr no Heroku
