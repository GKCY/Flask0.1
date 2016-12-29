```Python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run()
```

这是Flask入门的小demo，使用了route( )装饰器告诉什么样的URL才能触发这个函数。

route( )装饰器的源码如下：

```Python
self.view_functions = {}
self.url_map = Map()

def route(self, rule, **options):
    def decorator(f):
        self.add_url_rule(rule, f.__name__, **options)
        self.view_functions[f.__name__] = f
        return f
    return decorator

def add_url_rule(self, rule, endpoint, **options):
    options['endpoint'] = endpoint
    options.setdefault('methods', ('GET',))
    self.url_map.add(Rule(rule, **options))
```

装饰器第一步调用了`add_url_rule（）`函数，如果没有指定`methods`，默认设为`GET`方法。然后将一个`Rule(rule, **options)`对象放进`url_map`这个`Map()`对象里。

`from werkzeug.routing import Map, Rule`

Map和Rule类是从werkzeug工具包导入的。由Rule类创建的路由规则被放入Map中。

view_functions是一个字典，用来注册所有被route( )修饰器修饰的函数。它的键为函数的名字即`f.__name__` ，值为函数对象自己。

这就是0.1版本中`route()`装饰器的实现过程。

