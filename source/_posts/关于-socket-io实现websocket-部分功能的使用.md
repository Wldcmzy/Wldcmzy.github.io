---
title: 关于[socket.io实现websocket]部分功能的使用
date: 2022-06-11 10:29:30
categories:
  - [基础知识, socket]
tags:
	- socket
	- python
	- javascript
	- websocket
	- socket.io
---

## JavaScript客户端

``` javascript
// 连接请求
socket = io.connect('ws://' + document.domain + ':' + location.port + '/wsmine', {
            reconnectionDelayMax: 10000,
    		//附加在请求里的内容
            query: {
                "cookie": this_cookie
            }
        });
//连接时触发
 socket.on("connect", (rev) => {
            alert("地图生成时间可能较长，请稍等！\n\n扫雷时请不要快速点击，服务器承受不住！");
        })
//断开时触发
socket.on("connect", (rev) => {})

//收到特定消息
socket.on("your massage name", (rev) => {})
```

## python flask_socketio 服务端

启动脚本

``` python
# 文件1
from flask_socketio import SocketIO
clearmind_socketio = SocketIO()

# 文件2
from flask import Blueprint
clearmind_blueprint = Blueprint('main', __name__)

# 启动脚本
from flask import Flask
from BackEnd.objects import clearmind_socketio
from BackEnd.routes import clearmind_blueprint
from flask_cors import CORS

def create_app(debug = True):
    app = Flask(__name__)
    app.debug = debug
    app.config['SECRET_KEY'] = 'clearmind'
    app.register_blueprint(clearmind_blueprint)
    clearmind_socketio.init_app(app)
    CORS(app, supports_credentials=True)
    from BackEnd import events
    return app


if __name__ == '__main__':
    app = create_app()
    clearmind_socketio.run(app, host='0.0.0.0', port=26666)

```

消息规则

``` python
import threading
from flask import session, request
from flask_socketio import emit, join_room, leave_room, disconnect

# 命名空间wslogin 连接时响应
@clearmind_socketio.on('connect', namespace='/wslogin')
def login_connect():
    print_and_log('收到登录请求...')
    try:
        username = request.args['username']
        password = request.args['password']
        '''......'''
        if 4 < 5:
            # 发送reply消息
        	emit('reply', reply)
        else:
            # 发送reply消息
        	emit('reply', 'deny')
    except Exception as e:
        print_and_log(str(type(e)),str(e))
        disconnect()
        return False
   
# 命名空间wslogin 连接断开时响应
@clearmind_socketio.on('disconnect', namespace='/wslogin')
def login_disconnect():
    '''登录连接断开时执行'''
    try : 
        username = request.args['username']
        password = request.args['password']
    except:
        pass
    print_and_log(f'登录连接断开...   {username}')
    
    
    
# 命名空间wsmine 收到click信息时响应
@clearmind_socketio.on('click', namespace='/wsmine')
def mine_click(info):
    '''收到点击地图时间, 把数据中的参数抛给后端处理'''

    try :
        cookie = request.args['cookie']
        data = json.loads(info)
        x, y = data['x'], data['y']
        username, tm = cookie_user_dict[cookie]
    except Exception as e:
        print_and_log('>>> error ' +  str(type(e)) + ' ' + str(e))
        disconnect()
        return False

    '''代码'''
    
    # 广播broadcast消息
    emit('broadcast', json.dumps({'x' : x, 'y' : y, 'color' : color, 'timmer' : timmer, 'username' : username}), broadcast = True)

    '''代码'''
    
    # 广播game end消息
    emit('game end', json.dumps(CM_server.rank()), broadcast = True)
    
    '''代码'''
    
    # 广播args 消息
    emit('args', json.dumps(CM_server.args()), broadcast = True)

```

