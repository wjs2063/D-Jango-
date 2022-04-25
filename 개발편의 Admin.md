 
## 슈퍼유저 생성하기

1. python manage.py createsuperuser
2. 이름,이메일주소,password 만든후 python manage.py runserver
3. localhost:8000/admin
4. pybo/admin.py 클릭후
5. from django.contrib import admin   
   from .models import Question
   admin.site.register(Question)
등록후 사이트 새로고침

6. qeustion 추가하기


## 장고 ADMIN 에 데이터 검색 기능 추가하기

pybo/admin.py 에 QuestionAdmin 클래스 추가후 serach_fields 에 'subject'를 추가


from django.contrib import admin
from .models import Question

# Register your models here.
class QuestionAdmin(admin.ModelAdmin):
    search_fields = ['subject']
admin.site.register(Question,QuestionAdmin)

#### 새로고침 





