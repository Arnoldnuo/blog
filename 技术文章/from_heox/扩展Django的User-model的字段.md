title: 扩展Django的User model的字段
date: 2015-07-09 18:57:34
tags:
---

###扩展Django的User model（和Django的验证app绑定），添加一些其他的字段的最好的方法是什么？我也想使用email作为username来进行登录验证。我找到了一些[方法](http://www.b-list.org/weblog/2006/jun/06/django-tips-extending-user-model/)，但是我不知道哪个是最好的方法。

------------------
如果你只有一种user类型，我推荐你使用AbstractUser.[Extending Django's default user](https://docs.djangoproject.com/en/dev/topics/auth/customizing/#extending-django-s-default-user)，比如你想在你想为user添加生日和喜欢的颜色，你可以使用下面的代码。

	#myusers/models.py
	from django.contrib.auth.models import AbstractUser
	from django.db import models

	class MyUser(AbstractUser):
    	dob = models.DateField()
    	favorite_color = models.CharField(max_length=32, default='Blue')
 
 如果你想要更灵活，你可以扩展`AbstractBaseUser`，但是一般情况下你只需要`AbstractUser`。
 在一些情况下，你需要[使用settings.AUTH_USER_MODEL修改你的user model](https://docs.djangoproject.com/en/dev/topics/auth/customizing/#substituting-a-custom-user-model)。比如你定义的user model叫做`myusers`
	
	#settings.py
	AUTH_USER_MODEL = 'myusers.MyUser'

如果你通过一个one-to-one的字段引用User model也是可以的，但是这其实不那么简洁，因为它在数据库中创建了其他的表（在那些你需要多种用户的场景下，这种方法依然是有效的）。


