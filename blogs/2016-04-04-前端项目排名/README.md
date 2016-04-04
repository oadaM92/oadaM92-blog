# Github前端项目排名(2016-04-04)
## 一、前言

近几年前端技术日新月异，从 RequireJS 到 AngularJS 再到 React，似乎每天都有新的技术诞生。而大神们总能第一时间获取到最新资讯，在我们还在刀耕火种的年代就使用上各种高新技术，甚是膜拜。

为了赶上时代的脚步，加上昨天闲着蛋痛。。。就打算去 Github 研究一波，看看大家都在干什么。万一找到一个潜力股项目在萌芽阶段，然后我就去读懂它的源代码，努力成为项目主要贡献者，等星星上来之后，不就成为又一个大牛了吗！(想想就好...)。于是项目按星星排序，一个个研究。。。突然发现这样子太笨了，不如写个爬虫把所有信息都捉取过来弄个排行榜吧。正计划中又发现这个网站[Github Rank](http://www.githubrank.com/)，上面写着 *by Github API v3*。原来有 Github 有开放 api 的，还tm写爬虫那不是逗吗。。。



## 二、使用Nodejs生成排行榜

于是乎就用 Nodejs 编写了简单的脚本，自动生成前端项目的排行榜。前端项目指使用语言类型为 JavaScript 、 CSS 和 HTML 的项目。具体代码如下，大家可以复制过去玩玩，只要使用`node index.js`就可以生成最新的前端项目排行榜，当然如果你想自定义排行榜类型的话，修改一下源码，也很简单（代码使用了一些 ES6 的语法，如果大家对 ES6 不太熟悉，强烈推荐查看阮一峰老师的开源电子书 [ECMAScript 6入门](http://es6.ruanyifeng.com/)）。此外要注意的一点是生成的文档为 markdown 格式，所以你还需要一个相关的编辑器查看。 这份代码也可以在我的 [Github](https://github.com/oadaM92/frontend-repos-ranking) 上查看。

```javascript
var https = require('https');
var fs = require('fs');

var options = {
    protocol: 'https:',
    host: 'api.github.com',
    path: '/search/repositories?q=language:javascript+language:css+language:html+stars:>=10000&sort=stars&order=desc&per_page=100',
    method: 'GET',
    headers: {
        'User-Agent': 'frontend-repos-ranking',
    }
};

var getRepos = new Promise(function(resolve, reject) {
    var req = https.request(options, (res) => {
        var data = '';
        res.setEncoding('utf8');
        res.on('data', (chunk) => {
            data += chunk;
        });
        res.on('end', () => {
            resolve(data);
        })
    });

    req.on('error', (e) => {
        console.log(`problem with request: ${e.message}`);
    });

    req.end();
});

getRepos.then((repos) => {
    var markdown = 
`# frontend-repos-ranking
## ${(new Date()).toDateString()}
github上所有前端项目（HTML+CSS+JavaScript）的总排名！
本页面使用nodejs结合github的api自动生成，您可以通过运行\`node index.js\`来生成最新的排行榜。\n\n`;

    markdown += ' Rank | Name(Description) | Star | Language | Created_At \n';

    markdown += ' --- | --- | --- | --- | --- \n';

    repos = JSON.parse(repos);

    repos.items.forEach((item, index) => {

        markdown += `${index+1}|[**${item.full_name}**](${item.html_url})<br><br>${item.description}|${item.stargazers_count}|${item.language}|${item.created_at.split('T')[0]}\n`;

    });

    fs.writeFile('README.md', markdown);
});
```



## 三、排行榜

接下来放出排名前100的前端项目，这样我就可以好好研究一下每一个项目啦（hehe...）。

| Rank | Name(Description)                        | Star   | Language   | Created_At |
| ---- | ---------------------------------------- | ------ | ---------- | ---------- |
| 1    | [**FreeCodeCamp/FreeCodeCamp**](https://github.com/FreeCodeCamp/FreeCodeCamp)<br><br>The http://FreeCodeCamp.com open source codebase and curriculum. Learn to code and help nonprofits. | 101749 | JavaScript | 2014-12-24 |
| 2    | [**twbs/bootstrap**](https://github.com/twbs/bootstrap)<br><br>The most popular HTML, CSS, and JavaScript framework for developing responsive, mobile first projects on the web. | 94459  | CSS        | 2011-07-29 |
| 3    | [**mbostock/d3**](https://github.com/mbostock/d3)<br><br>A JavaScript visualization library for HTML and SVG. | 48331  | JavaScript | 2010-09-27 |
| 4    | [**angular/angular.js**](https://github.com/angular/angular.js)<br><br>HTML enhanced for web apps | 48295  | JavaScript | 2010-01-06 |
| 5    | [**FortAwesome/Font-Awesome**](https://github.com/FortAwesome/Font-Awesome)<br><br>The iconic font and CSS toolkit | 41054  | HTML       | 2012-02-17 |
| 6    | [**facebook/react**](https://github.com/facebook/react)<br><br>A declarative, efficient, and flexible JavaScript library for building user interfaces. | 39549  | JavaScript | 2013-05-24 |
| 7    | [**jquery/jquery**](https://github.com/jquery/jquery)<br><br>jQuery JavaScript Library | 38815  | JavaScript | 2009-04-03 |
| 8    | [**h5bp/html5-boilerplate**](https://github.com/h5bp/html5-boilerplate)<br><br>A professional front-end template for building fast, robust, and adaptable web apps or sites. | 33483  | JavaScript | 2010-01-24 |
| 9    | [**meteor/meteor**](https://github.com/meteor/meteor)<br><br>Meteor, the JavaScript App Platform | 33130  | JavaScript | 2012-01-19 |
| 10   | [**airbnb/javascript**](https://github.com/airbnb/javascript)<br><br>JavaScript Style Guide | 32896  | JavaScript | 2012-11-01 |
| 11   | [**daneden/animate.css**](https://github.com/daneden/animate.css)<br><br>A cross-browser library of CSS animations. As easy to use as an easy thing. | 30965  | CSS        | 2011-10-12 |
| 12   | [**facebook/react-native**](https://github.com/facebook/react-native)<br><br>A framework for building native apps with React. | 29822  | JavaScript | 2015-01-09 |
| 13   | [**getify/You-Dont-Know-JS**](https://github.com/getify/You-Dont-Know-JS)<br><br>A book series on JavaScript. @YDKJS on twitter. | 28689  | JavaScript | 2013-11-16 |
| 14   | [**hakimel/reveal.js**](https://github.com/hakimel/reveal.js)<br><br>The HTML Presentation Framework | 27192  | JavaScript | 2011-06-07 |
| 15   | [**impress/impress.js**](https://github.com/impress/impress.js)<br><br>It's a presentation framework based on the power of CSS3 transforms and transitions in modern browsers and inspired by the idea behind prezi.com. | 26984  | JavaScript | 2011-12-28 |
| 16   | [**moment/moment**](https://github.com/moment/moment)<br><br>Parse, validate, manipulate, and display dates in javascript. | 25424  | JavaScript | 2011-03-01 |
| 17   | [**adobe/brackets**](https://github.com/adobe/brackets)<br><br>An open source code editor for the web, written in JavaScript, HTML and CSS. | 25028  | JavaScript | 2011-12-07 |
| 18   | [**jashkenas/backbone**](https://github.com/jashkenas/backbone)<br><br>Give your JS App some Backbone with Models, Views, Collections, and Events | 24505  | JavaScript | 2010-09-30 |
| 19   | [**Semantic-Org/Semantic-UI**](https://github.com/Semantic-Org/Semantic-UI)<br><br>Semantic is a UI component framework based around useful principles from natural language. | 24411  | JavaScript | 2013-04-08 |
| 20   | [**expressjs/express**](https://github.com/expressjs/express)<br><br>Fast, unopinionated, minimalist web framework for node. | 24289  | JavaScript | 2009-06-26 |
| 21   | [**mrdoob/three.js**](https://github.com/mrdoob/three.js)<br><br>JavaScript 3D library. | 24145  | JavaScript | 2010-03-23 |
| 22   | [**socketio/socket.io**](https://github.com/socketio/socket.io)<br><br>Realtime application framework (Node.JS server) | 24047  | JavaScript | 2010-03-11 |
| 23   | [**blueimp/jQuery-File-Upload**](https://github.com/blueimp/jQuery-File-Upload)<br><br>File Upload widget with multiple file selection, drag&drop support, progress bar, validation and preview images, audio and video for jQuery. Supports cross-domain, chunked and resumable file uploads. Works with any server-side platform (Google App Engine, PHP, Python, Ruby on Rails, Java, etc.) that supports standard HTML form file uploads. | 23220  | JavaScript | 2010-12-01 |
| 24   | [**driftyco/ionic**](https://github.com/driftyco/ionic)<br><br>Advanced HTML5 mobile development framework and SDK. Build incredible mobile apps with web technologies you already know and love. Best friends with AngularJS. | 23142  | JavaScript | 2013-08-20 |
| 25   | [**zurb/foundation-sites**](https://github.com/zurb/foundation-sites)<br><br>The most advanced responsive front-end framework in the world. Quickly create prototypes and production code for sites that work on any kind of device. | 23026  | JavaScript | 2011-10-13 |
| 26   | [**google/material-design-icons**](https://github.com/google/material-design-icons)<br><br>Material Design icons by Google | 23014  | CSS        | 2014-10-08 |
| 27   | [**resume/resume.github.com**](https://github.com/resume/resume.github.com)<br><br>Resumes generated using the GitHub informations | 21789  | JavaScript | 2011-02-06 |
| 28   | [**nodejs/node**](https://github.com/nodejs/node)<br><br>Node.js JavaScript runtime :sparkles::turtle::rocket::sparkles: | 21699  | JavaScript | 2014-11-26 |
| 29   | [**NARKOZ/hacker-scripts**](https://github.com/NARKOZ/hacker-scripts)<br><br>Based on a true story | 21456  | JavaScript | 2015-11-21 |
| 30   | [**necolas/normalize.css**](https://github.com/necolas/normalize.css)<br><br>A collection of HTML element and attribute style-normalizations | 20603  | HTML       | 2011-05-04 |
| 31   | [**gulpjs/gulp**](https://github.com/gulpjs/gulp)<br><br>The streaming build system | 20300  | JavaScript | 2013-07-04 |
| 32   | [**google/material-design-lite**](https://github.com/google/material-design-lite)<br><br>Material Design Lite Components in HTML/CSS/JS | 19866  | HTML       | 2015-01-14 |
| 33   | [**harvesthq/chosen**](https://github.com/harvesthq/chosen)<br><br>Chosen is a library for making long, unwieldy select boxes more friendly. | 19239  | HTML       | 2011-04-18 |
| 34   | [**TryGhost/Ghost**](https://github.com/TryGhost/Ghost)<br><br>Just a blogging platform | 19015  | JavaScript | 2013-05-04 |
| 35   | [**nnnick/Chart.js**](https://github.com/nnnick/Chart.js)<br><br>Simple HTML5 Charts using the <canvas> tag | 18875  | JavaScript | 2013-03-17 |
| 36   | [**jashkenas/underscore**](https://github.com/jashkenas/underscore)<br><br>JavaScript's utility _ belt | 17789  | JavaScript | 2009-10-25 |
| 37   | [**Modernizr/Modernizr**](https://github.com/Modernizr/Modernizr)<br><br>Modernizr is a JavaScript library that detects HTML5 and CSS3 features in the user’s browser. | 17706  | JavaScript | 2009-09-25 |
| 38   | [**ariya/phantomjs**](https://github.com/ariya/phantomjs)<br><br>Scriptable Headless WebKit | 17545  | HTML       | 2010-12-27 |
| 39   | [**Dogfalo/materialize**](https://github.com/Dogfalo/materialize)<br><br>Materialize, a CSS Framework based on Material Design | 17470  | JavaScript | 2014-09-12 |
| 40   | [**caolan/async**](https://github.com/caolan/async)<br><br>Async utilities for node and the browser | 17083  | JavaScript | 2010-06-01 |
| 41   | [**select2/select2**](https://github.com/select2/select2)<br><br>Select2 is a jQuery based replacement for select boxes. It supports searching, remote data sets, and infinite scrolling of results. | 16753  | JavaScript | 2012-03-04 |
| 42   | [**reactjs/redux**](https://github.com/reactjs/redux)<br><br>Predictable state container for JavaScript apps | 16538  | JavaScript | 2015-05-29 |
| 43   | [**tastejs/todomvc**](https://github.com/tastejs/todomvc)<br><br>Helping you select an MV* framework - Todo apps for Backbone.js, Ember.js, AngularJS, and many more | 16505  | JavaScript | 2011-06-03 |
| 44   | [**emberjs/ember.js**](https://github.com/emberjs/ember.js)<br><br>Ember.js - A JavaScript framework for creating ambitious web applications | 16016  | JavaScript | 2011-05-25 |
| 45   | [**vuejs/vue**](https://github.com/vuejs/vue)<br><br>Simple yet powerful library for building modern web interfaces. | 15884  | JavaScript | 2013-07-29 |
| 46   | [**lodash/lodash**](https://github.com/lodash/lodash)<br><br>A modern JavaScript utility library delivering modularity, performance, & extras. | 15689  | JavaScript | 2012-04-07 |
| 47   | [**Prinzhorn/skrollr**](https://github.com/Prinzhorn/skrollr)<br><br>Stand-alone parallax scrolling library for mobile (Android + iOS) and desktop. No jQuery. Just plain JavaScript (and some love). | 15327  | HTML       | 2012-03-18 |
| 48   | [**callemall/material-ui**](https://github.com/callemall/material-ui)<br><br>React Components that Implement Google's Material Design. | 15085  | JavaScript | 2014-08-18 |
| 49   | [**FezVrasta/bootstrap-material-design**](https://github.com/FezVrasta/bootstrap-material-design)<br><br>Material design theme for Bootstrap 3 | 14861  | CSS        | 2014-08-18 |
| 50   | [**babel/babel**](https://github.com/babel/babel)<br><br>Babel is a compiler for writing next generation JavaScript. | 14783  | JavaScript | 2014-09-28 |
| 51   | [**Polymer/polymer**](https://github.com/Polymer/polymer)<br><br>Build modern apps using web components | 14750  | HTML       | 2012-08-23 |
| 52   | [**kenwheeler/slick**](https://github.com/kenwheeler/slick)<br><br>the last carousel you'll ever need | 14153  | JavaScript | 2014-03-24 |
| 53   | [**numbbbbb/the-swift-programming-language-in-chinese**](https://github.com/numbbbbb/the-swift-programming-language-in-chinese)<br><br>中文版 Apple 官方 Swift 教程《The Swift Programming Language》 | 14101  | CSS        | 2014-06-03 |
| 54   | [**mozilla/pdf.js**](https://github.com/mozilla/pdf.js)<br><br>PDF Reader in JavaScript | 14036  | JavaScript | 2011-04-26 |
| 55   | [**balderdashy/sails**](https://github.com/balderdashy/sails)<br><br>Realtime MVC Framework for Node.js | 14032  | JavaScript | 2012-03-18 |
| 56   | [**bower/bower**](https://github.com/bower/bower)<br><br>A package manager for the web | 13899  | JavaScript | 2012-09-07 |
| 57   | [**yahoo/pure**](https://github.com/yahoo/pure)<br><br>A set of small, responsive CSS modules that you can use in every web project. | 13831  | HTML       | 2013-04-22 |
| 58   | [**webpack/webpack**](https://github.com/webpack/webpack)<br><br>A bundler for javascript and friends. Packs many modules into a few bundled assets. Code Splitting allows to load parts for the application on demand. Through "loaders" modules can be CommonJs, AMD, ES6 modules, CSS, Images, JSON, Coffeescript, LESS, ... and your custom stuff. | 13806  | JavaScript | 2012-03-10 |
| 59   | [**alvarotrigo/fullPage.js**](https://github.com/alvarotrigo/fullPage.js)<br><br>fullPage plugin by Alvaro Trigo. Create full screen pages fast and simple | 13559  | JavaScript | 2013-09-20 |
| 60   | [**hammerjs/hammer.js**](https://github.com/hammerjs/hammer.js)<br><br>A javascript library for multi-touch gestures :// You can touch this | 13496  | JavaScript | 2012-03-02 |
| 61   | [**less/less.js**](https://github.com/less/less.js)<br><br>Leaner CSS | 13455  | JavaScript | 2010-02-20 |
| 62   | [**usablica/intro.js**](https://github.com/usablica/intro.js)<br><br>A better way for new feature introduction and step-by-step users guide for your website and project. | 13361  | JavaScript | 2013-03-10 |
| 63   | [**Leaflet/Leaflet**](https://github.com/Leaflet/Leaflet)<br><br> :leaves: JavaScript library for mobile-friendly interactive maps | 13245  | JavaScript | 2010-09-22 |
| 64   | [**defunkt/jquery-pjax**](https://github.com/defunkt/jquery-pjax)<br><br>pushState + ajax = pjax | 13223  | JavaScript | 2011-02-26 |
| 65   | [**google/web-starter-kit**](https://github.com/google/web-starter-kit)<br><br>Web Starter Kit - a workflow for multi-device websites | 13149  | HTML       | 2014-04-07 |
| 66   | [**angular/material**](https://github.com/angular/material)<br><br>Material design for Angular | 13112  | JavaScript | 2014-07-01 |
| 67   | [**bevacqua/dragula**](https://github.com/bevacqua/dragula)<br><br>:ok_hand: Drag and drop so simple it hurts | 13025  | JavaScript | 2015-04-13 |
| 68   | [**t4t5/sweetalert**](https://github.com/t4t5/sweetalert)<br><br>A beautiful replacement for JavaScript's "alert" | 13002  | JavaScript | 2014-09-30 |
| 69   | [**IanLunn/Hover**](https://github.com/IanLunn/Hover)<br><br>A collection of CSS3 powered hover effects to be applied to links, buttons, logos, SVG, featured images and so on. Easily apply to your own elements, modify or just use for inspiration. Available in CSS, Sass, and LESS. | 12995  | CSS        | 2014-01-02 |
| 70   | [**sahat/hackathon-starter**](https://github.com/sahat/hackathon-starter)<br><br>A boilerplate for Node.js web applications | 12812  | CSS        | 2013-11-13 |
| 71   | [**twbs/ratchet**](https://github.com/twbs/ratchet)<br><br>Build mobile apps with simple HTML, CSS, and JavaScript components. | 12763  | CSS        | 2012-08-17 |
| 72   | [**enaqx/awesome-react**](https://github.com/enaqx/awesome-react)<br><br>A collection of awesome things regarding React ecosystem. | 12707  | JavaScript | 2014-08-06 |
| 73   | [**ajaxorg/ace**](https://github.com/ajaxorg/ace)<br><br>Ace (Ajax.org Cloud9 Editor) | 12460  | JavaScript | 2010-10-27 |
| 74   | [**Unitech/pm2**](https://github.com/Unitech/pm2)<br><br>Production process manager for Node.js apps with a built-in load balancer | 12387  | JavaScript | 2013-05-21 |
| 75   | [**twitter/typeahead.js**](https://github.com/twitter/typeahead.js)<br><br>typeahead.js is a fast and fully-featured autocomplete library | 12320  | JavaScript | 2013-02-19 |
| 76   | [**facebook/immutable-js**](https://github.com/facebook/immutable-js)<br><br>Immutable persistent data collections for Javascript which increase efficiency and simplicity. | 12200  | JavaScript | 2014-07-02 |
| 77   | [**ftlabs/fastclick**](https://github.com/ftlabs/fastclick)<br><br>Polyfill to remove click delays on browsers with touch UIs | 12136  | JavaScript | 2012-02-13 |
| 78   | [**videojs/video.js**](https://github.com/videojs/video.js)<br><br>Video.js - open source HTML5 & Flash video player | 12056  | JavaScript | 2010-05-14 |
| 79   | [**angular-ui/bootstrap**](https://github.com/angular-ui/bootstrap)<br><br>Native AngularJS (Angular) directives for Bootstrap. Smaller footprint (20kB gzipped), no 3rd party JS dependencies (jQuery, bootstrap JS) required. Please read the README.md file before submitting an issue! | 11989  | JavaScript | 2012-10-05 |
| 80   | [**reactjs/react-router**](https://github.com/reactjs/react-router)<br><br>A complete routing solution for React.js | 11918  | JavaScript | 2014-05-16 |
| 81   | [**photonstorm/phaser**](https://github.com/photonstorm/phaser)<br><br>Phaser is a fun, free and fast 2D game framework for making HTML5 games for desktop and mobile web browsers, supporting Canvas and WebGL rendering. | 11820  | JavaScript | 2013-04-12 |
| 82   | [**GitbookIO/gitbook**](https://github.com/GitbookIO/gitbook)<br><br>Modern book format and toolchain using Git and Markdown | 11675  | JavaScript | 2014-03-31 |
| 83   | [**adam-p/markdown-here**](https://github.com/adam-p/markdown-here)<br><br>Google Chrome, Firefox, and Thunderbird extension that lets you write email in Markdown and render it before sending. | 11651  | JavaScript | 2012-05-13 |
| 84   | [**typicode/json-server**](https://github.com/typicode/json-server)<br><br>Get a full fake REST API with zero coding in less than 30 seconds (seriously) | 11442  | JavaScript | 2013-11-27 |
| 85   | [**designmodo/Flat-UI**](https://github.com/designmodo/Flat-UI)<br><br>Flat UI Free - Design Framework (html/css3/less/js). Flat UI is based on Bootstrap, a comfortable, responsive, and functional framework that simplifies the development of websites. | 11415  | JavaScript | 2013-03-03 |
| 86   | [**dhg/Skeleton**](https://github.com/dhg/Skeleton)<br><br>Skeleton: A Dead Simple, Responsive Boilerplate for Mobile-Friendly Development | 11403  | CSS        | 2011-04-30 |
| 87   | [**kriskowal/q**](https://github.com/kriskowal/q)<br><br>A tool for creating and composing asynchronous promises in JavaScript | 11349  | JavaScript | 2010-09-04 |
| 88   | [**zenorocha/clipboard.js**](https://github.com/zenorocha/clipboard.js)<br><br>:scissors: Modern copy to clipboard. No Flash. Just 3kb gzipped :clipboard: | 11323  | JavaScript | 2015-09-18 |
| 89   | [**ecomfe/echarts**](https://github.com/ecomfe/echarts)<br><br>A powerful charting and visualization library for browser | 11102  | JavaScript | 2013-04-03 |
| 90   | [**facebook/flux**](https://github.com/facebook/flux)<br><br>Application Architecture for Building User Interfaces | 11069  | JavaScript | 2014-07-20 |
| 91   | [**angular/angular-seed**](https://github.com/angular/angular-seed)<br><br>Seed project for angular apps. | 11055  | HTML       | 2010-12-24 |
| 92   | [**tobiasahlin/SpinKit**](https://github.com/tobiasahlin/SpinKit)<br><br>A collection of loading indicators animated with CSS | 10942  | CSS        | 2013-12-12 |
| 93   | [**dimsemenov/PhotoSwipe**](https://github.com/dimsemenov/PhotoSwipe)<br><br>JavaScript image gallery for mobile and desktop, modular, framework independent | 10833  | JavaScript | 2011-04-07 |
| 94   | [**rstacruz/nprogress**](https://github.com/rstacruz/nprogress)<br><br>For slim progress bars like on YouTube, Medium, etc | 10817  | JavaScript | 2013-08-20 |
| 95   | [**jasmine/jasmine**](https://github.com/jasmine/jasmine)<br><br>DOM-less simple JavaScript testing framework | 10702  | JavaScript | 2008-12-02 |
| 96   | [**gruntjs/grunt**](https://github.com/gruntjs/grunt)<br><br>Grunt: The JavaScript Task Runner | 10676  | JavaScript | 2011-09-21 |
| 97   | [**petkaantonov/bluebird**](https://github.com/petkaantonov/bluebird)<br><br>:bird: :zap: Bluebird is a full featured promise library with unmatched performance. | 10484  | JavaScript | 2013-09-07 |
| 98   | [**request/request**](https://github.com/request/request)<br><br>Simplified HTTP request client. | 10450  | JavaScript | 2011-01-23 |
| 99   | [**pugjs/pug**](https://github.com/pugjs/pug)<br><br>Jade - robust, elegant, feature rich template engine for Node.js | 10416  | JavaScript | 2010-06-23 |
| 100  | [**madrobby/zepto**](https://github.com/madrobby/zepto)<br><br>Zepto.js is a minimalist JavaScript library for modern browsers, with a jQuery-compatible API | 10378  | HTML       | 2010-09-20 |



## 四、总结

有朝一日我会把上面的全部项目都研究一遍的。。。