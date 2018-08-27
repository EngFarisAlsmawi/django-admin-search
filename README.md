Django Admin Search
===================

# Install:
    pip install django-admin-search
    
# Add to your INSTALLED_APPS, in settings.py:
    INSTALLED_APPS = [  
        ...
        'django_admin_search',
        ...
    ]  

# Create a search form for model:
    from .models import Area
    from django.forms import ModelForm, Form
    from django.forms import DateField, CharField, ChoiceField, TextInput


    class YourFormSearch(Form):
        name = CharField(required=False)
        date = DateField(required=False, widget=TextInput(
            attrs={ 
                'filter_method': '__gte',
            }
        ))

# In your admin:
    from django_admin_search.admin import BaseAdvacedSearchAdmin
    from .models import YourModel
    from .form import YourForm, YourFormSearch

    @register(YourModel)
    class ModelAdmin(BaseAdvacedSearchAdmin):
        form = YourForm
        search_form = YourFormSearch


# to multiple filters in same field:
    class YourFormSearch(Form):
        ...
        name = CharField(required=False)
        begin = DateField(required=False, widget=TextInput(
            attrs={
                'filter_field': 'date', 
                'filter_method': '__gte',
            }
        ))
        end = DateField(required=False, widget=TextInput(
            attrs={
                'filter_field': 'date', 
                'filter_method': '__lte',
            }
        ))

# add placeholder and mask
    class YourFormSearch(Form):
        ...
        date = DateField(required=False, widget=TextInput(
            attrs={
                'data-mask': "00/00/0000", 
                'placeholder': 'MM/DD/YYYY'
            }
        ))