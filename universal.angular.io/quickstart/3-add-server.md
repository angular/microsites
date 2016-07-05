```javascript
import * as path from 'path';
import * as express from 'express';
import * as bodyParser from 'body-parser';

// Angular 2 Universal
import 'angular2-universal/polyfills';
import {
provide,
enableProdMode,
expressEngine,
REQUEST_URL,
ORIGIN_URL,
BASE_URL,
NODE_ROUTER_PROVIDERS,
NODE_HTTP_PROVIDERS,
ExpressEngineConfig
} from 'angular2-universal';

// replace this line with your Angular 2 root component
import {App} from './app/app.component';

const app = express();
const ROOT = path.join(path.resolve(__dirname, '..'));

enableProdMode();

// Express View
app.engine('.html', expressEngine);
app.set('views', __dirname);
app.set('view engine', 'html');

app.use(bodyParser.json());

function ngApp(req, res) {
let baseUrl = '/';
let url = req.originalUrl || '/';

let config: ExpressEngineConfig = {
directives: [ App ],

// dependencies shared among all requests to server
platformProviders: [
provide(ORIGIN_URL, {useValue: 'http://localhost:3000'}),
provide(BASE_URL, {useValue: baseUrl}),
],

// dependencies re-created for each request
providers: [
provide(REQUEST_URL, {useValue: url}),
NODE_ROUTER_PROVIDERS,
NODE_HTTP_PROVIDERS,
],

// if true, server will wait for all async to resolve before returning response
async: true,

// if you want preboot, you need to set selector for the app root
// you can also include various preboot options here (explained in separate document)
preboot: { appRoot: 'app' }
};

res.render('index', config);
}

// Serve static files
app.use(express.static(ROOT, {index: false}));

// send all requests to Angular Universal
// if you want Express to handle certain routes (ex. for an API) make sure you adjust this
app.use('*', ngApp);

// Server
app.listen(3000, () => {
console.log('Listening on: http://localhost:3000');
});
```