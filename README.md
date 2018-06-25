# express_session# express_session
1. Продолжаем прошлую работу, выполняя рефакторинг кода в index.js, чтобы содержимое документа со списком логинов
и паролей было доступно после инициализации приложения:
2. Выполняем в командной строке curl -kSL 'https://kodaktor.ru/g/session_pug' -o './views/login.pug' и добавляем маршрут /login
.get('/login', r => r.res.render('login'))
3. Устанавливаем компонент Express: yarn add body-parser, добавляем зависимость в проект: const bodyParser = require('body-parser') и ёё использование в приложение .use(bodyParser.json()) , .use(bodyParser.urlencoded({ extended: true })), и добавляем маршрут для обработки POST-запросом:
.post('/login/check/', r => {
const {body: {login: l}} = r;
const user = items.find(({login}) => login === l);
if(user) {
if(user.password === r.body.pass) { 
r.res.redirect('/users');
} else {
r.res.send('Wrong pass!');
}
} else {
r.res.send('No such user!');
}
})

4. Устанавливаем компонент Express, отвечающий за обработку сессий: yarn add express-session, добавляем зависимость в проект:const session = require('express-session'); а так же добавляем в приложение .use(session({ secret: 'mysecret', resave: true, saveUninitialized: true })) и req.session.auth = 'ok';
5. Добавляем функцию checkAuth промежуточного программного обеспечения (конвейера, middleware) для обработки защищённых маршрутов:
const checkAuth = (r, res, next) => {
if(r.session.auth === 'ok') {
next();
} else {
res.redirect('/login');
}
};

6. Добавляем маршрут /logout для прекращения сессии:
.get('/logout', r => {
r.session.auth = 'neok';
r.res.redirect('/login');
}) 
7. Результаты выполненной работы:

![alt text](https://github.com/stuklovao/express_session/blob/master/шаблон%20(1).jpg) 
![alt text](https://github.com/stuklovao/express_session/blob/master/шаблон%20(2).jpg)
