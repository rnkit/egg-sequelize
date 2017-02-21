# egg-sequelize

Sequelize plugin in egg

[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][codecov-image]][codecov-url]
[![David deps][david-image]][david-url]
[![Known Vulnerabilities][snyk-image]][snyk-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/egg-sequelize.svg?style=flat-square
[npm-url]: https://npmjs.org/package/egg-sequelize
[travis-image]: https://img.shields.io/travis/eggjs/egg-sequelize.svg?style=flat-square
[travis-url]: https://travis-ci.org/eggjs/egg-sequelize
[codecov-image]: https://codecov.io/gh/eggjs/egg-sequelize/branch/master/graph/badge.svg
[codecov-url]: https://codecov.io/gh/eggjs/egg-sequelize
[david-image]: https://img.shields.io/david/eggjs/egg-sequelize.svg?style=flat-square
[david-url]: https://david-dm.org/eggjs/egg-sequelize
[snyk-image]: https://snyk.io/test/npm/egg-sequelize/badge.svg?style=flat-square
[snyk-url]: https://snyk.io/test/npm/egg-sequelize
[download-image]: https://img.shields.io/npm/dm/egg-sequelize.svg?style=flat-square
[download-url]: https://npmjs.org/package/egg-sequelize

Egg's sequelize plugin.

## Install

```bash
$ npm i --save egg-sequelize

# And one of the following:
$ npm install --save pg pg-hstore
$ npm install --save mysql # For both mysql and mariadb dialects
$ npm install --save tedious # MSSQL
```


## Usage & configuration

- `config.default.js`

```js
exports.sequelize = {
  dialect: 'mysql', // support: mysql, mariadb, postgres, mssql
  database: 'test',
  host: 'localhost',
  port: '3306',
  username: 'root',
  password: '',
};
```

More documents please refer to [Sequelize.js](http://sequelize.readthedocs.io/en/v3/)

## models

Please set sequelize models under `app/model`

## Examples

### Standard

`app/model/user.js`

```js

'use strict'

module.exports = model => {
  return model.define('user', {
    login: model.Sequelize.STRING,
    name: model.Sequelize.STRING(30),
    password: model.Sequelize.STRING(32),
    age: model.Sequelize.INTEGER,
    created_at: model.Sequelize.DATE,
    updated_at: model.Sequelize.DATE,
  }, {
    classMethods: {
      * findByLogin(login) {
        yield this.findOne({ login: login });
      },
    },
  });
};

```

`app/controller/user.js`

```js

'use strict'

module.exports = function* () {
  this.body = yield this.model.user.findByLogin('foo');
};
```

### Associate


`app/model/post.js`

```js

'use strict'

module.exports = model => {
  return sequelize.define('post', {
    name: model.Sequelize.STRING(30),
    user_id: model.Sequelize.INTEGER,
    created_at: model.Sequelize.DATE,
    updated_at: model.Sequelize.DATE,
  }, {
    classMethods: {
      associate(model) {
        model.post.belongsTo(model.user);
      }
    }
  });
};
```

`app/controller/post.js`

```js

'use strict'

module.exports = function* () {
  this.body = yield this.model.post.find({
    'name':'aaa'
  });
}
```
