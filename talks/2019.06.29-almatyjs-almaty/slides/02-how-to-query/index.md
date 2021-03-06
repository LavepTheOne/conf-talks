# Как запрашивать данные <br/>с GraphQL-сервера?

-----

## Через GraphiQL

![GraphQL Query](./graphiql.png) <!-- .element: class="plain" -->

-----

### GraphiQL производные

- [GraphQL Playground](https://www.graphqlbin.com/v2/6RQ6TM)
- [OneGraphiQL](https://www.onegraph.com/graphiql)
- [Altair](https://altair.sirmuel.design/)

-----

### На июнь 2019, самый удобный – OneGraphiQL

<iframe src="https://www.onegraph.com/graphiql" width="100%" height="720px" />

-----

## Через Postman <span class="red">(боже упаси)</span>

В 7.2 добавили поддержку GraphQL (космолет не меньше 🤣) <!-- .element: class="fragment" -->

<img width="700" alt="" src="https://user-images.githubusercontent.com/1946920/60190996-696e7700-9855-11e9-90de-92d01412c63d.png">

-----

Наряду с Postman'ом еще есть [Insomnia](https://insomnia.rest/graphql/)

-----

## Через CURL в терминале

```bash
curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '{ "query": "{ userMany { name gender age } }" }' \
  https://graphql-compose.herokuapp.com/user/

```

Response:

```js
{"data":{"userMany":[
  {"name":"User 1","gender":"male","age":20},
  {"name":"User 2","gender":"ladyboy","age":28},
  ...
  {"name":"User 10","gender":"female","age":21}
]}}

```

-----

## Через обычный `fetch` в браузере

```js
fetch('https://graphql-compose.herokuapp.com/user/', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ query: '{ userMany { name gender age } }' }),
})
  .then(res => res.json())
  .then(res => console.log(res));

```

-----

## Через чутка поумневший (обёрнутый) `fetch`

- 🛵 Отправили запрос получили ответ <!-- .element: class="fragment" -->
- 🚜 Возможно, по строке запроса закешировали <!-- .element: class="fragment" -->
- 🚕 Возможно, во время запроса сообщали о текущем состоянии, вызывая коллбэки и хуки <!-- .element: class="fragment" -->

-----

## Через чутка поумневший `fetch`

- `graphql-hooks` – simple for React
- `urql` – simple for React
- `graphql.js` – simple for vanilla JS, support fragments
- `Lokka` – simple for vanilla JS
- `graphql-request` – 150 LoC wrapper for fetch
- `apollo-link` – isomorphic fetch with middlewares

<https://github.com/nodkz/conf-talks/tree/master/articles/graphql/clients>

-----

## Пример через `apollo-link`

### на сервере или в браузере – не важно <!-- .element: class="gray" -->

```js
import { execute } from 'apollo-link';
import { HttpLink } from 'apollo-link-http';
import gql from 'graphql-tag';

const link = new HttpLink({ 
  uri: 'https://graphql-compose.herokuapp.com/user/'
});

const query = gql`{ userMany { name gender age } }`;

execute(link, { query }).subscribe(res => {
  document.getElementById("app").innerHTML = JSON.stringify(res);
});

```

<https://codesandbox.io/embed/intelligent-euclid-ipcxf>

-----

## Либо через навёрнутый <br/>GraphQL-клиент

- `Relay` – amazing performance (complexity)
- `ApolloClient` – balance between features and complexity
