from django.db import models

# Create your models here.
class ProductModel(models.Model):

    name=models.CharField(max_length=100)

    category=models.CharField(max_length=100)

    price=models.DecimalField(max_digits=100,decimal_places=2)

    quantity=models.IntegerField()

    date_added=models.DateField(auto_now_add=True)





from django import forms
from application.models import ProductModel

class ProductForm(forms.ModelForm):

    class Meta:

        model=ProductModel

        fields=["name","category","price","quantity"]




from django.shortcuts import render
from django.views.generic import View
from application.forms import ProductForm
from application.models import ProductModel

# Create your views here.
class AddProduct(View):

    def get(self,request):

        form=ProductForm

        return render(request,"addproduct.html",{"form":form})
    

    def post (self,request):

        form=ProductForm(request.POST)

        if form.is_valid():

            ProductModel.objects.create(**form.cleaned_data)

        form=ProductForm

        return render(request,"addproduct.html",{"form":form})   


class ReadProduct(View):

    def get(self,request):

        data=ProductModel.objects.all()

        return render(request,"readprod.html",{'data':data}) 


from django.contrib import admin
from django.urls import path
from application import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('addproduct/',views.AddProduct.as_view()),
    path('readproduct/',views.ReadProduct.as_view()),
