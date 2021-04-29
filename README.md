# ImportBundles.gs

Useful code available on npm, bundled up for server-side AppsScripts. Use this as library or copy any `*.Bundle.gs` file in code to your project and use.

Note that the intent here is not to provide entire included packages wholesale, but pieces of them that are particularly useful:

- Methods for making working with dates easier
- Methods for making working with objects easier

This is a library that is built with [this one](https://github.com/classroomtechtools/appsscriptsModules). It uses JavaScript bundling technology to define the methods we need from npm, and bundle them up into a useable format in the AppsScripts runtime.



## Getting Started

### Use as library

Add it with ID: `1flbxyhaMAWh_n_stUeo0GunO_mToDK5wdYR4OJ-3NZzmBX-wsFlqG9Zi`.

```js
  const _ = Bundle.import_('Lodash');
  const obj3 = _.deepClone(obj1, obj2);
  // or
  const datefns = Bundle.import_('DateFns');
  const date = datefns.parseJSON('2000-03-15T05:20:10.123Z');
```

The name passed to `import_` should match the characters of the handle of the filename (everything prior to the `.`).

### Copy'n'paste

And files in this repo ending with `.Bundle.gs` can be copied and pasted into your own project. Then, to use it:

1. Ensure that `const Import = Object.create(null);` appears at the top of the first `*.Bundle` file you use (by order displayed in the IDE)
2. Any file after that may use `const {Namespace} = Import;` where `Namespace` is the identifer you want to use, i.e. `const {Lodash} = Import` or `const {DateFns} = Import;`.

Note: There is nothing special about this `const {name} = Import;` syntax; it's just a cute name for a regular variable with valid js.

## Why?

There are programming challenges that have been solved by other developers in the npm ecosphere. The AppsScripts community would be better served by having those solutions being made available to them.

### Example: Validating date strings

If you have a string representation of a date, and you want to convert it into a date object, so that you can save it to a Google Sheet, you can use this vanilla js:

```js
const str = '2011-10-05T14:48:00.000Z';
const date = new Date(str);
```

And that works brilliantly. But if that string isn't in a format that works for dates, it still works:

```js
const str = '3';
const date = new Date(str);  // yikes!
```

It converts the `"3"` into a valid date. What if you, the programmer, needs to know if it's a valid date or not?

The `date-fns` package solves this by raising a `RangeError` of `Invalid Date`, which is great, so now I can wrap it in a try block:

```js
const datefns = Bundle.import_('DateFns');
const {parseISO} = datefns;
const str = '3';
try {
    const date = parseISO(str);
} except (e) {
    Logger.log('not a valid date!');
}
```

### Example: making deep copies of objects

If you want to create an object that is the combination of two or more, and they are nested, it's a bit of an issue. You could use the `Object.assign` method:

```js
const nestedObj1 = {one: {two: '2'}};
const nestedObj2 = {two: {three: '3'}};
const obj3 = Object.assign(nestedObj1, nestedObj2);
```

For nested objects, it holds references, so if you do this assignment:

```js
obj3.two = 100;
nestedObj1.two == 100;  // true ... yikes!
```

Look at that last line carefully! It's really not what you want!

Or you could use the spread syntax, which does work in the above example. However:

```js
// Object nesting
const x = { a: { a: 1 } }
const y = { a: { b: 1 } }
const z = { ...x, ...y } // { a: { b: 1 } }
```

Instead of `{ a: { a: 1, b: 1 } }` z is `{ a: { b: 1 } }`. Yikes! (Taken from [here](https://stackoverflow.com/questions/27936772/how-to-deep-merge-instead-of-shallow-merge).)

We should be able to do this, right?

```js
var object = {
  'a': [{ 'b': 2 }, { 'd': 4 }]
};
 
var other = {
  'a': [{ 'c': 3 }, { 'e': 5 }]
};

_.merge(object, other);
// => { 'a': [{ 'b': 2, 'c': 3 }, { 'd': 4, 'e': 5 }] }
```

Such functionality is available in the lodash library:

```js
const obj3 = _.merge(nestedObj1, nestedObj2);
obj3.two = 100;
nestedObj1.two == 100;  // false ... yipee
```

This is a solved problem! Let's use it!

## Documentation

This library is basically just a convenience wrapper for other projects. Consult documentation on their end on how to use them.

### DateFns

- `format` [documentation](https://date-fns.org/v2.21.1/docs/format)
- `parseJson` [documentation](https://date-fns.org/v2.21.1/docs/parseJSON)
- `parseISO` [documentation](https://date-fns.org/v2.21.1/docs/parseISO)
- `utcToZonedTime` [documentation](https://github.com/marnusw/date-fns-tz#utctozonedtime)

The last function is particularly useful for converting dates into target timezone values.

### Lodash

- `cloneDeep` [documentation](https://lodash.com/docs/4.17.15#cloneDeep)
- `merge` [documentation](https://lodash.com/docs/4.17.15#merge)

These function is useful for making copies of, or combining, nested objects.


## Misc

The project's default identifier is `Bundle` but is called "ImportBundles" to emphasize what it's used for.
