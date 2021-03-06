# Ambry [![Build Status](https://travis-ci.org/chicoxyzzy/ambry.svg?branch=master)](https://travis-ci.org/chicoxyzzy/ambry)

Ambry is WebStorage wrapper that makes your life easier. It's target is to improve developer experience and to cover inconsistency in browser implemetations.
Using Ambry you can create storages with namespaces, control scheme version and subscribe to particular storage.

Tests are powered by [BrowserStack](https://www.browserstack.com) — cool cross browser testing tool.

### Installation

```
npm install --save ambry
```

### Creating storage

```javascript
import { createStorage } from 'ambry';

const storage = createStorage(config);
```

`config` consists of:
- `type` — should be `'localStorage'` or `'sessionStorage'` (Default value: `'localStorage'`)
- `namespace` - prefix for your storage in storage key field (Default value: `null`)
- `schemeVersion` - your scheme version which is useful in cases your storage structure could change over time (Default value: `null`)
- `clearOnError` - if `true` clears field when parsing for `getItem()` fails or when `setItem()` is impossible because of storage quota has exceeded (Default value: `true`)
- `doNotThrow` - if `true` then creates dummy storage instead of throwing error. Dummy storage has all methods that standard storage has (Default value: `true`)

### Setting new value or updating value

```javascript
storage.setItem(field, value);
```

If quota is exceeded and you have `clearOnError` set to `true` then storage will be cleared, then scheme version will be set if needed and then `setItem()` will be called another time

### Getting value

```javascript
storage.getItem(field);
```

If value for that field can't be parsed and `clearOnError` is set to `true` then that value will be removed from storage

### Removing value

```javascript
storage.removeItem(field);
```

### Clearing storage

```javascript
storage.clear();
```

### Changing scheme version

```javascript
storage.setSchemeVersion(version);
```

This will clear storage if previous version is not strictly equal to new version

### Subscribing to storage changes

```javascript
const unsubscribe = storage.subscribe(handler);
```

This will subscribe to that particular storage changes only. It returns new function which when called will unsubscribe your handler.

The `handler` is callback function which has such parameters:
- `key` - the field name inside of storage
- `oldValue` - previous value for that key
- `newValue` - new value for that key

### Converting existing localStorage or sessionStorage data to Ambry storage

If you have Web Storage in your app you can just `createStorage()` to work with it as with Ambry storage. You can even split your storage data to multiple Ambry storages using `namespace`

## Contributing

Your contributions are always welcome! Please feel free to create issues.
