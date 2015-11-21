# Mongoose IP Addresses
## [mongoose][] Plugin for storing IP addresses in your models

## Install
```
npm install --save mongoose-ip-address
```

## Usage

```javascript

// some-model.js

var mongoose = require('mongoose');
var ipAddressPlugin = require('mongoose-ip-address');

var SampleSchema = new Schema({
  ...
});
SampleSchema.plugin(ipAddressPlugin, {fields: ["ip_address", "another_ip_address"]});
var SampleModel = db.model("SampleModel", SampleSchema);

module.exports = SampleModel

```

```javascript

// Using your model

var mongoose = require('mongoose');
var SomeModel = require('./models/some-model.js');

mongoose.connect('mongodb://localhost/cool-db');

someModelInstance = SomeModel.new({
  ip_address: "192.168.1.2" // string, can be IPv4 or IPv6
  ...
})

// Alternately, use =
// string, can be IPv4 or IPv6
someModelInstance.ip_address = "FE80:0000:0000:0000:0202:B3FF:FE1E:8329";


someModelInstance.save()

console.log(someModelInstance.ip_address); // 192.168.1.2
console.log(someModelInstance._ip_address_buf); // Buffer


```

This will add the fields you specify in the `fields` argument as virtuals to your schema. It will actually store the ip address as a `Buffer` in MongoDB, but it will allow you to get and set these fields as strings.

There are many advantages to this, for example, it makes grabbing entries by a range of ip addresses much easier.

```javascript
var ip = require('ip');

var ip_bin = ip.toBuffer('192.168.1.1');

SomeModel.find({
  $and: [
      { '_ip_address_buf' : {$lt: ip_bin}},
      { '_ip_address_buf'   : {$gt: ip_bin}}
  ]},
  function(err, models) {
      if(err) return console.error(err);
      console.log(models);
});
```


## Bugs and pull requests

Please use the github [repository][] to notify bugs and make pull requests.

## License

This software is © 2015 Keenahn Jung, released under the MIT licence. Use it, fork it.

See the LICENSE file for details.

[mongoose]: http://mongoosejs.com
[repository]: http://github.com/keenahn/mongoose-ip-address
