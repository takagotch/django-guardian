### django-guardian
---
https://github.com/django-guardian/django-guardian

```py
// guardian/testapp/tests/test_core.py
from itertools import chain

from django.conf import settings

from guardian.core import ObjectPermissionChecker

from guardian.testapp.models import Project, ProjectUserObjectPermission, ProjectGroupObjectPermission

auth_app = django_apps.get_app_config('auth')
User = get_user_model()

class CustomUserTests(TestCase):

  def test_create_anonymous_user(self):
    crate_anonymouse_user(object(), using='default')




```

```
```

```
```

