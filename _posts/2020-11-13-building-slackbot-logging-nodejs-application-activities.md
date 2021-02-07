---
layout: post
title: "Node.js 응용 프로그램 작업을 로깅하기 위한 Slackbot 구축"
author: "Front Log"
thumbnail: "https://blog.logrocket.com/wp-content/uploads/2020/11/slackbot-logging-nodejs-app-activities.png"
tags: 
---


![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/slackbot-logging-nodejs-app-activities.png?fit=730%2C487&ssl=1)

슬랙(Slack)은 기업이 전자 메일을 통신 및 데이터 공유 수단으로 보완하도록 설계된 클라우드 기반 인스턴트 메시징 플랫폼이다. 그것은 또한 웹 후크를 사용하여 채널에 메시지를 보내는 것과 같은 멋진 기능을 가지고 있다.

웹 후크는 어떤 일이 일어날 때 응용 프로그램에서 보내는 자동 페이로드입니다. 기본적으로 응용프로그램이 자동 메시지 또는 정보를 다른 응용프로그램으로 보낼 수 있는 방법입니다.

이 기사에서는 응용 프로그램에서 발생하는 모든 작업을 Node.js를 사용하여 기록하는 Slackbot을 구축하겠습니다. 우리의 봇은 우리의 Node.js 응용프로그램에서 발생하는 모든 활동을 우리가 곧 만들 채널에 기록할 것이다.

### 전제조건

- JavaScript에 대한 기본적인 친숙도
- 개발 시스템에 설치된 Node.js
- 개발 시스템에 설치된 MongoDB
- REST API의 기본 이해

## 프로젝트 설정

봇을 만들기 전에 사용자가 계정을 만들고 로그인할 수 있는 간단한 Node.js 애플리케이션을 만들어야 합니다. 봇은 사용자가 계정을 만들었을 때, 계정을 만드는 동안 오류가 발생했을 때, 올바른 자격 증명을 사용하여 로그인할 때, 그리고 사용자가 잘못된 자격 증명을 사용하여 로그인을 시도했을 때 기록됩니다.

Express 생성기를 사용하여 새 Express 애플리케이션을 생성한 다음 애플리케이션에 필요한 모든 종속성을 설치합니다. 이렇게 하려면 터미널을 열고 다음을 입력하십시오.

```undefined
npx express-generator --no-view
```

애플리케이션 비계를 작성한 후 npm install을 실행하여 프로젝트 종속성을 설치합니다.

```coffeescript
npm i axios bcrypt cors jsonwebtoken mongoose dotenv
```

이러한 파일이 설치되면 다음과 같이 `app.js` 파일을 수정합니다.

```php
require('dotenv').config()
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');
var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');
var app = express();
app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({
    extended: false
}));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));
require("./config/mongoose")(app);

app.use('/', indexRouter);
app.use('/users', usersRouter);
module.exports = app;
```

이제 응용 프로그램을 위해 Mongoose를 설정해야 합니다. `config/mongoose.js` 파일을 생성하고 다음 코드를 추가하십시오.

```js
const mongoose = require("mongoose");
module.exports = app => {
    mongoose.connect("mongodb://localhost:27017/slackbot", {
        useUnifiedTopology: true,
        useNewUrlParser: true,
        useFindAndModify: false
    }).then(() => console.log("conneceted to db")).catch(err => console.log(err))
    mongoose.Promise = global.Promise;
    process.on("SIGINT", cleanup);
    process.on("SIGTERM", cleanup);
    process.on("SIGHUP", cleanup);
    if (app) {
        app.set("mongoose", mongoose);
    }
};
function cleanup() {
    mongoose.connection.close(function () {
        process.exit(0);
    });
}
```

npm start를 실행하면 콘솔에 "connected to db"가 표시되며, 이는 정확히 db가 동작한다고 생각되는 것을 의미합니다.

이제 애플리케이션에 맞는 모델과 컨트롤러를 설정해 보겠습니다. `models/users.js` 파일을 생성하고 다음을 추가하십시오.

```js
const mongoose = require("mongoose");
const bcrypt = require("bcrypt");
const jwt = require("jsonwebtoken");
const Schema = mongoose.Schema;
const userSchema = new Schema({
    name: {
        type: String,
        required: true,
    },
    password: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true,
    },
}, {
    timestamps: true,
});

userSchema.methods.hashPassword = async password => {
    return await bcrypt.hashSync(password, 10);
}
userSchema.methods.compareUserPassword = async (inputtedPassword, hashedPassword) => {
    return await bcrypt.compare(inputtedPassword, hashedPassword)
}
userSchema.methods.generateJwtToken = async (payload, secret, expires) => {
    return jwt.sign(payload, secret, expires)
}
module.exports = mongoose.model("User", userSchema);
```

여기서는 사용자 모델에 대한 간단한 Mongoose 스키마를 생성하고 사용자의 암호를 해시하고, 사용자의 암호를 비교하며, 사용자 로그인 자격 증명이 올바르면 JWT 토큰을 생성하는 기능을 정의합니다.

또한 `controller/users.js` 파일을 생성하고 다음 코드를 추가합니다.

```js
const User = require("../models/user");
exports.createNewUser = async (req, res) => {
    try {
        const user = new User({
            name: req.body.name,
            email: req.body.email,
            phone_number: req.body.phone_number,
            role: req.body.role
        });
        user.password = await user.hashPassword(req.body.password);
        let addedUser = await user.save()
        res.status(200).json({
            msg: "Your Account Has been Created",
            data: addedUser
        })
    } catch (err) {
        console.log(err)
        res.status(500).json({
            error: err
        })
    }
}
exports.logUserIn = async (req, res) => {
    const {
        email,
        password
    } = req.body
    try {
        let user = await User.findOne({
            email: email
        });
        //check if user exit
        if (!user) {
            return res.status(400).json({
                type: "Not Found",
                msg: "Wrong Login Details"
            })
        }
        let match = await user.compareUserPassword(password, user.password);
        if (match) {
            let token = await user.generateJwtToken({
                user
            }, "secret", {
                expiresIn: 604800
            })
            if (token) {
                res.status(200).json({
                    success: true,
                    token: token,
                    userCredentials: user
                })
            }
        } else {
            return res.status(400).json({
                type: "Not Found",
                msg: "Wrong Login Details"
            })
        }
    } catch (err) {
        console.log(err)
        res.status(500).json({
            type: "Something Went Wrong",
            msg: err
        })
    }
}
```

사용자 계정을 만들고 사용자를 로그인하기 위한 기본 기능입니다.

우리는 우리가 만든 컨트롤러를 듣기 위해 `routes/user.js` 파일을 수정해야 한다.

```js
var express = require('express');
const controller = require('../controllers/user')
var router = express.Router();
/* GET users listing. */
router.get('/', function (req, res, next) {
  res.send('respond with a resource');
});

router.post('/register', controller.createNewUser)
router.post('/login', controller.logUserIn)


module.exports = router;
```

POSTMAN을 사용하여 로그인 및 경로 등록을 테스트할 수 있습니다.

## 슬랙봇 구축

봇을 만들기 전에, 우리는 새로운 느슨한 응용 프로그램을 만들어야 합니다. https://api.slack.com으로 이동하여 로그인했는지 확인합니다. Start Building 버튼을 클릭하면 봇 이름을 지정하고 통합하려는 작업영역을 지정할 수 있는 페이지로 이동됩니다.

이 설정을 마친 후 수신 웹 후크 경로로 이동하여 활성화합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/configuring-slack-webhooks.png?resize=730%2C399&ssl=1)

우리는 우리의 작업공간과 소통하기 위해 웹후크 URL이 필요할 것이다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/specifying-webooks-url.png?resize=730%2C476&ssl=1)

Add New Web hook to Workspace 버튼을 클릭합니다. 그러면 봇에서 메시지를 게시할 채널을 선택하라는 메시지가 표시됩니다. 원하는 채널을 선택하고 허용 버튼을 클릭합니다.

![image](https://i2.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/adding-new-webhook.png?resize=730%2C338&ssl=1)

허용을 클릭하면 응용 프로그램에 대한 웹 후크 URL이 생성됩니다. 이것을 복사하여 `.env` 파일에 저장할 수 있습니다.

```undefined
HOOK=<hook>
```

.gitignore 파일에 `.env` 파일을 추가하는 것을 잊지 마십시오.

이제 `util/bot.js` 파일을 만듭니다. 여기서 봇을 설정합니다. 우리는 우리의 Slack API로 요청을 보내는 기능을 가질 것입니다. 이 함수는 `error`와 `payload`의 두 가지 매개 변수를 사용합니다.

웹 후크 URL로 메시지를 전송하기 위해 JSON에서 페이로드(오류 또는 실제 페이로드)를 `application/json` POST 요청의 본문으로 전송합니다. 여기가 악시오스가 들어오는 곳이야.

다음과 같이 `bot.js` 파일을 수정합니다.

```js
const axios = require("axios");
const hook = process.env.HOOK;
exports.sendNotificationToBotty = async (error, log) => {
    try {
        let slackbody;
        if (log) {
            slackbody = {
                mkdwn: true,
                attachments: [{
                    pretext: "Booty Notification",
                    title: "Activity Log",
                    color: "good",
                    text: log,
                }, ],
            };
        } else if (error) {
            slackbody = {
                mkdwn: true,
                attachments: [{
                    pretext: "Booty Notification",
                    title: "Error Notification",
                    color: "#f50057",
                    text: error,
                }, ],
            };
        }
        await axios.post(
            `https://hooks.slack.com/services/${hook}`,
            slackbody
        );
    } catch (err) {
        console.log(err);
    }
};
```

이제 응용 프로그램에서 이 기능을 사용할 수 있습니다. 우리는 슬랙이 제공한 메시지 첨부파일을 사용하여 메시지를 표시합니다.

그래서 이제 우리는 이 모듈을 `controllers/user.js` 파일로 가져와 우리 봇이 활동이 일어날 때 사용자 정의 메시지를 보낼 수 있게 해야 한다. 다음과 같이 `controller/user.js` 파일을 수정합니다.

```js
const User = require("../models/user");
const bot = require("../util/bot")
exports.createNewUser = async (req, res) => {
    try {
        const user = new User({
            name: req.body.name,
            email: req.body.email,
            phone_number: req.body.phone_number,
            role: req.body.role
        });
        user.password = await user.hashPassword(req.body.password);
        let addedUser = await user.save()
        await bot.sendNotificationToBotty(null, `${addedUser.name} Just Created an account with Email as ${addedUser.email}`)
        res.status(200).json({
            msg: "Your Account Has been Created",
            data: addedUser
        })
    } catch (err) {
        console.log(err)
        res.status(500).json({
            error: err
        })
    }
}
exports.logUserIn = async (req, res) => {
    const {
        email,
        password
    } = req.body
    try {
        let user = await User.findOne({
            email: email
        });
        //check if user exit
        if (!user) {
            await bot.sendNotificationToBotty(`Login Attempt with Invalid Credentials ${email} as Email and ${password} as Password`)
            return res.status(400).json({
                type: "Not Found",
                msg: "Wrong Login Details"
            })
        }
        let match = await user.compareUserPassword(password, user.password);
        if (match) {
            let token = await user.generateJwtToken({
                user
            }, "secret", {
                expiresIn: 604800
            })
            if (token) {
                await bot.sendNotificationToBotty(null, `${user.name} Just Logged in`)
                res.status(200).json({
                    success: true,
                    token: token,
                    userCredentials: user
                })
            }
        } else {
            await bot.sendNotificationToBotty(`Login Attempt with Invalid Credentials ${email} as Email and ${password} as Password`)
            return res.status(400).json({
                type: "Not Found",
                msg: "Wrong Login Details"
            })
        }
    } catch (err) {
        await bot.sendNotificationToBotty(`An Error Occured`)
        console.log(err)
        res.status(500).json({
            type: "Something Went Wrong",
            msg: err
        })
    }
}
```

이제 새 사용자가 계정을 만들 때 봇은 사용자 이름과 이메일을 채널에 전송하며 사용자가 로그인할 때도 동일한 현상이 발생합니다.

![image](https://i1.wp.com/blog.logrocket.com/wp-content/uploads/2020/11/slack-messages-activity.png?resize=730%2C266&ssl=1)

사용자가 잘못된 로그인 세부 정보를 사용하여 로그인하려고 할 때와 같은 오류가 발생하면 오류 메시지가 전송됩니다.

## 결론

로깅은 진지하게 고려해야 하는 모든 응용 프로그램의 필수적인 부분입니다. 이 기사에서는 사용자 지정 Node.js 응용 프로그램에서 Slack 웹 후크를 사용하는 방법을 배웠습니다.더 흥미로운 애플리케이션을 구축하는 데 사용될 수 있습니다. 응용 프로그램 소스 코드를 가져오려면 여기를 클릭하십시오.