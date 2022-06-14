# JavaScript Debugging

## Cookies

Posting this as I was struggling with this question as well and finally found a solution.

This works in the Firefox console only as far as I can tell...

1. Set a breakpoint on the very first line of javascript that you know runs on your page after a refresh (before any cookie is set).
2. Then clear your cache and cookies.
3. Paste the below code snippet into your console in Firefox.
4. Remove the breakpoint and resume script execution.

You should see the stack trace for each cookie being created in the console!

**Firefox:**

```
// add cookie property to HTMLDocument constructor
origDescriptor = Object.getOwnPropertyDescriptor(HTMLDocument.prototype, 'cookie');

Object.defineProperty(document, 'cookie', {
    get() {
        return origDescriptor.get.call(this);
    },

    set(value) {
        console.log("%c Cookie is :" + value, "background: #ffffff; color: #000000");
        console.trace();
        // debugger;
        return origDescriptor.set.call(this, value);
    },

    enumerable: true,
    configurable: true
});
```

**Chrome:**
```
function debugAccess(obj, prop, debugGet) {
    let origValue = obj[prop];

    Object.defineProperty(obj, prop, {
        get: function () {
            if (debugGet) debugger;
            return origValue;
        },

        set: function(val) {
            debugger;
            return origValue = val;
        }
    });
};

debugAccess(document, 'cookie');
```

Source: [StackOverflow- Determining origin of cookie which javascript or tracking pixel](https://stackoverflow.com/questions/30946617/determining-origin-of-cookie-which-javascript-or-tracking-pixel)