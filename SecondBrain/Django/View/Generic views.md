# Basic views

## view

기본 뷰 클래스

method 목록:
1. [`setup()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.setup "django.views.generic.base.View.setup")
2. [`dispatch()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.dispatch "django.views.generic.base.View.dispatch")
3. [`http_method_not_allowed()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.http_method_not_allowed "django.views.generic.base.View.http_method_not_allowed")
4. [`options()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.options "django.views.generic.base.View.options")

ex) views.py
```
from django.http import HttpResponse
from django.views import View

class MyView(View):
    def get(self, request, *args, **kwargs):
        return HttpResponse("Hello, World!")
```
ex) urls.py
```
from django.urls import path
from myapp.views import MyView

urlpatterns = [
    path("mine/", MyView.as_view(), name="my-view"),
]
```

`as_view(initkwargs)` : 요청을 받고 응답을 반환하는 호출 가능한 view를 반환한다.

## TemplateView
간단한 정보를 템플릿을 통해 사용자에게 전달하거나 정적인 콘텐츠를 보여줄 때 유용하다. 예를 들어, 웹 페이지의 소개, 문서 페이지, 단순한 데이터 목록 등을 템플릿과 함께 보여주는 데 활용할 수 있다.

method 목록:
1. [`setup()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.setup "django.views.generic.base.View.setup")
2. [`dispatch()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.dispatch "django.views.generic.base.View.dispatch")
3. [`http_method_not_allowed()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.http_method_not_allowed "django.views.generic.base.View.http_method_not_allowed")
4. [`get_context_data()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-simple/#django.views.generic.base.ContextMixin.get_context_data "django.views.generic.base.ContextMixin.get_context_data")

ex) views.py
```
from django.views.generic.base import TemplateView

from articles.models import Article

class HomePageView(TemplateView):
    template_name = "home.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["latest_articles"] = Article.objects.all()[:5]
        return context
```


## RedirectView
지정된 URL로 리다이렉션하는 view

method 목록:
1. [`setup()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.setup "django.views.generic.base.View.setup")
2. [`dispatch()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.dispatch "django.views.generic.base.View.dispatch")
3. [`http_method_not_allowed()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.http_method_not_allowed "django.views.generic.base.View.http_method_not_allowed")
4. [`get_redirect_url()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.RedirectView.get_redirect_url "django.views.generic.base.RedirectView.get_redirect_url")

ex) views.py
```
from django.shortcuts import get_object_or_404
from django.views.generic.base import RedirectView

from articles.models import Article

class ArticleCounterRedirectView(RedirectView):
    permanent = False
    query_string = True
    pattern_name = "article-detail"

    def get_redirect_url(self, *args, **kwargs):
        article = get_object_or_404(Article, pk=kwargs["pk"])
        article.update_counter()
        return super().get_redirect_url(*args, **kwargs)
```

ex) urls.py
```
from django.urls import path
from django.views.generic.base import RedirectView

from article.views import ArticleCounterRedirectView, ArticleDetailView

urlpatterns = [
    path(
        "counter/<int:pk>/",
        ArticleCounterRedirectView.as_view(),
        name="article-counter",
    ),
    path("details/<int:pk>/", ArticleDetailView.as_view(), name="article-detail"),
    path(
        "go-to-django/",
        RedirectView.as_view(url="https://www.djangoproject.com/"),
        name="go-to-django",
    ),
]
```

<hr>

# Generic display views

## DetailView
아마 하나의 객체의 detail을 표시할 때 사용하는 Read를 위한 view
이 view가 실행되는 동안에 `self.object`가 작동 중인 객체가 포함된다.

method 목록
1. [`setup()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.setup "django.views.generic.base.View.setup")
2. [`dispatch()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.dispatch "django.views.generic.base.View.dispatch")
3. [`http_method_not_allowed()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.http_method_not_allowed "django.views.generic.base.View.http_method_not_allowed")
4. [`get_template_names()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-simple/#django.views.generic.base.TemplateResponseMixin.get_template_names "django.views.generic.base.TemplateResponseMixin.get_template_names")
5. [`get_slug_field()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-single-object/#django.views.generic.detail.SingleObjectMixin.get_slug_field "django.views.generic.detail.SingleObjectMixin.get_slug_field")
6. [`get_queryset()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-single-object/#django.views.generic.detail.SingleObjectMixin.get_queryset "django.views.generic.detail.SingleObjectMixin.get_queryset")
7. [`get_object()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-single-object/#django.views.generic.detail.SingleObjectMixin.get_object "django.views.generic.detail.SingleObjectMixin.get_object")
8. [`get_context_object_name()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-single-object/#django.views.generic.detail.SingleObjectMixin.get_context_object_name "django.views.generic.detail.SingleObjectMixin.get_context_object_name")
9. [`get_context_data()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-single-object/#django.views.generic.detail.SingleObjectMixin.get_context_data "django.views.generic.detail.SingleObjectMixin.get_context_data")
10. [`get()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/generic-display/#django.views.generic.detail.BaseDetailView.get "django.views.generic.detail.BaseDetailView.get")
11. [`render_to_response()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-simple/#django.views.generic.base.TemplateResponseMixin.render_to_response "django.views.generic.base.TemplateResponseMixin.render_to_response")

ex) myapp/views.py
```
from django.utils import timezone
from django.views.generic.detail import DetailView

from articles.models import Article

class ArticleDetailView(DetailView):
    model = Article

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["now"] = timezone.now()
        return context
```

ex) myapp/urls.py
```
from django.urls import path

from article.views import ArticleDetailView

urlpatterns = [
    path("<slug:slug>/", ArticleDetailView.as_view(), name="article-detail"),
]
```

ex) myapp/article_detail.html :
```
<h1>{{ object.headline }}</h1>
<p>{{ object.content }}</p>
<p>Reporter: {{ object.reporter }}</p>
<p>Published: {{ object.pub_date|date }}</p>
<p>Date: {{ now|date }}</p>
```


## ListView
iterable한 여러 객체의 목록을 표시할 때 사용되는 Read를 위한 view 
이 view가 실행되는 동안 `self.object_list` 가 작동하는 객체 목록이 포함된다.

method 목록

1. [`setup()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.setup "django.views.generic.base.View.setup")
2. [`dispatch()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.dispatch "django.views.generic.base.View.dispatch")
3. [`http_method_not_allowed()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/base/#django.views.generic.base.View.http_method_not_allowed "django.views.generic.base.View.http_method_not_allowed")
4. [`get_template_names()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-simple/#django.views.generic.base.TemplateResponseMixin.get_template_names "django.views.generic.base.TemplateResponseMixin.get_template_names")
5. [`get_queryset()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-multiple-object/#django.views.generic.list.MultipleObjectMixin.get_queryset "django.views.generic.list.MultipleObjectMixin.get_queryset")
6. [`get_context_object_name()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-multiple-object/#django.views.generic.list.MultipleObjectMixin.get_context_object_name "django.views.generic.list.MultipleObjectMixin.get_context_object_name")
7. [`get_context_data()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-multiple-object/#django.views.generic.list.MultipleObjectMixin.get_context_data "django.views.generic.list.MultipleObjectMixin.get_context_data")
8. [`get()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/generic-display/#django.views.generic.list.BaseListView.get "django.views.generic.list.BaseListView.get")
9. [`render_to_response()`](https://docs.djangoproject.com/ko/4.2/ref/class-based-views/mixins-simple/#django.views.generic.base.TemplateResponseMixin.render_to_response "django.views.generic.base.TemplateResponseMixin.render_to_response")

ex) views.py

```
from django.utils import timezone
from django.views.generic.list import ListView

from articles.models import Article

class ArticleListView(ListView):
    model = Article
    paginate_by = 100  # if pagination is desired

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["now"] = timezone.now()
        return context
```

ex) myapp/urls.py

```
from django.urls import path

from article.views import ArticleListView

urlpatterns = [
    path("", ArticleListView.as_view(), name="article-list"),
]
```

ex) myapp/article_list.html
```
<h1>Articles</h1>
<ul>
{% for article in object_list %}
    <li>{{ article.pub_date|date }} - {{ article.headline }}</li>
{% empty %}
    <li>No articles yet.</li>
{% endfor %}
</ul>
```

<hr>

# Generic editing views

## FormView
form을 표시하는 view로 오류가 발생할 시 유효성 검사 오류가 있는 양식을 다시 표시한다. 성공할 시 새 URL로 redirection 된다.

ex) myapp/forms.py
```
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField()
    message = forms.CharField(widget=forms.Textarea)

    def send_email(self):
    # email을 전송하는 로직을 구현한다.
        pass
```

ex) myapp/views.py

```
from myapp.forms import ContactForm
from django.views.generic.edit import FormView

class ContactFormView(FormView):
    template_name = "contact.html"
    form_class = ContactForm
    success_url = "/thanks/"
    # 폼 제출 후 리다이렉션할 URL 지정

    def form_valid(self, form):
	    # FormView 클래스의 유효성검사 함수를 오버라이드 한 함수
        # This method is called when valid form data has been POSTed.
        # It should return an HttpResponse.
        form.send_email()
        return super().form_valid(form)
        // FormView 클래스의 기본적인 유효성검사를 추가적으로 진행하도록 super().form_valid(form)을 수행한다.
```

ex) myapp/contact.html

```
<form method="post">{% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Send message">
</form>
```


## CreateView
객체 생성, 유효성 검사에 실패한 경우 양식 다시 표시, 객체 저장을 위한 양식을 표시하는 view이다.

ex) myapp/views.py

```
from django.views.generic.edit import CreateView
from myapp.models import Author

class AuthorCreateView(CreateView):
    model = Author
    fields = ["name"]
```

ex) myapp/author_form.html
```
<form method="post">{% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Save">
</form>
```


## UpdateView
객체 수정, 유효성 검사에 실패한 경우 양식 다시 표시, 객체 저장을 위한 양식을 표시하는 view이다.

ex) myapp/views.py

```
from django.views.generic.edit import UpdateView
from myapp.models import Author

class AuthorUpdateView(UpdateView):
    model = Author
    fields = ["name"]
    template_name_suffix = "_update_form"
```

ex) myapp/author_form.html
```
<form method="post">{% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Update">
</form>
```



## DeleteView
객체를 삭제하는 view, POST 방식으로 요청된 경우에만 삭제된다. GET을 통해 이 view를 호출할 시, 동일한 URL에 POST하는 양식이 포함된 확인 페이지가 표시된다.

ex) myapp/views.py

```
from django.urls import reverse_lazy
from django.views.generic.edit import DeleteView
from myapp.models import Author

class AuthorDeleteView(DeleteView):
    model = Author
    success_url = reverse_lazy("author-list")
```

ex) myapp/author_form.html
```
<form method="post">{% csrf_token %}
    <p>Are you sure you want to delete "{{ object }}"?</p>
    {{ form }}
    <input type="submit" value="Confirm">
</form>
```




