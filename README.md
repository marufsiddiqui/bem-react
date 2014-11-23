# BemReact

BemReact is a small module on top of [React](https://github.com/facebook/react/).

It:
  * enables you to use bemjson in templates instead of jsx or plain js
  * takes over generating of css classes based on BEM

## Getting Started

```js
var BemReact = require('bem-react');
```

## Building component
BemReact's component is the same as React's component except you should provide `renderBemJson` method instead of `render`.

```js
var Button = BemReact.createClass({
    renderBemJson : function() {
        return {
            block : 'button',
            tag : 'button',
            mods : {
                size : this.props.size,
                pressed : this.state.pressed,
                focused : this.state.focused,
                disabled : this.props.disabled
            },
            children : this.props.text
        };
    }
});

```
## Using component
```js
BemReact.render(
    { block : Button, size : 'xl', disabled : true, text : 'click me' },
    document.body);
// insert to body following html: <button class="button button_size_xl button_disabled">click me</button>
```

### Composition
```js
var Dropdown = BemReact.createClass({
    getInitialState : function() {
        return {
            opened : this.props.opened
        };
    },

    _onButtonClick : function() {
        this.setState({ opened : !this.state.opened });
    },

    renderBemJson : function() {
        return {
            block : 'dropdown',
            mods : {
                opened : this.state.opened,
                disabled : this.props.disabled
            },
            tag : 'div',
            children : [
                {
                    block : Button,
                    key : 'b',
                    disabled : this.props.disabled,
                    text : 'click me',
                    onClick : this._onButtonClick
                },
                {
                    block : Popup,
                    mix : [{ block : 'dropdown', elem : 'popup' }],
                    key : 'p',
                    visible : this.state.opened && !this.props.disabled,
                    children : this.props.children
                }
            ]
        };
    }
});
```

## Top-Level API

API is the similar to original React API:

### createClass(Object specification)

### render(Object componentJson, DOMElement container, [Function callback])

### renderToString(Object componentJson)

### renderToStaticMarkup(Object componentJson)

### unmountComponentAtNode(DOMElement container)

