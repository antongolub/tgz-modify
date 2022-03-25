# @antongolub/tgz-modify
modify **.tgz** file entries

> Fork of [gforceg/tgz-modify](https://github.com/gforceg/tgz-modify) that brings async api

## Install
```bash
# npm
npm i @antongolub/tgz-modify

# yarn
yarn add @antongolub/tgz-modify
```

## Usage
* Open `./files/package.tgz`. 
* Make some changes to `package/package.json` and remove `README.md` all together.
* Then output to a new .tgz file: `./output/package.tgz`

```javascript 
const tgz_modify = require('@antongolub/tgz-modify')

tgz_modify('files/package.tgz', 'output/package.tgz', (header, data) => {
  switch(header.name) {
    case 'package/package.json':
      let obj = JSON.parse(data)
      obj.name = 'some-other-project'
      obj.author = 'Some Jerk'
      data = JSON.stringify(obj, null, '\t')
      break;
    case 'package/README.md':
      return null // returning null will skip the file.
  }
  return data
})

// to handle `onFinish` event pass an additional callback
tgz_modify('files/package.tgz', 'output/package.tgz', (h, d) => d, (err) => {})

// or just await a promise
await tgz_modify('files/package.tgz', 'output/package.tgz', ...)
```

To overwrite the **.tgz** file, simply use the same filename for the output file: 

```javascript
tgz.modify('./input.tgz', './input.tgz', ...)
```

## License
MIT
