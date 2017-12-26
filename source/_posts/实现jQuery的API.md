---
title: 实现jQuery的API
date: 2017-12-26 11:40:30
tags:
---
## 使用 DOM API 实现jQuery

1. 在window上定义一个全局jquery方法
``` javascript: 
window.jQuery = function(nodeOrSelector) {
        let nodes = {};
        nodes.addClass = function(classes) {
           
        }
        nodes.text = function(text) {
          
        }
        return nodes
    }
```

2. 根据传入参数来查询操作的Node节点
	case-1：nodeOrSelector为字符串的时候，则查询
	case-2：nodeOrSelector为节点的时候，则封装成一个hash
``` javascript: 
if (typeof nodeOrSelector === 'string') {
	    let temp = document.querySelectorAll(nodeOrSelector);
	    for (let i = 0; i < temp.length; i++) {
	        nodes[i] = temp[i];
	    }
	    nodes['length'] = temp.length;
	} else if (nodeOrSelector instanceof Node) {
	    nodes = { 0: nodeOrSelector, length: 1 };
	}
}
```

3. 实现addClass
	case-1: classes为字符串，则直接添加到样式表中
	case-2：class为数组，则循环添加到样式表中
``` javascript: 
nodes.addClass = function(classes) {
    if (typeof classes === 'string') {
        this.classList.add(classes);
    } else {
        classes.foreach((value) => {
            this.classList.add(value);
        })
    }
}
```

4. 获取文本内容或设置文本内容
	case-1：如果text为undefined则获取文本内容
	case-2：如果text不为undefined则设置文本内容
``` javascript: 
nodes.text = function(text) {
    if (text === undefined) {
        let texts = [];
        for (let i = 0; i < nodes.length; i++) {
            texts.push(nodes[i].textContent);
        }
        return texts;
    } else {
        for (let i = 0; i < nodes.length; i++) {
            nodes[i].textContent = text;
        }
    }
}
```
5. 给jquery取一个别名$
6. 验证
	var $div = $('div') //获取jquery对象
	$div.addClass('red') // 给div添加一个‘red’的类 
	$div.text('hello')//给div设置‘hello’文本内容 
