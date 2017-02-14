# Making queries

.zshrc에 알리아스 등록!
mig = makemigrations 랑 migrate

[Saving ForeignKey and ManyToManyField fields](https://docs.djangoproject.com/en/1.10/topics/db/queries/#saving-foreignkey-and-manytomanyfield-fields)

포린키는 add() 못쓰고 대입후에 save()해줘야함
매니투매니는 add()로 저장 

Updating a ManyToManyField works a little differently – use the add() method on the field to add a record to the relation. This example adds the Author instance joe to the entry object:


>* python dynamic variable name
* global()[] 이용하기!!!! 동적으로 변수명 지을수있다
동적으로 키값대입

## 실습
queries에서 실습
queries/blog/models.py

```python
class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

    def __str__(self):
        return self.name


class Author(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField()

    def __str__(self):
        return self.name


class Post(models.Model):
    blog = models.ForeignKey(Blog)
    title = models.CharField(max_length=255)
    content = models.TextField(blank=True)
    published_date = models.DateField(null=True, blank=True)
    modified_date = models.DateField(auto_now=True)
    authors = models.ManyToManyField(Author)
    comments_count = models.IntegerField(default=0)
    pingbacks_count = models.IntegerField(default=0)
    rating = models.IntegerField(default=0)

    def __str__(self):
        return self.title
```

```shell
In [2]: b = Blog.objects.create(name='Beatles Blog', tagline='All the latest Beatles News')

In [3]: b
Out[3]: <Blog: Beatles Blog>

In [5]: a_list = ['John', 'Paul', 'George', 'Ringo']


In [7]: for author in a_list:
   ...:    globals()[author.lower()] =  Author.objects.create(name=author)
   ...:    

In [8]: john
Out[8]: <Author: John>

In [9]: p = Post.objects.create(blog=b, title='First Post')

In [10]: p.authors.add(john, paul, ringo, george)

In [11]: p.authors.all()
Out[11]: <QuerySet [<Author: John>, <Author: Paul>, <Author: George>, <Author: Ringo>]>

```

### Retrieving specific objects with filters

Entry.objects.filter(pub_date__year=2006)
With the default manager class, it is the same as:

Entry.objects.all().filter(pub_date__year=2006)

objects.filter하면 전체에서 필터링함. 결국 둘이 똑같다.


gt는 초과 lt는 작다

### Filtered QuerySets are unique

### QuerySets are lazy
```
>>> q = Entry.objects.filter(headline__startswith="What")
>>> q = q.filter(pub_date__lte=datetime.date.today())
>>> q = q.exclude(body_text__icontains="food")
>>> print(q)
```
마지막에 프린트할때 비로소 쿼리셋이 실행된다 
그전까지는 쿼리 작성만 돼있고 실행은 안되고 있음

## Field lookups
조건걸때는 언더스코어 두개 `__`


The field specified in a lookup has to be the name of a model field. There’s one exception though, in case of a ForeignKey you can specify the field name suffixed with _id. In this case, the value parameter is expected to contain the raw value of the foreign model’s primary key. For example:

```shell
In [17]: Post.objects.get(blog=1)
Out[17]: <Post: First Post>

In [18]: Post.objects.get(blog_id=1)
Out[18]: <Post: First Post>
```

#### exact

```shell
In [14]: Post.objects.get(title__exact="First Post")
Out[14]: <Post: First Post>

In [15]: Post.objects.get(title="First Post")
Out[15]: <Post: First Post>

```

### Lookups that span relationships

필터에서 조건들은 AND연산
````
In [22]: Blog.objects.filter(post__authors__isnull=True)
```
이것은 작가가 null인 블로그를 검사함.
```
In [21]: Blog.objects.filter(post__authors__name__isnull=True)
```
작가안의 이름이 null인 것 까지 검사함

그래서 매니투매니에서 필터링할때 조건을 명확히 분리해서 써줘야하ㅏㅁ
```
Blog.objects.filter(entry__authors__isnull=False, entry__authors__name__isnull=True)
```
이런식으로!


실습

```shell
In [32]: Blog.objects.filter(post__title__contains='Yesterday', post__published_date__day=11)
Out[32]: <QuerySet []>

In [33]: Blog.objects.filter(post__title__contains='Yesterday').filter(post__published_date__day=11)
Out[33]: <QuerySet [<Blog: Beatles Blog>]>

```
위아래의 차이. 안에다 조건을 한꺼번에 주는거랑 필터를 연결시키는거의 차이를 알아야함. 예쌍과 다른 결과를 얻을 수 있다. 

위에꺼는 AND연산이라 동시에 만족해야함 포스트의 타이틀이 예스터데이면서 발행이 11일에 된 포스트가 있는 블로그 객체를 찾는다.


아래는 처음에 먼저 이 조건으로 필터를 건다.
`post__title__contains='Yesterday'`이것과 연관된 Blog객체가 결과로 나온다.
그래서 나온 결과에 다시 두번째 필터`post__published_date__day=11`를 한다. 결과로 Blog객체가 나온다.


### Filters can reference fields on the model

자기 자신에 대해 참조할 때는 F exressions를 이용한다
`from django.db.models import F`

```shell
In [49]: Post.objects.filter(comments_count__gt=F('pingbacks_count'))
Out[49]: <QuerySet [<Post: First Post>]>

```

### aggregate() expressions
