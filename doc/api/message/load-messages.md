## .loadMessages( json )

Load messages data.

The first level of keys must be locales. For example:

```
{
  en: {
    hello: "Hello"
  },
  pt: {
    hello: "Olá"
  }
}
```

ICU MessageFormat pattern is supported: variable replacement, gender and plural
inflections. For more information see [`.messageFormatter( path ) ➡ function([
variables ])`](./message-formatter.md).

### Parameters

**json**

JSON object of messages data.

### Example

```javascript
Globalize.loadMessages({
  pt: {
    greetings: {
      hello: "Olá",
      bye: "Tchau"
    }
  }
});

Globalize( "pt" ).formatMessage( "greetings/hello" );
➡ Olá
```

Use Arrays as a convenience for multiple lines strings.

```javascript
Globalize.loadMessages({
  en: {
    task: [
      "You have {0, plural,",
      "    one {one task}",
      "  other {{1} tasks}",
      "} remaining"
    ]
  }
});

Globalize( "en" ).formatMessage( "task", 1000, Globalize( "en" ).formatNumber( 1000 ) );
➡ "You have 1,000 tasks remaining"
```

It's possible to inherit messages, for example:

```javascript
Globalize.loadMessages({
  root: {
    amen: "Amen"
  },
  pt: {
    amen: "Amém"
  }
});

Globalize( "pt-PT" ).formatMessage( "amen" ); // "Amém"
Globalize( "de" ).formatMessage( "amen" ); // "Amen"
Globalize( "en" ).formatMessage( "amen" ); // "Amen"
Globalize( "en-GB" ).formatMessage( "amen" ); // "Amen"
Globalize( "fr" ).formatMessage( "amen" ); // "Amen"
```

Note that `pt-PT`, `de`, `en`, `en-GB`, and `fr` have never been defined.
`.formatMessage()` inherits `pt-PT` messages from `pt` (`pt-PT` ➡ `pt`), and
it inherits the other messages from root, eg. `en-GB` ➡ `en` ➡ `root`. Yes,
`root` is the last bundle of the parent lookup.

Attention: On browsers, message inheritance only works if the optional
dependency `cldr/unresolved` is loaded.

```html
<script src="cldr/unresolved.js"></script>
```
