---
title: Background wokers are finally coming to Django!!!
published: 2024-06-13
tags: [webdevlopment, django, OSS]
category: Django
draft: true
---

Django will finally be able to execute long-running tasks in the background without using any external package. Previously, I used Celery, but installing Redis and writing all that boilerplate just to send an email is not a great development experience.

## DEP 14 
DEP(Django enhancement purposal) 14 : Background Workers was written and stewarded by [Jake Howard](https://github.com/RealOrangeOne) that was accepted by django steering council. You can see the purposal [here](https://github.com/django/deps/blob/main/accepted/0014-background-workers.rst)

## So when i can try it?
You can try it right now but it is still in development. So things can change.

::github{repo=RealOrangeOne/django-tasks}

## Installation
    
```shell
python -m pip install django-tasks
```

### Setup

Add `django_tasks` to your `INSTALLED_APPS` in your django settings.
You can also define different type of backends that will execute your tasks in settings. You can check them out in the repo.
If not defined default backend is used.
Default backend is `ImmediateBackend` that executes the task immediately in current thread. 
```python
TASKS = {
    "default": {
        "BACKEND": "django_tasks.backends.immediate.ImmediateBackend"
    }
}
```


### Usage
You can define your tasks like this.

```python
from django_tasks import task

@task()
def calculate_meaning_of_life() -> int:
    return 42
```

and then can execute your taks like this 
```python
result = calculate_meaning_of_life.enqueue()
```

### Retrieving Task result

```python
result_id = result.id

# Later, somewhere else...
calculate_meaning_of_life.get_result(result_id)
```

## Conclusion

I think it will be great addition to django. I wanted this feature for a long time. 












