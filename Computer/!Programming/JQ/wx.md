# weui

## date picker
```

    wxpickerdate:function(id,name){
        weui.datePicker({
            start: 2020,
            end: new Date().getFullYear(),
            onConfirm: function (result) {
                $(id).text($.time.wxtransdate(result))
            },
            //onChange: function (result) {},
            title: name
        });
    }
    

```
## time picker

    wxpickertime:function(id,name){
            let hours =new Array(),minites=new Array()
            for (var i = 0; i< 24; i++) {
                var hours_item = {};
                hours_item.label = ('' + i).length === 1 ? '0' + i : '' + i;
                hours_item.value = i;
                hours.push(hours_item);
            }
            for (var j= 0; j < 60; j++) {
                var minites_item = {};
                minites_item.label = ('' + j).length === 1 ? '0' + j : '' + j;
                minites_item.value = j;
                minites.push(minites_item);
            }
    
        weui.picker(hours, minites, {
            defaultValue: [new Date().getHours(), new Date().getMinutes()],
            onConfirm: function(result) {
                $(id).text($.time.wxtranstime(result))
            },
            title: name
        });
    },
```


```