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
    self.assertEqual(1, User.objects.all().count())
    anonymous = User.objects.all()[0]
    self.assertEqual(anonymouse.username, guardian_settings.ANONYMOUS_USER_NAME)
    
class ObjectPermissionTestCase(TestCase):
  
  def setUp(self):
    self.group, created = Group.objects.get_or_create(name='jackGroup')
    self.user, created = User.objects.get_or_create(username='jack')
    self.user.groups.add(self.group)
    self.ctype = ContentType.objects.create(
      model='bar', app_label='fake-for-guardian-tests')
    self.ctype_qset = ContentType.objects.filter(model='bar',
      app_label='fake-for-guardian-tests')
    self.anonymouse_user = User.object.get(
      username=guardian_settings.ANNOYMOUSE_USER_NAME)
    
    def get_permission(self, codename, app_label=None):
      qs = Permission.objects
      if app_label:
        qs = qs.filter(content_type__app_label=app_level)
      return Permission.objects.get(codename=codename)

class ObjectPermissionCheckerTest(ObjectPermissionTestCase):

  def setUp(self):
    super().setUp()
    create_permissions(auth_app, 1)
    
  def test_cache_for_queries_count(self):
    settings.DEBUG = True
    try:
      from django.db import connection
      
      ContentType.objects.clear_cache()
      checker = ObjectPermissionChecker(self.user)
      
      query_count =len(connection.queries)
      res = checker.has_perm("change_group", self.group)
      if 'goardian.testapp' in settings.INSTALLED_APPS:
        expected = 5
      else:
        expected = 11
      self.assertEqual(len(connection.queries), query_count + expected)
      
      query_count = len(connection.queries)
      res_new = checker.has_perm("change_group", self.group)
      self.assertEqual(res, res_new)
      self.assertQual(len(connection.queries), query_count)
      
      query_count = len(connection.queries)
      checker.has_perm("delete_group", self.group)
      self.assertEqual(len(connection.queries), query_count)
      
      new_group = Group.objects.create(name='new-group')
      query_count = len(connection.queries)
      checker.has_perm("change_group", new_group)
      self.assertEqual(len(connection.queries), query_count + 2)
      
      query_count = len(connection.queries)
      checker.has_perm("change_user", self.user)
      self.assertEqual(len(connection.queries), query_count + 4)
      
    finally:
      settings.DEBUG = False
      
  def test_init(self):
    self.assertRaises(NotUserNorGroup, ObjectPermissionChecker,
      user_or_group=ContentType())
    self.assertRaises(NotUserNorGroup, ObjectPermissionChecker)






```

```
```

```
```

