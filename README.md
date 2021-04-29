# ImportBundles.gs

Useful stuff available on npm, bundled up for server-side appsscripts. Use this as library or copy any .gs file in code to your project and use.

Note that the intent here is not to provide entire included packages wholesale, but pieces of them that are particularly useful.

- Methods for making working with date easier
- Methods for making working with objects

## Use as library

Add it with ID: `1flbxyhaMAWh_n_stUeo0GunO_mToDK5wdYR4OJ-3NZzmBX-wsFlqG9Zi`.

```js
  const _ = Bundle.import_('Lodash');
  const obj3 = _.deepClone(obj1, obj2);
  // or
  const datefns = Bundle.import_('DateFns');
  const date = datefns.parseJSON('2000-03-15T05:20:10.123Z');
```

The name passed to `import_` should match the characters of the handle of the filename (everything prior to the `.`).

## Usage

### DateFns

- `format` [documentation](https://date-fns.org/v2.21.1/docs/format)
- `parseJson` [documentation](https://date-fns.org/v2.21.1/docs/parseJSON)
- `parseISO` [documentation](https://date-fns.org/v2.21.1/docs/parseISO)
- `utcToZonedTime` [documentation](https://github.com/marnusw/date-fns-tz#utctozonedtime)

The last function is particularly useful for converting dates into target timezone values.

### Lodash

- `cloneDeep` [documentation](https://lodash.com/docs/4.17.15#cloneDeep)

This function is useful for making copies of nested objects.

### Copy'n'paste

And files in this repo ending with `.Bundle.gs` can be copied and pasted into your own project. Then, to use it:

1. Ensure that `const Import = Object.create(null);` appears at the top of the first `*.Bundle` file you use (by order displayed in the IDE)
2. Any file after that may use `const {Namespace} = Import;` where `Namespace` is the identifer you want to use, i.e. `const {Lodash} = Import` or `const {DateFns} = Import;`.

Note: There is nothing special about this `const {name} = Import;` syntax; it's just a cute name for a regular variable with valid js.

### Documentation

This library is basically just a convenience wrapper for other projects. Consult documentation on their end on usage.

#### On its name

The project's default identifier is `Bundle` but is called "ImportBundles" to emphasize what it's used for.
