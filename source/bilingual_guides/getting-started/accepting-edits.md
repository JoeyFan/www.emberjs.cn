英文原文：[http://emberjs.com/guides/getting-started/accepting-edits/](http://emberjs.com/guides/getting-started/accepting-edits/)

## Accepting Edits

## 接受修改

In the previous step we updated TodoMVC to allow a user to toggle the display of a text `<input>` for editing a todo's title. Next, we'll add the behavior that immediately focuses the `<input>` when it appears, accepts user input and, when the user presses the `<enter>` key or moves focus away from the editing `<input>` element, persists these changes, then redisplays the todo with its newly updated text.

在上一步中我们修改了TodoMVC使其可以支持用户能够切换到一个文本输入框`<input>`来编辑一个待办事项的标题。接下来，我们将实现在`<input>`显示时立即当前焦点移至其上，开始接收用户的输入，并在用户按下`<enter>`键时或把焦点从编辑的`<input>`元素中移出时，将用户的修改持久化，并显示待办事项修改后的标题。

Create a new file `js/views/edit_todo_view.js`. You may place this file anywhere you like (even just putting all code into the same file), but this guide will assume you have created the file and named it as indicated.

首先我们需要创建一个新的文件`js/views/edit_todo_view.js`。你可以将该文件放置到任意你喜欢的地方（即使将所有代码放置在同一个文件中），不过本指南将假设你按照指定的方式创建和命名该文件。

In `js/views/edit_todo_view.js` create an extension of `Ember.TextField`:

在`js/views/edit_todo_view.js`中创建一个`Ember.TextField`的扩展：

```javascript
Todos.EditTodoView = Ember.TextField.extend({
  classNames: ['edit'],

  insertNewline: function () {
    this.get('controller').acceptChanges();
  },

  focusOut: function () {
    this.get('controller').acceptChanges();
  },

  didInsertElement: function () {
    this.$().focus();
  }
});
```

In `index.html` require this new file:

在`index.html`中引入这个新文件：

```html
<!--- ... additional lines truncated for brevity ... -->
<!--- ... 为保持代码简洁，在此省略了其他代码 ... -->
  <script src="js/controllers/todo_controller.js"></script>
  <script src="js/views/edit_todo_view.js"></script>
</body>
<!--- ... additional lines truncated for brevity ... -->
<!--- ... 为保持代码简洁，在此省略了其他代码 ... -->
```

This view implements the `<input>` behavior described above. When the `<input>` is inserted into the page, it will immediately gain focus through its `didInsertElement` event. Pressing the `<enter>` key  will trigger the `insertNewline` event, calling a method `acceptChanges` on this view's instance of `TodoController`. Moving focus away from from the `<input>` will trigger the `focusOut` event, calling a method `acceptChanges` on this view's instance of `TodoController`.

这个视图为`<input>`实现了之前描述的行为。当`<input>`插入到页面时，通过`didInsertElement`事件可以立即获取焦点。按下`<enter>`键将触发`insertNewline`事件，并调用该视图对应的`TodoController`实例的`acceptChanges`方法。当`<input>`失去焦点时，将触发`focusOut`事件，并也会调用该视图对应的`TodoController`实例的`acceptChanges`方法。

A CSS class `edit` is applied for styling.

CSS类`edit`被用于样式。

In `index.html` replace the static `<input>` element with `Todos.EditTodoView` and connect the `value`:

在`index.html`中，将静态的`<input>`元素替换为`Todos.EditTodoView`，并与`value`建立连接：

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<!--- ... 为保持代码简洁，在此省略了其他代码 ... -->
{{#if isEditing}}
  {{view Todos.EditTodoView valueBinding="title"}}
{{else}}
<!--- ... additional lines truncated for brevity ... -->
<!--- ... 为保持代码简洁，在此省略了其他代码 ... -->
```

This will insert an `<input>` element into our page with the behaviors we added above. Additionally, it will bind the `value` property of this `<input>` to the `title` property of this instance of `TodoController`. We will not implement a `title` property on the controller so it will retain the default behavior of proxying all requests to its `model`. 

上述代码将在我们的页面上插入一个具有我们之前添加的行为的`<input>`元素。此外，还会将`<input>`的`value`属性绑定到对应`TodoController`实例的`title`属性。我们不在控制器中实现`title`属性，这样所有针对`title`的请求都会被委派到控制器的`model`。


In `js/controllers/todo_controller.js`, add the method `acceptChanges` that we called from `EditTodoView`:

在`js/controllers/todo_controller.js`中，添加在`EditTodoView`中调用的`acceptChanges`方法：

```javascript
// ... additional lines truncated for brevity ...
// ... 为保持代码简洁，在此省略了其他代码 ...
acceptChanges: function () {
  this.set('isEditing', false);
  this.get('model').save();
},
// ... additional lines truncated for brevity ...
// ... 为保持代码简洁，在此省略了其他代码 ...
```

This method will set the controller's `isEditing` property to false and commit all changes made to the todo.

这个方法将控制器的`isEditing`属性设置为假，并提交所有针对该待办事项的修改。

### Live Preview

### 在线演示

<a class="jsbin-embed" href="http://jsbin.com/ecicok/2/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources

### 附加资源

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/70eb45e2e69e6bbc30a7d8b69812c6696bbc8cd3)
  * [Controller Guide](/guides/controllers)
  * [Ember.TextField API documentation](http://emberjs.com/api/classes/Ember.TextField.html)

  * [用`diff`格式呈现本次修改](https://github.com/emberjs/quickstart-code-sample/commit/70eb45e2e69e6bbc30a7d8b69812c6696bbc8cd3)
  * [控制器指南](/guides/controllers)
  * [Ember.TextField API文档](http://emberjs.com/api/classes/Ember.TextField.html)
