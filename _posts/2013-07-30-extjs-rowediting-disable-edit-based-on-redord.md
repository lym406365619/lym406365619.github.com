---
layout: post
title: ExtJS列表中根据行记录来控制单元格是否可以编辑
categories: [ExtJS]
tags: [RowEditing]
description: ExtJS中，对Grid列表进行编辑，可以通过加入ExtJS列表插件来实现，控制列表中的哪些字段可以编辑比较容易实现。不过根据点击记录的内容来控制是否可以编辑，就稍微复杂一些了
---
{% include JB/setup %}
# ExtJS列表中根据行记录来控制单元格是否可以编辑
ExtJS中，对Grid列表进行编辑，可以通过加入ExtJS列表插件来实现（在grid->columns->column增加<strong>editor</strong>属性），控制列表中的哪些字段可以编辑比较容易实现。不过根据点击记录的内容来控制是否可以编辑，就稍微复杂一些了，下面简单介绍一下。

##1 说明

Use [beforeedit](http://docs.sencha.com/ext-js/4-0/#!/api/Ext.grid.plugin.RowEditing-event-beforeedit) event:
{% highlight js %}
grid.on('beforeedit', function(editor, e) {
  if (e.colIdx === 0 && e.record.get('status') == 4)
    return false;
});
{% endhighlight %}

<strong>UPDATE</strong>

The solution above is not working for rowEditor.
However you can make needed field to be disabled on beforeedit. To do that you should be able to access rowediting plugin. Assign pluginId to plugin:
{% highlight js %}
plugins: [
    Ext.create('Ext.grid.plugin.RowEditing', {
        clicksToEdit: 1,
        pluginId: 'rowEditing'
    })
],
{% endhighlight %}

Now just disable needed field if some conditions are met:
{% highlight js %}
grid.on('beforeedit', function(e) {
    if (e.record.get('status') == 4)
        grid.getPlugin('rowEditing').editor.form.findField('neededColumnDataIndex').disable();
    else
        grid.getPlugin('rowEditing').editor.form.findField('neededColumnDataIndex').enable();
});
{% endhighlight %}

Here is [demo](http://jsfiddle.net/molecule/4Mftm/) (try to edit first row).

##2 demo

{% highlight js %}
Ext.create('Ext.data.Store', {
    storeId:'simpsonsStore',
    fields:['name', 'email', 'phone'],
    data: [
        {"name":"Lisa", "email":"lisa@simpsons.com", "phone":"555-111-1224"},
        {"name":"Bart", "email":"bart@simpsons.com", "phone":"555--222-1234"},
        {"name":"Homer", "email":"home@simpsons.com", "phone":"555-222-1244"},
        {"name":"Marge", "email":"marge@simpsons.com", "phone":"555-222-1254"}
    ]
});

var grid = Ext.create('Ext.grid.Panel', {
    title: 'Simpsons',
    store: Ext.data.StoreManager.lookup('simpsonsStore'),
    columns: [
        {header: 'Name',  dataIndex: 'name', editor: 'textfield'},
        {header: 'Email', dataIndex: 'email', flex:1,
            editor: {
                xtype: 'textfield',
                allowBlank: false
            }
        },
        {header: 'Phone', dataIndex: 'phone'}
    ],
    selType: 'rowmodel',
    plugins: [
        Ext.create('Ext.grid.plugin.RowEditing', {
            clicksToEdit: 1,
            pluginId: 'rowEditing'
        })
    ],
    height: 200,
    width: 400,
    renderTo: Ext.getBody()
});

grid.on('beforeedit', function(e) {
    if (e.record.get('phone') == "555-111-1224")
        grid.getPlugin('rowEditing').editor.form.findField('name').disable();
    else
        grid.getPlugin('rowEditing').editor.form.findField('name').enable();
});
{% endhighlight %}

##参考
[http://stackoverflow.com/questions/7679364/extjs-4-rowediting-disable-edit-on-one-column-based-on-record](http://stackoverflow.com/questions/7679364/extjs-4-rowediting-disable-edit-on-one-column-based-on-record)