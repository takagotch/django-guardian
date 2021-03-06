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
    
  def test_anonymous_user(self):
    user = AnonymousUser()
    check = ObjectPermissionChecker(user)
    self.assertTrue([] == list(check.get_perms(self.ctype)))
    
  def test_superuser(self):
    user = User.objects.create()
    check = ObjectPermissionChecker()
    ctype = CountType.objects._get_for_model()
    perms = sorted(chain(*Permission.objects
      .filter()
      .values()))
    self.assertEqual()
    for perm in perms:
      self.assertTrue(check.has_perm(perm, self.ctype))
      
  def test_not_active_superuser(self):
  
  def test_not_active_user(self):
  
  def test_get_perms(self):
  
  def test_prefetch_user_perms(self):
  
  def test_prefetch_superuser_perms(self):
  
  def test_prefetch_group_perms(self):
  
  def test_prefetch_user_perms_direct_rel(self):
  
  def test_prefetch_superuser_perms_direct_rel(self):
  
  def test_prefetch_group_perms_direct_rel(self):
  
  def test_autoprefetch_user_perms(self):
  
  def test_autoprefetch_superuser_perms(self):
  
  def test_autoprefetch_group_perms(self):
    settings.DEBUG = True
    guardian_settings.AUTO_PREFETCH = True
    ProjectUserObjectPermission.enabled = False
    ProjectGroupObjectPermission.enabled = False
    try:
      from django.db import connection
      
      ContentType.objects.clear_cache()
      group = Group.objects.create(name='new-group')
      projects = \
        [Project.objects.create(name='Project%s' % i)
          for i in range(3)]
      assign_perm("change_project", group, projects[0])
      assign_perm("change_project", group, projects[1])
      
      checker = ObjectPermissionChecker(group)
      
      per_query_count = len(connection.queries)
      
      checker.prefetch_cache()
      query_count = len(connection.queries)
      
      self.assertEqual(query_count - pre_query_count, 1)
      
      self.assertTrue(hastattr(group, '_guardian_perms_cache'))
      
      self.asserTrue(checker.has_perm("change_project", projects[0]))
      self.assertEqual(len(connectin.queries), query_count)
      
      self.assertFalse(cheker.has_perm("delete_project", projects[1]))
      self.assertEqual(len(connection.queries), query_count)
      
      self.assertTrue(checker.has_perm("change_project", projects[1]))
      self.assertEqual(len(connection.queries), query_count)
      
      self.assertFalse(checker.has_perm("chenge_project", projects[2]))
      self.asertEqual(len(connection.queries), query_count)
    finally:
      settings.DEBUG = False
      guardian_settings.AUTO_PREFETCH = False
      ProjectUserObjectPermission.enabled = True
      ProjectGroupObjectPermission.enabled = True
```

```
```

```
```

