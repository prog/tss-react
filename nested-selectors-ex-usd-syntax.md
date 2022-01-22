# Nested selectors (ex $ syntax)

`tss-react` unlike `jss-react` doesn't support the `$` syntax but there's type safe alternatives that achieve the same results.

In **JSS** you can do:

```tsx
{
  "parent": {
      "padding": 30,
      "&:hover $child": {
          "backgroundColor": "red"
      },
  },
  "child": {
      "backgroundColor": "blue"
  }
}
//...
<div className={classes.parent}>
    <div className={classes.children}>
        Background turns red when the mouse is hover the parent
    </div>
</div>
```

![](https://user-images.githubusercontent.com/6702424/129976981-0637235a-570e-427e-9e77-72d100df0c36.gif)

This is how you would achieve the same result with `tss-react`

```tsx
export function App() {
    const { classes } = useStyles();

    return (
        <div className={classes.parent}>
            <div className={classes.child}>
                Background turns red when mouse is hover the parent.
            </div>
        </div>
    );
}

const useStyles = makeStyles<void, "child">()((_theme, _params, classes) => ({
    "parent": {
        "padding": 30,
        [`&:hover .${classes.child}`]: {
            "backgroundColor": "red",
        },
    },
    "child": {
        "backgroundColor": "blue",
    },
}));
```

An other example:

```tsx
export function App() {
    const { classes, cx } = useStyles({ "color": "primary" });

    return (
        <div className={classes.root}>
            <div className={classes.child}>
                The Background take the primary theme color when the mouse is
                hover the parent.
            </div>
            <div className={cx(classes.child, classes.small)}>
                The Background take the primary theme color when the mouse is
                hover the parent. I am smaller than the other child.
            </div>
        </div>
    );
}

const useStyles = makeStyles<
    { color: "primary" | "secondary" },
    "child" | "small"
>()((theme, { color }, classes) => ({
    "root": {
        "padding": 30,
        [`&:hover .${classes.child}`]: {
            "backgroundColor": theme.palette[color].main,
        },
    },
    "small": {},
    "child": {
        "border": "1px solid black",
        "height": 50,
        [`&.${classes.small}`]: {
            "height": 30,
        },
    },
}));
```

[https://user-images.githubusercontent.com/6702424/144655154-51d0d294-e392-4af5-8802-f3df9aa1b905.mov](https://user-images.githubusercontent.com/6702424/144655154-51d0d294-e392-4af5-8802-f3df9aa1b905.mov)

> WARNING: Nested selectors requires [ES6 Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Proxy) support which [IE doesn't support](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Proxy#browser\_compatibility).\
> It can't be polyfilled ([this](https://github.com/GoogleChrome/proxy-polyfill) will not work) but don't worry, if `Proxy` is not available on a particular browser, no error will be thrown and TSS will still do its work.\
> Only nested selectors won't work.

### Nested selector with the `withStyles` API

{% embed url="https://user-images.githubusercontent.com/6702424/143791304-7705816a-4d25-4df7-9d45-470c5c9ec1bf.mp4" %}