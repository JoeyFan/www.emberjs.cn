英文原文：[http://emberjs.com/guides/views/built-in-views/](http://emberjs.com/guides/views/built-in-views/)

## 内置视图

Ember中定义了一套用于构建一些非常基础的控件的视图，比如文本输入框、勾选框和选择列表。

这些视图有：

#### Ember.Checkbox

```handlebars
<label>
  {{view Ember.Checkbox checkedBinding="model.isDone"}}
  {{model.title}}
</label>
```

#### Ember.TextField

```javascript
App.MyText = Ember.TextField.extend({
  formBlurredBinding: 'App.adminController.formBlurred',
  change: function(evt) {
    this.set('formBlurred', true);
  }
});
```

#### Ember.Select

```handlebars
{{view Ember.Select viewName="select"
                    contentBinding="App.peopleController"
                    optionLabelPath="model.fullName"
                    optionValuePath="model.id"
                    prompt="Pick a person:"
                    selectionBinding="App.selectedPersonController.person"}}
```

#### Ember.TextArea

```javascript
var textArea = Ember.TextArea.create({
  valueBinding: 'TestObject.value'
});
```
