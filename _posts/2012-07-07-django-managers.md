---
layout: post
title: 'Django Managers'

---
<p>
A manager is a special attribute used to query the Database while using Django framework.
For e.g. In Django code  <strong>Person.objects.all</strong>
<strong>Person</strong> is the <strong>model</strong>.
<strong>objects</strong> is the <em><strong>manager</strong></em><strong></strong>.
<strong>Points to remember:</strong>
</p>

<ol>
	<li>Each Django model has at least one manager.</li>
	<li>It is possible to write custom managers.</li>
</ol>
<p>Why do we need custom manager? There are several reasons , but the most common ones being,</p>
<ol>
	<li>To add extra manager methods to provide table-level functionality to models.</li>
	<li>To modify the initial QuerySet that is returned by the manager.</li>
	<li>Helps to avoid having duplicate code.</li>
</ol>
<p><strong>Example of a custom manager for Person Model</strong></p>


	#models.py
	from django.db import models

	class PersonManager(models.Manager):
		def person_count(self,keyword):
			return self.filter(name__icontains = keyword).count()

	<em>class Person(models.Model):</em>
	<em> name = models.CharField(max_length=50)</em>
	# some code goes here ...
	<em>objects = PersonManager()</em> # This assignment will replace the "default" manager for the model.

	<em> Person.objects.person_count('john')</em> # will return the count of people with name 'John'
	3

<p>You can modify initial QuerySet by overriding Manager.get_query_set() method. A good example is</p>

	<em>class MaleManager(models.Manager):</em>
	<em> def get_query_set(self):</em>
	<em> return super(MaleManager, self).get_query_set().filter(sex='M') # filter Male.</em>

	<em>class FemaleManager(models.Manager):</em>
	<em> def get_query_set(self):</em>
	<em> return super(FemaleManager, self).get_query_set().filter(sex='F') # filter Female's</em>

	<em>class Person(models.Model):</em>
	<em> first_name = models.CharField(max_length=50)</em>
	<em> last_name = models.CharField(max_length=50)</em>
	<em> sex = models.CharField(max_length=1, choices=(('M', 'Male'), ('F', 'Female')))</em>
	<em>people = models.Manager()</em> # default manager
	<em>malemgr = MaleManager()</em> # manager to manipulate initial query set for filtering males.
	<em>femmgr = FemaleManager()</em> # manager to manipulate initial query set for filtering females.

<p>Calls to these managers are done as follows:</p>
<em> Person.malemgr.all()</em>
<em>  Person.femmgr.all()</em>
<em>  Person.people.all()</em>
