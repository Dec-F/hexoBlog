---
title: 项目中可编辑Tree结构的最佳实践
date: 2017-06-17 10:36:28
categories:
- Javascript
tags:
- Vue
- JavaScript
---
公司后台项目使用的是Vue+Element组件库，整体上觉得很好用。但是有些组件还是需要结合业务需要自己来实现的，比如这次的Tree组件，需要支持编辑并且最后的数据要上传服务器储存。

Element中的Tree组件目前是支持JSX语法渲染出编辑按钮和事件处理函数的，但是并没有提供数据的传出接口，这就导致了我只能更新view但是拿不到编辑后的数据。由于是多人协作的项目，改Tree源码是不现实的，我就决定自己实现一个。

递归渲染出Tree是不难的，难点在加入编辑功能后就需要频繁的使用id去遍历Tree的数据，寻找目标数据后改变它。极其不优雅。

后来在知乎上看到一种实现思路，目前我觉得是很好的。大概就是使用Tree生成一个Map，可以直接用id索引到目标值，而Tree和Map中保存的是同一数据的两份引用，直接修改Map，Tree中的值也会变。这样只维护一份源数据就可以既能够有Tree结构方便展示的长处，也能有Map结构方便查找的优点。

<!--more -->

Tree结构

```js
[{
    id: 1,
    label: '一级 1',
    children: [{
        id: 4,
        label: '二级 1-1',
        children: [{
            id: 9,
            label: '三级 1-1-1'
            }, {
              id: 10,
              label: '三级 1-1-2'
            }]
        }]
    }, {
        id: 2,
        label: '一级 2',
        children: [{
            id: 5,
            label: '二级 2-1'
          }, {
            id: 6,
            label: '二级 2-2'
          }]
        }, {
          id: 3,
          label: '一级 3',
          children: [{
            id: 7,
            label: '二级 3-1'
          }, {
            id: 8,
            label: '二级 3-2'
            }]
}]
```
这种结构用来展示是极方便的且符合直觉的。但是层级的结构想要准确的寻找一个值是很麻烦的事。

所以要写个函数在拿到Tree结构数据后再生成一份Map结构的数据。并且在添加或者删除数据时手动更新，以保证两份引用的一致性。

函数大概长这样
```js
getMap = (arr) => {
            return arr.reduce((acc, val) => {
                acc[val.id] = val

                if (val.children) {
                    acc = Object.assign(acc, this.getMap(val.children))
                }

                return acc
            }, {})
        }
```

生成的map数据是这样

```js
{
    "1": {
        "id": 1,
        "label": "一级 1",
        "children": [
            {
                "id": 4,
                "label": "二级 1-1",
                "children": [
                    {
                        "id": 9,
                        "label": "三级 1-1-1"
                    },
                    {
                        "id": 10,
                        "label": "三级 1-1-2"
                    }
                ]
            }
        ]
    },
    "2": {
        "id": 2,
        "label": "一级 2",
        "children": [
            {
                "id": 5,
                "label": "二级 2-1"
            },
            {
                "id": 6,
                "label": "二级 2-2"
            }
        ]
    },
    "3": {
        "id": 3,
        "label": "一级 3",
        "children": [
            {
                "id": 7,
                "label": "二级 3-1"
            },
            {
                "id": 8,
                "label": "二级 3-2"
            }
        ]
    },
    "4": {
        "id": 4,
        "label": "二级 1-1",
        "children": [
            {
                "id": 9,
                "label": "三级 1-1-1"
            },
            {
                "id": 10,
                "label": "三级 1-1-2"
            }
        ]
    },
    "5": {
        "id": 5,
        "label": "二级 2-1"
    },
    "6": {
        "id": 6,
        "label": "二级 2-2"
    },
    "7": {
        "id": 7,
        "label": "二级 3-1"
    },
    "8": {
        "id": 8,
        "label": "二级 3-2"
    },
    "9": {
        "id": 9,
        "label": "三级 1-1-1"
    },
    "10": {
        "id": 10,
        "label": "三级 1-1-2"
    }
}
```

展示用Tree，查找用Map。美滋滋
