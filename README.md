<p align="center">
  <img src="https://raw.githubusercontent.com/Exelord/ember-custom-actions/master/logo.png" alt="Ember Custom Actions Logo" width="100%">

  <a href='https://travis-ci.org/Exelord/ember-custom-actions'><img src="https://travis-ci.org/Exelord/ember-custom-actions.svg?branch=master" alt="Dependency Status" /></a> <a href='https://gemnasium.com/github.com/Exelord/ember-custom-actions'><img src="https://gemnasium.com/badges/github.com/Exelord/ember-custom-actions.svg" alt="Dependency Status" /></a>
</p>

The Ember add-on lets you call custom actions to your JSON API server.
Ember Custom Actions is compatible with Ember 2.8 (and higher) applications..

# Getting started

## Installation

`ember install ember-custom-actions`

## Documentation

### Configuration

You can define your custom default options in your `config/environment.js` file.

``` js
module.exports = function(environment) {
  var ENV = {
    'ember-api-actions': {
      type: 'PUT',
      ajaxOptions: {},
      pushToStore: false,
      normalizeOperation: ''
    },
  };

  return ENV;
}
```
#### `type`
default type of the request (GET, PUT, POST, DELETE, etc..)

#### `urlType`
Base of the URL that's generated for the action. If not defined `urlType` is equal `type`;

#### `ajaxOptions`
you can define your own ajax options for example headers.

#### `pushToStore`
if you want to push the recived data to the store mark this option to `true` and change your application `JSONAPISerializer` to:
``` js
// app/serializers/application.js

import { JSONAPISerializer } from 'ember-custom-actions';
export default JSONAPISerializer.extend();
```
#### `normalizeOperation`
You can define how your outgoing data should be serialized.

Example of data:
```js
{
  firstParam: 'My Name',
  colors: { rubyRed: 1, blueFish: 3 }
}
```
using a `dasherize` transformer our request data will be look like:

```js
{
  first-param: 'My Name',
  colors: { ruby-red: 1, blue-fish: 3 }
}
```
It's great for API with request data format restrictions.

**Available transformers:**
  - camelize
  - capitalize
  - classify
  - dasherize
  - decamelize
  - underscore


### Model actions
To define custom action like: `posts/1/publish` you can use
`modelAction(path, options)` method with arguments:

`path` - the url of the action in our case `publish`
`options` - optional parameter which will overwrite the configuration options

```js
import Model from 'ember-data/model';
import { modelAction } from 'ember-custom-actions';

export default Model.extend({
  publish: modelAction('publish', { pushToStore: false }),
});

```

#### Usage
```js
let user = this.get('currentUser');
let postToPublish = this.get('store').findRecord('post', 1);
let payload = { publisher: user };

postToPublish.publish(payload).then((status) => {
  alert(`Post has been: ${status}`)
});
```

### Resource actions
To define custom action like: `posts/favorites` you can use
`resourceAction(path, options)` method with arguments:

`path` - the url of the action in our case `favorites`
`options` - optional parameter which will overwrite the configuration options

```js
import Model from 'ember-data/model';
import { resourceAction } from 'ember-custom-actions';

export default Model.extend({
  favorites: resourceAction('favorites', { type: 'GET' }),
});

```

#### Usage
```js
let user = this.get('currentUser');
let emptyPost = this.get('store').createRecord('post');
let payload = { user };

emptyPost.favorites(payload).then((favoritesPosts) => {
  console.log(favoritesPosts);
}).finally(()=>{
  emptyPost.deleteRecord();
});
```

# Development

## Installation

* `git clone <repository-url>` this repository
* `cd ember-custom-actions`
* `npm install`
* `bower install`

## Running

* `ember serve`
* Visit your app at [http://localhost:4200](http://localhost:4200).

## Running Tests

* `npm test` (Runs `ember try:each` to test your addon against multiple Ember versions)
* `ember test`
* `ember test --server`

## Building

* `ember build`

For more information on using ember-cli, visit [https://ember-cli.com/](https://ember-cli.com/).

## Thanks
Big thanks to Mike North and his [Project](https://github.com/mike-north/ember-api-actions) for the initial concept.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/exelord/Monarchy. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the Contributor Covenant code of conduct.

## License

This version of the package is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
