**���ĵ���2016��10��3�շ���ʱmulter�İ汾��1.2.0�������ܲ������µģ�**
**�������ܴ��ڷ�������������Ҫ�Ķ�ԭ��Ӣ��[README](../README.md)**
**���ĵ������ο���**

# Multer [![Build Status](https://travis-ci.org/expressjs/multer.svg?branch=master)](https://travis-ci.org/expressjs/multer) [![NPM version](https://badge.fury.io/js/multer.svg)](https://badge.fury.io/js/multer) [![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](https://github.com/feross/standard)

Multer ��һ�� node.js �м�������ڴ��� `multipart/form-data` ���͵ı����ݣ�����Ҫ�����ϴ��ļ�������д�� [busboy](https://github.com/mscdex/busboy) ֮�Ϸǳ���Ч��

**ע��**: Multer ���ᴦ���κη� `multipart/form-data` ���͵ı����ݡ�

## ��������

- [English](../README.md) (Ӣ��)
- [�6�3�7�0�2�5](https://github.com/expressjs/multer/blob/master/doc/README-ko.md) (������)

## ��װ

```sh
$ npm install --save multer
```

## ʹ��

Multer �����һ�� `body` ���� �Լ� `file` �� `files` ���� �� express �� `request` �����С�
`body` ������������ı�����Ϣ��`file` �� `files` �������������ϴ����ļ���Ϣ��

����ʹ�÷���:

```javascript
var express = require('express')
var multer  = require('multer')
var upload = multer({ dest: 'uploads/' })

var app = express()

app.post('/profile', upload.single('avatar'), function (req, res, next) {
  // req.file �� `avatar` �ļ�����Ϣ
  // req.body �������ı������ݣ�������ڵĻ�
})

app.post('/photos/upload', upload.array('photos', 12), function (req, res, next) {
  // req.files �� `photos` �ļ��������Ϣ
  // req.body �������ı������ݣ�������ڵĻ�
})

var cpUpload = upload.fields([{ name: 'avatar', maxCount: 1 }, { name: 'gallery', maxCount: 8 }])
app.post('/cool-profile', cpUpload, function (req, res, next) {
  // req.files ��һ������ (String -> Array) �����ļ�����ֵ���ļ�����
  //
  // ���磺
  //  req.files['avatar'][0] -> File
  //  req.files['gallery'] -> Array
  //
  // req.body �������ı������ݣ�������ڵĻ�
})
```

�������Ҫ����һ��ֻ���ı���ı��������ʹ���κ�һ�� multer ���� (`.single()`, `.array()`, `fields()`). ������һ��ʹ�� `.array()` ������:

```javascript
var express = require('express')
var app = express()
var multer  = require('multer')
var upload = multer()

app.post('/profile', upload.array(), function (req, res, next) {
  // req.body �����ı���
})
```

## API

### �ļ���Ϣ

ÿ���ļ������������Ϣ:

Key | Description | Note
--- | --- | ---
`fieldname` | Field name �ɱ�ָ�� |
`originalname` | �û�������ϵ��ļ������� |
`encoding` | �ļ����� |
`mimetype` | �ļ��� MIME ���� |
`size` | �ļ���С���ֽڵ�λ�� |
`destination` | ����·�� | `DiskStorage`
`filename` | ������ `destination` �е��ļ��� | `DiskStorage`
`path` | ���ϴ��ļ�������·�� | `DiskStorage`
`buffer` | һ������������ļ��� `Buffer`  | `MemoryStorage`

### `multer(opts)`

Multer ����һ�� options ����������������� `dest` ���ԣ��⽫���� Multer ���ϴ��ļ��������ġ������ʡ�� options ������Щ�ļ����������ڴ��У���Զ����д����̡�

Ϊ�˱���������ͻ��Multer ���޸��ϴ����ļ�����������������ܿ��Ը���������Ҫ���ơ�

�����ǿ��Դ��ݸ� Multer ��ѡ�

Key | Description
--- | ---
`dest` or `storage` | ������洢�ļ�
`fileFilter` | �ļ���������������Щ�ļ����Ա�����
`limits` | �����ϴ�������
`preservePath` | ��������ļ����������ļ�·��

ͨ����ֻ��Ҫ���� `dest` ����
��������

```javascript
var upload = multer({ dest: 'uploads/' })
```

����������ϴ�ʱ���и���Ŀ��ƣ������ʹ�� `storage` ѡ����� `dest`��Multer ���� `DiskStorage` �� `MemoryStorage` �����洢���棻���⻹���Դӵ�������ø�����õ����档

#### `.single(fieldname)`

����һ���� `fieldname` �������ļ�������ļ�����Ϣ������ `req.file`��

#### `.array(fieldname[, maxCount])`

����һ���� `fieldname` �������ļ����顣�������� `maxCount` �������ϴ��������������Щ�ļ�����Ϣ������ `req.files`��

#### `.fields(fields)`

����ָ�� `fields` �Ļ���ļ�����Щ�ļ�����Ϣ������ `req.files`��

`fields` Ӧ����һ���������飬Ӧ�þ��� `name` �Ϳ�ѡ�� `maxCount` ���ԡ�

Example:

```javascript
[
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 }
]
```

#### `.none()`

ֻ�����ı�������κ��ļ��ϴ������ģʽ�������� "LIMIT\_UNEXPECTED\_FILE" ������� `upload.fields([])` ��Ч��һ����

#### `.any()`

����һ�С��ļ����齫������ `req.files`��

**����:** ȷ�������Ǵ������û����ļ��ϴ���
��Զ��Ҫ�� multer ��Ϊȫ���м��ʹ�ã���Ϊ�����û������ϴ��ļ���һ����û��Ԥ�ϵ���·�ɣ�Ӧ��ֻ������Ҫ�����ϴ��ļ���·����ʹ�á�

### `storage`

#### `DiskStorage`

���̴洢���������������ļ��Ĵ洢��

```javascript
var storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, '/tmp/my-uploads')
  },
  filename: function (req, file, cb) {
    cb(null, file.fieldname + '-' + Date.now())
  }
})

var upload = multer({ storage: storage })
```

������ѡ����ã�`destination` �� `filename`�����Ƕ�������ȷ���ļ��洢λ�õĺ�����

`destination` ������ȷ���ϴ����ļ�Ӧ�ô洢���ĸ��ļ����С�Ҳ�����ṩһ�� `string` (���� `'/tmp/uploads'`)�����û������ `destination`����ʹ�ò���Ĭ�ϵ���ʱ�ļ���

**ע��:** ������ṩ�� `destination` ��һ������������Ҫ���𴴽��ļ��С����ṩһ���ַ�����multer ��ȷ������ļ������㴴���ġ�

`filename` ����ȷ���ļ����е��ļ�����ȷ���� ���û������ `filename`��ÿ���ļ�������Ϊһ������ļ�����������û����չ���� 

**ע��:** Multer ����Ϊ������κ���չ������ĳ���Ӧ�÷���һ���������ļ�����

ÿ���������������������� (`req`) ��һЩ��������ļ�����Ϣ (`file`) ��������ľ�����

ע�� `req.body` ���ܻ�û����ȫ��䣬��ȡ������ͻ��˷����ֶκ��ļ�����������˳��

#### `MemoryStorage`

�ڴ�洢���潫�ļ��洢���ڴ��е� `Buffer` ������û���κ�ѡ��

```javascript
var storage = multer.memoryStorage()
var upload = multer({ storage: storage })
```

��ʹ���ڴ�洢���棬�ļ���Ϣ������һ�� `buffer` �ֶΣ���������������ļ����ݡ�

**����**: ����ʹ���ڴ�洢���ϴ��ǳ�����ļ������߷ǳ����С�ļ����ᵼ�����Ӧ�ó����ڴ����

### `limits`
һ������ָ��һЩ���ݴ�С�����ơ�Multer ͨ���������ʹ�� busboy����ϸ�����Կ����� [busboy's page](https://github.com/mscdex/busboy#busboy-methods) �ҵ���

����ʹ��������Щ:

Key | Description | Default
--- | --- | ---
`fieldNameSize` | field ������󳤶� | 100 bytes
`fieldSize` | field ֵ����󳤶�  | 1MB
`fields` | ���ļ� field ��������� | ����
`fileSize` | �� multipart ���У��ļ���󳤶� (�ֽڵ�λ) | ����
`files` | �� multipart ���У��ļ�������� | ����
`parts` | �� multipart ���У�part ������������(fields + files) | ����
`headerPairs` | �� multipart ���У���ֵ��������� | 2000

���� limits ���԰����������վ�����ܾܾ����� (DoS) ������

### `fileFilter`
����һ������������ʲô�ļ������ϴ��Լ�ʲô�ļ�Ӧ���������������Ӧ�ÿ�������������

```javascript
function fileFilter (req, file, cb) {

  // �������Ӧ�õ��� `cb` ��booleanֵ��
  // ָʾ�Ƿ�Ӧ���ܸ��ļ�

  // �ܾ�����ļ���ʹ��`false`��������:
  cb(null, false)

  // ��������ļ���ʹ��`true`��������:
  cb(null, true)

  // ��������⣬�����������������һ������:
  cb(new Error('I don\'t have a clue!'))

}
```

## ���������

������һ������multer ����Ѵ����͸� express�������ʹ��һ���ȽϺõĴ���չʾҳ ([express��׼��ʽ](http://expressjs.com/guide/error-handling.html))��

������벶׽ multer �����Ĵ���������Լ������м������

```javascript
var upload = multer().single('avatar')

app.post('/profile', function (req, res) {
  upload(req, res, function (err) {
    if (err) {
      // ��������
      return
    }

    // һ�ж���
  })
})
```

## ���ƴ洢����

�������Ҫ�����Լ��Ĵ洢���棬�뿴 [����](/StorageEngine.md) ��

## License

[MIT](LICENSE)
