Ghost 0.10.1 is available! Hot Damn. Click here to upgrade.

Ghost 0.11.1 is available! Hot Damn. Click here to upgrade.

Matrix
Jiaqi Ye

Search
 
Search
New Post
Content
Team
Subscribers
SETTINGS
General
Navigation
Tags
Code Injection
Apps
Labs
VIEW BLOG
Content
NEW POST
更好用的C++静态检查工具——cppcheck
sickcatsickcat Draft
(Untitled)
Jiaqi YeJiaqi Ye Draft
(Untitled)
KarikoKariko Draft
(Untitled)
KarikoKariko Draft
XSS和CSRF那些事（下篇）
JunoJuno Draft
滚动条
EugeneLeeEugeneLee Draft
promise的实现与函数的promise化
William.D.KingWilliam.D.King Published 23 days ago
python 中线程同步机制
langzi989langzi989 Published a month ago
Know this,use this!
KarikoKariko Published a month ago
Agenda魔改之旅——管道通信
DaddyTrapDaddyTrap Published a month ago
Python 单例模式学习
HsuHsu Published a month ago
不只是gdb
longjlongj Published a month ago
C++11初级篇
DaddyTrapDaddyTrap Published a month ago
java静态代码检查工具——pmd的使用笔记
longjlongj Published 2 months ago
Linux 命令初探
HsuHsu Published 2 months ago
Google Test 简介与基本用法
ToooommyToooommy Published 2 months ago
Model-View-Controller
Jiaqi YeJiaqi Ye Published 2 months ago
Makefile的使用
DaddyTrapDaddyTrap Published 2 months ago
XSS和CSRF那些事（上篇）
JunoJuno Published 2 months ago
JavaScript状态模式(State in JavaScript)
GongJunningGongJunning Published 2 months ago
javascript职责链模式
EugeneLeeEugeneLee Published 2 months ago
Node的c/c++扩展模块
William.D.KingWilliam.D.King Published 2 months ago
JS的异步编程的解决方案
returnGirlreturnGirl Published 2 months ago
Javascript函数节流
William.D.KingWilliam.D.King Published 2 months ago
Use C++11 Inheritance Control Keywords to Prevent Inconsistencies in Class Hierarchies
longjlongj Published 2 months ago
使用Sublime3+Chrome搭建可视化Markdown编写环境（无需浏览器插件）
JunoJuno Published 2 months ago
About Matrix
KarikoKariko Page
与文件相关的Blob、File、ArrayBuffer等对象
William.D.KingWilliam.D.King Published 2 months ago
留言板
KarikoKariko Page
TAGS
KarikoKariko Page
ARCHIVES
KarikoKariko Page
Model-View-Controller
简介
MVC是一种“用户交互”软件的构架方法。它把一个应用软件分为三个互相关联的部分，分别是Model、View和Controller。如今，在Web应用开发的时候很经常被使用。

然而，不少人对于这种软件设计的方法有误解。现在我以个人的理解方式来谈一谈这种设计模式。

老师讲的MVC
系统分析与设计上课的时候，老师会讲MVC的软件设计模式。

老师会在黑板上熟练地画出边界类、控制器、数据模型，然后告诉我们下面这个图。



老师讲的系统流程：

用户在前端操作view触发一个事件，假设按了一个button
Controller将处理这一个事件，并且会修改model实体的“状态”
Model的修改放回到Controller，然后Controller修改View
在这个过程中：

View: 负责显示用户交互的界面
Controller: 处理用户在界面中触发的事件（或者是其他事件，比如定时器触发的），然后根据这个事件的类型、用户输入的参数、用户本身的信息，对model数据实体的状态进行修改，最后再操作View进行修改。
Model: 数据实体，保存着这个系统里面所有关键抽象的实体对象。（所谓的“实体对象”就是“实体类是用于对必须存储的信息和相关行为建模的类的实例”）
三个部分之间既有关系，又相对独立，是一种很好设计模式。各个部分各司其职，配合工作，构建出一个逻辑清晰的软件系统。

老师会跟你强调一点，Model和View之间是没有之间关联的，那样画图是要扣分的。

实际上这种观点并不是完全正确的。

Web 应用中的MVC
Although originally developed for desktop computing, model–view–controller has been widely adopted as an architecture for World Wide Web applications in major programming languages. Several commercial and noncommercial web frameworks have been created that enforce the pattern. These software frameworks vary in their interpretations, mainly in the way that the MVC responsibilities are divided between the client and server.[14]

Early web MVC frameworks took a thin client approach that placed almost the entire model, view and controller logic on the server. This is still reflected in popular frameworks such as Ruby on Rails, Django, ASP.NET MVC and Express. In this approach, the client sends either hyperlink requests or form input to the controller and then receives a complete and updated web page (or other document) from the view; the model exists entirely on the server.[14] As client technologies have matured, frameworks such as AngularJS, EmberJS, JavaScriptMVC and Backbone have been created that allow the MVC components to execute partly on the client (also see Ajax).

From Wikipedia

维基百科这段话讲的我很认同，大概的意思是，传统的MVC设计模式是对于“桌面型”应用的，但是随时Web的发展，很多Web应用会选择使用MVC作为其架构。

在讨论web应用的MVC框架的时候，应该分为“服务端”和“客户端”来讨论。

对于传统的Web应用
这种Web应用可以说只有服务端，所谓的前端都是事先写好的html页面。整个请求过程就是：

用户在浏览器发起Http Get/Post 请求
服务端接受到请求
服务端根据用户请求的数据，“计算”出要输出的html页面
服务端响应用户请求
页面渲染，本次请求结束
在这个过程中有太多知识涉及，我们现在讨论服务端处理请求的过程。

传统的Web应用就在这层服务端上做了MVC的架构。Ruby on Rails, Django, ASP.NET MVC and Express 纷纷中枪。

这里拿一个python tornado的项目莉子：

.
├── doc   存放项目需求、分工、项目汇报等文档
├── db    存放数据库表结构
└── src   项目的代码
   ├── handlers   controller
   ├── model      model
   ├── templates  view
   ├── static     静态资源
   ├── application.py  系统应用设置
   ├── server.py   服务器启动脚本
   └── urls.py     路由规则
这个是系统分析与设计的作业，大概的功能就是提供用户购票的功能，非常简单的功能。但是麻雀虽小，五脏俱全。

online ticket system

MVC各个层次在这个项目的分工：

model : 实体对象，存储了user、ticket这两个业务中的关键实体类，并且提供了对于mysql数据库的CRUD功能
view : 视图，在这个项目中使用html+tornado的模板引擎构成，主要负责传给浏览器要渲染的html页面
handler(Controller) : 系统的主要业务逻辑，对每个路由过来的请求进行相应的操作，同时返回页面和数据给请求
对应下图的操作： 

Model Ticket
#coding:utf-8

import database

class Ticket(database.DataBase):  
    def from_id(self, ticketId):
        try:
            ticket = self.db.get(sql);
            self.id = ticket.id
            self.train = ticket.train
            self.departure = ticket.departure
            self.edeparture = ticket.edeparture
            self.destination = ticket.destination
            self.edestination = ticket.edestination
            self.departureTime = ticket.departureTime
            self.arrivalTime = ticket.arrivalTime
            self.price = ticket.price
            self.statu = ticket.statu
        except:
            return
    def buy_ticket(self, userId):
        if self.statu == "sold":
            return False
        self.db.execute("update ticket set ticket.statu = 'sold' where id = {0}".format(self.id))
        self.db.execute("insert into orders (userId, ticketId) values ({0}, {1})".format(userId, self.id))
        return True

    @staticmethod
    def get_all_tickets():
        db = database.DataBase.initial_data_base()
        tickets = db.query(sql);
        return tickets

    @staticmethod
    def get_my_tickets(userId):
        db = database.DataBase.initial_data_base()
        tickets = db.query(sql)
        return tickets
Controller Ticket
#coding:utf-8

import tornado.web  
import model.ticket

class TicketHandler(tornado.web.RequestHandler):  
    def get(self, ticketId):
        ticket = model.ticket.Ticket()
        ticket.from_id(ticketId)

        self.render("ticket.html", ticket = ticket)

    def post(self, ticketId):
        if not self.get_secure_cookie('user'):
            self.render("please login!")

        user = model.user.User()
        user.from_username(self.get_secure_cookie('user'))

        ticket = model.ticket.Ticket()
        ticket.from_id(ticketId)

        if ticket.buy_ticket(user.id):
            self.render("error.html", message = "buy ticket success!")
        else:
            self.render("error.html", message = "failed to buy ticket!")
这个是传统的web开发项目，MVC体现在在整个server-side，可以理解为，server-side是一个black box，输入是用户的请求，输出是html代码。

只传输数据的web项目
“传统”得web项目是通过传递html页面的，那么如果请求的server是一个http “数据接口”接口服务器呢？

实际上很多时候，特别是在做单页面开发，或者是安卓、IOS移动端开发时候，server-side一般都是传输json数据（xml也有，不过现在大家比较喜欢json）

我们接下来看下下面这个python flask的项目，这个项目额外引用了一些技术：

Gevent （python 异步编程）
sqlalchemy (python ORM)
项目组织架构： 

其中，我们看用户修改个人信息的功能：

Model
# coding: utf-8
from runtob import db  
from sqlalchemy.dialects import mysql


class UserProfile(db.Model):  
    __tablename__ = 'user_profile'
    userId = db.Column(db.Integer, primary_key=True)
    nickname = db.Column(db.String(50), nullable=False)
    gender = db.Column(mysql.ENUM('male', 'female', ''), nullable=False)
    phone = db.Column(db.String(30), nullable=False)
    email = db.Column(db.String(50), nullable=False)
    avatar = db.Column(db.String(200), nullable=False)

    def __init__(self, userId, nickname='', gender='', phone='', email='', avatar=''):
        self.userId = userId
        self.nickname = nickname
        self.gender = gender
        self.phone = phone
        self.email = email
        self.avatar = avatar
Controller
@bp.route('/UpdateInformation', methods=['POST'])
@permission
def UpdateInformation():  
    userId = request.form['userId']
    gender = request.form['gender']
    nickname = request.form['nickname']
    phone = request.form['phone']
    email = request.form['email']
    avatar = request.form['avatar']
    user = UserProfile.query.get(userId)
    user.gender = {
            '0':'male',
            '1':'female',
        }.get(gender, '')
    user.phone = phone
    user.nickname = nickname
    user.email = email #todo 校验
    user.avatar = avatar
    db.session.commit()
    return jsonify({'state': 200, 'error': ''})
其中，我们在Model层使用了ORM技术

ORM 对象关系映射

是一种程序设计技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换。从效果上说，它其实是创建了一个可在编程语言里使用的“虚拟对象数据库”。 在这里，我们使用了sqlalchemy，使得数据库的userprofile的表和python model中的userprofile类做了映射，从而只要对user的实例赋值然后提交则可以同步更新数据库。很大程度上减少了sql语句的编写，增强了代码的可维护性。

对于Server Side MVC 的一些疑惑
1. “三层架构”与“MVC”的区别
很多人有这个疑惑。所谓的“三层架构”是表示层、业务逻辑层、数据层。网上对于这个问题有不少回答，我觉得都是扯淡的。 

In software engineering, multitier architecture (often referred to as n-tier architecture) is a client–server architecture in which presentation, application processing, and data management functions are physically separated. The most widespread use of multitier architecture is the three-tier architecture.

来自维基百科对于三层架构的解释。多层架构是软件工程中基于CS的一种架构，他将表示、应用处理和数据管理功能进行物理分离。最常见的多层架构是三层架构。

以下是构架图：



Web开发时候指南： 
在Web开发领域，三层构架通常指前端浏览器渲染的网页、Server上运行的代码以及数据持久化层（一般用数据库和文件系统）

表示层，实质是浏览器帮我们实现了，我们通常会忽略掉。表示层就是浏览器渲染出来的前端页面。
业务逻辑层其实就是WebServer对于前端请求的处理
而数据层则是使用数据库系统以及文件系统实现的数据持久化层
当我们清晰理解MVC和三层架构的真正思想时候，就能理解到这两者的区别所在。

2. CRUD 应该放在 Model 还是 Controller？
观点1：CRUD应该放在Controller层:

用户提交到服务器的http请求，由Controller处理，得到
Controller负责数据业务的CRUD，并且响应
Model 只代表实体对象，如用户，而不提供CRUD方法
通过资源控制器（Resource Controller）实现对数据库等资源的调用
观点2：CRUD应该放在Model层：

Model 层提供数据持久化（通常是对数据库的操作）方法
Controller 接受用户提交的http请求，当产生数据变化时候，调用Model层的方法
Model 层将“实体的状态”通过数据库的持久化来表现
其实，这两种说法都是对的。数据库系统和Web-Server其实是两个独立系统，这点决定了数据库系统的数据持久化实现对于Web-Server应该是透明的。

对于不同的应用有不同的解决方法，但是通常来说，大家为了解决Controller过于臃肿的问题，还是会将数据的CRUD放在model层，通过Controller调用Model的方法实现CRUD。

下面看一个PHP CI框架下的莉子：

Model
class MEntity extends Mx_Model {  
    function insert_entry($data) {
        ...
        database operations
    }
}
Controller
class test extends Mx_Controller {  
    public function insert_entry() {
        $this->load->model('some_model');

        $data['foo'] = $_POST['foo'];
        $data['bar'] = $_POST['bar'];

        $this->some_model->insert_entry($data);

        $this->load->view('some_view'); // Tell the user the data was inserted
    }
}
其中Controller是请求的入口，CI框架中，请求的URL对应为：

POST: /index.php/test/insert_entry  
例子中的Model包含了对数据库的操作，而在Controller中则对应地可以调用model中的代码，层次分明，逻辑清晰。



Model代表了你的数据结构。通常情况下，您的Model类将包含帮助您检索、插入和更新数据库中的信息的函数。 
视图是要被表示给用户的信息。一个视图通常是一个网页，但在CodeIgniter（CI框架），视图也可以是一个页面片段如页眉或页脚。它也可以是一个网页，或任何其他类型的“html块”。 该控制器作为中介之间的模型，视图，和需要处理的HTTP请求并生成一个网页的其他资源。

接下来再看一个express+mongodb的例子： - Model (user 用户实体)

require! ['mongoose']  
ObjectId = mongoose.Schema.Types.ObjectId

UserSchema = new mongoose.Schema {  
  authenticated: Number                                 #验证状态, 1表示已验证
  auth_code: String                                     #验证码
  email: String,                                        #邮箱
  password: String,                                     #密码
  username: String,                                     #用户名
  role: Number,                                         #角色，0为普通用户，1为管理员
  gender: Number,                                       #性别，0汉子，1妹子，2保密
  avatar: String,                                       #头像
  real_name: String,                                    #真实姓名
  phone_num: String,                                    #联系方式
  qq: String,                                           #QQ号码
  weixin: String,                                       #微信号
  host_acts: [{type: ObjectId, ref:'Activity'}],        #已发布的活动
  following_acts: [{type: ObjectId, ref: 'Activity'}],  #已关注的活动
  joining_acts: [{type: ObjectId, ref: 'Activity'}],    #已报名的活动
  tags: [{type: ObjectId, ref: 'Tag'}],                 #订阅的tag
  meta: {
    createAt: {
      type: Date,
      default: Date.now!
    },
    updateAt: {
      type: Date,
      default: Date.now!
    }
  }
}

UserSchema.pre 'save',(next)!->  
  if @.isNew 
    @.meta.createAt = @.meta.updateAt = Date.now!
  else
    @.meta.updateAt = Date.now!
  next!

UserSchema.statics = {  
  fetch: (cb)->
    @ .find {} .sort 'meta.updateAt' .exec cb
  findById: (id, cb)->
    @ .findOne {_id: id} .exec cb
}

module.exports = UserSchema  
Controller (signup)
require! {  
  '../../passport'
  User: '../../models/user'
  Tag: '../../models/tag'
  '../../authMail'
  multer
  '../../imageCropper'
  path
  async
  mongoose
}

DEFAULT_AVATAR = 'images/default_avatar.png'

usernameExists = (username, cb)!->  
  User.find username: username, (err, docs)!->
    if err
      cb err
    else
      cb null, docs.length > 0

emailExists = (email, cb)!->  
  User.find email: email, (err, docs)!->
    if err
      cb err
    else
      cb null, docs.length > 0

createUser = (data, cb)!->  
  data.password = passport.hash data.password
  data.auth_code = Math.random!toString!
  data.authenticated = 0
  data.role = 0
  data.tags = data.tags.split(',')
  data.gender = parseInt(data.gender)
  data.avatar = DEFAULT_AVATAR
  User.create data, (err)!-> cb err, data.auth_code

module.exports = (req, res)!->  
  # x inputs from body
  username = req.body.username
  password = req.body.password
  email = req.body.email
  gender = req.body.gender
  tags = req.body.tags
  # signup
  # check if username exists
  usernameExists username, (err, exists)!->
    if err
      console.log err
      res.status 500 .end!
    else if exists
      res.end 'username exists'
    else
      emailExists email, (err, exists)!->
        if err
          console.log err
          res.status 500 .end!
        else if exists
          res.end 'email exists'
        else
          createUser {
            username: username
            password: password
            email: email
            gender: gender
            tags: tags
          }, (err, authCode)!->
            if err
              console.log err
              res.status 500 .end!
            else
              authMail.send email, authCode
              passport.signin res, username, password, (err)!->
                if err
                  console.log err
                  res.status 500 .end!
                else
                  res.end 'ok'
Mongodb + express + nodejs 在server-side，是一个非常好的配合。上述使用的MVC框架称为“express-mongoose-mvc”。

node中的mongodb 原生支持schema，只要要在代码中定义好schema结构，运行过程之中自动生成数据持久化。这种技术叫做ODM（Object Document Mapper， 对象文档映射），类似数据关系映射。由于Nodejs的数据结构和mongodb的数据结构几乎是一样的，所以采用这个技术的融合性非常地好。

同样地，对于nosql数据库，我们可以使用MVC框架进行我们代码的组织。

总结Web-Server-Side的MVC架构
Web-Server-Side是基于无连接的HTTP协议，server负责处理每个http的请求，最后写会响应给客户端。

Web-Server-Side的MVC结构分为传统页面传输和现在很多应用的数据传输。

对于页面传输形式整个网站的功能，前端和后台都由web-server来实现，用户请求的是html页面。在这里，View是html页面。

对于现在很多应用传输json或者xml数据，其实是省略了View层。在MVC的初始设计中，是为了解决“图形化”用户交互系统的设计，所以，从严格的意义上来说，这样的web-server并不是MVC架构。但是我们依然可以借鉴MVC的思想为我们清晰组织代码。

在Web-Server中Controller实质上就是路由处理，可以分为路由表和具体的路由功能。Model是实体，是这个系统定义的关键抽象类，一般地，应该提供数据库的CRUD方法，也可以尝试使用ORM技术与数据库表结构对接。

Client-Side的MVC
As client technologies have matured, frameworks such as AngularJS, EmberJS, JavaScriptMVC and Backbone have been created that allow the MVC components to execute partly on the client (also see Ajax).

维基百科的第二段讲述了，现代web技术的进步，区别于传统的依赖web服务端传输大段大段的数据，web开发者意识到，在前端的js中可以做很多事情。

html 代码并不是每次都需要传输整个文档：完全可以只传输部分，或者只传输数据，然后通过js代码填充到对应的位置即可
在某些需求中，用户希望不要每次操作都将整个页面刷新，比如需求是单页面应用，SPA（single page application）
甚至，前端可以不需要后台服务器传输任何的html页面，前端完全可以自行根据服务端的数据渲染出html页面
某安卓端应用，或者IOS端应用需要调用http接口，这时候我们不能够返回html页面（事实上可以用webview返回，但是理解应用开发就知道这样做很不好）
对于第一个需求，就出现了Ajax请求，请求服务器的时候，不需要刷新整个页面，而是委托js代码去请求，然后返回之后，js代码通过操作页面的dom元素动态地将后端数据拿回到前端显示。

然而当页面的逻辑复杂起来的时候，你会发现代码组织毫无章法，对于所有事件，几乎都是使用事件->元素操作->请求->返回数据->元素操作。

之前写的YOJ中查询提交代码记录，查询评测报告的前端代码。

需求就是，点击提交记录的按钮之后，弹出模态框，模态框中显示代码的具体内容。 点击提交评测报告按钮，弹出模态框，模态框中显示代码的具体内容。

$(document).ready( function () {
    $("body").on("click", ".source", function(event) {
        target = $(event.target);
        runid = target.data("runid");
        uid = target.data("uid");
        language = target.data("language");
            $.get(
            "\s-getcode", 
            {
                language : language,
                runid : runid,
                uid : uid
            },
            function(data,status) {
                $("#codecontent").text(data)
                $("#myModal").modal().show();
            });
    });

    $("body").on("click", ".report", function(event) {
        target = $(event.target);
        runid = target.data("runid");
            $.get(
            "\s-getreport", 
            {
                runid : runid
            },
            function(data,status) {
                $("#codecontent1").text(data)
                $("#myModal1").modal().show();
            });
    });
});
在处理简单逻辑时候时候，我们这样做完全没有问题。但是当业务需求多起来的时候，这样做代码就会显得杂乱无章，难以维护。

现在在引入前端MVC概念时候，我们引用一种面向对象的设计模式：观察者模式



观察者模式是很好地实现Web前端MVC的一种设计模式（pattern）。简单来说就是观察者订阅一些事件，也就是“暴露”自己事件监听的接口，当其他环境下的对象状态改变时可以调用notify方法来通知观察者状态改变，观察者随即改变自己的状态适应这个改变。

在前端中，我们的MVC框架是这样的：



一个主事件流程：

用户操作view，这个操作被发送到Controller进行处理
Controller 根据业务逻辑，对Model（数据实体）进行修改，这时候Controller有可能会修改View的内容
Model根据Controller对自己的调用修改自己的状态，这个状态修改之后，Model通知View做出相应的变化
我们来看一个Jquery的莉子：

这里有一段html

<head>  
  <link rel="stylesheet" href="./style.css" media="screen" title="no title" charset="utf-8">
  <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js"></script>
  <script type="text/javascript" src="./mvc.js"></script>
</head>

<div class="row container">  
    <div class="col-xs-4">
        <select id="list" class="form-control" size="10"></select>
    </div>
    <div class="col-xs-8">
        <button id="plusBtn" class="btn btn-default btn-block">+</button>
        <button id="minusBtn" class="btn btn-default btn-block">-</button>
    </div>
</div>  
大概的功能就是实现这个列表的增、删。



function Event(sender) {  
    this._sender = sender;
    this._listeners = [];
}

Event.prototype = {  
    attach: function (listener) {
        this._listeners.push(listener);
    },
    notify: function (args) {
        var index;

        for (index = 0; index < this._listeners.length; index += 1) {
            this._listeners[index](this._sender, args);
        }
    }
};

/**
 * The Model. Model stores items and notifies
 * observers about changes.
 */
function ListModel(items) {  
    this._items = items;
    this._selectedIndex = -1;

    this.itemAdded = new Event(this);
    this.itemRemoved = new Event(this);
    this.selectedIndexChanged = new Event(this);
}

ListModel.prototype = {  
    getItems: function () {
        return [].concat(this._items);
    },

    addItem: function (item) {
        this._items.push(item);
        this.itemAdded.notify({
            item: item
        });
    },

    removeItemAt: function (index) {
        var item;

        item = this._items[index];
        this._items.splice(index, 1);
        this.itemRemoved.notify({
            item: item
        });
        if (index === this._selectedIndex) {
            this.setSelectedIndex(-1);
        }
    },

    getSelectedIndex: function () {
        return this._selectedIndex;
    },

    setSelectedIndex: function (index) {
        var previousIndex;

        previousIndex = this._selectedIndex;
        this._selectedIndex = index;
        this.selectedIndexChanged.notify({
            previous: previousIndex
        });
    }
};

/**
 * The View. View presents the model and provides
 * the UI events. The controller is attached to these
 * events to handle the user interraction.
 */
function ListView(model, elements) {  
    this._model = model;
    this._elements = elements;

    this.listModified = new Event(this);
    this.addButtonClicked = new Event(this);
    this.delButtonClicked = new Event(this);

    var _this = this;

    // attach model listeners
    this._model.itemAdded.attach(function () {
        _this.rebuildList();
    });
    this._model.itemRemoved.attach(function () {
        _this.rebuildList();
    });

    // attach listeners to HTML controls
    this._elements.list.change(function (e) {
        _this.listModified.notify({
            index: e.target.selectedIndex
        });
    });
    this._elements.addButton.click(function () {
        _this.addButtonClicked.notify();
    });
    this._elements.delButton.click(function () {
        _this.delButtonClicked.notify();
    });
}

ListView.prototype = {  
    show: function () {
        this.rebuildList();
    },

    rebuildList: function () {
        var list, items, key;

        list = this._elements.list;
        list.html('');

        items = this._model.getItems();
        for (key in items) {
            if (items.hasOwnProperty(key)) {
                list.append($('<option>' + items[key] + '</option>'));
            }
        }
        this._model.setSelectedIndex(-1);
    }
};

/**
 * The Controller. Controller responds to user actions and
 * invokes changes on the model.
 */
function ListController(model, view) {  
    this._model = model;
    this._view = view;

    var _this = this;

    this._view.listModified.attach(function (sender, args) {
        _this.updateSelected(args.index);
    });

    this._view.addButtonClicked.attach(function () {
        _this.addItem();
    });

    this._view.delButtonClicked.attach(function () {
        _this.delItem();
    });
}

ListController.prototype = {  
    addItem: function () {
        var item = window.prompt('Add item:', '');
        if (item) {
            this._model.addItem(item);
        }
    },

    delItem: function () {
        var index;

        index = this._model.getSelectedIndex();
        if (index !== -1) {
            this._model.removeItemAt(this._model.getSelectedIndex());
        }
    },

    updateSelected: function (index) {
        this._model.setSelectedIndex(index);
    }
};

$(function () {
    var model = new ListModel(['PHP', 'NodeJs']),
        view = new ListView(model, {
            'list': $('#list'),
                'addButton': $('#plusBtn'),
                'delButton': $('#minusBtn')
        }),
        controller = new ListController(model, view);

    view.show();
});
当然可以把这部分改成异步的：

Event.prototype = {  
    attach: function (listener) {
        this._listeners.push(listener);
    },
    notify: function (args) {
        var index;
        for (index = 0; index < this._listeners.length; index += 1) {
          // async
          (function(listener, sender, args) {
            setTimeout(function() {
              listener(sender, args);
            }, 0);
          })(this._listeners[index], this._sender, args);
          // this._listeners[index](this._sender, args);
        }
    }
};
这就是前端MVC的主要思想。Model和View从某种意义上是实时同步的。

跪下奉上莫冠钊大神的前端MVC代码例子：link

eventbus.ls这个是观察者模式实现的关键

其中的“事件总线”和观察者模式的subject是一致的

更多与MVC相关
C++/Java 图形界面应用
web前端前端的很多框架
总结
MVC是一种开发思想，应该用更加抽象的层次去理解。其实在任何项目中，不用MVC也不会怀孕。设计模式只是一种思维方法使得你在组织大型代码的时候不至于手忙脚乱。

MVC在存在着不少观点的争论，在这些争论的背后的实质其实是应用环境的不同，或者是应用方式不一样。

综合来讲，只要是实用于自己的项目的设计模式就是好的。

主要参考资料
MVC Wikipedia