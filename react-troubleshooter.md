# React Troubleshooter

## Common errors

### __invariant violation__:

#### Unable to find element

When the DOM has mutated & you try to modify props / state on a component, you will find that React will complain about not being able to find the element.

Usually, we would expect `React.Component.componentWillUnmount` to be triggered before the component is unmounted from the DOM but this does not happen if the DOM element is destroyed by other methods outside or React's control (`document.body.innerHTML` for example).

React doc's comment about this with AJAX calls [here](https://facebook.github.io/react/tips/initial-ajax.html) is relevant as this issue can affect ajax calls that callback after the DOM has mutated so one fix is to use `React.Component.isMounted()` however if the DOM was completely removed, you may not even have access to `React.Component` anymore.

Where we may encounter potential issues like this, it is better to wrap any functions that modify state or listen for prop changes (`componentWillReceiveProps`) in a try / catch to ensure the application doesn't fail in these edge cases.

## Testing


### The Dom
When testing React components, there are times where you will need to [render into the document](https://facebook.github.io/react/docs/test-utils.html#renderintodocument) however there is a very clear warning in the documenation that states you will need global `window`, `window.document` & `window.document.createElement`.

Luckily, there is [react-tests-globals-setup](https://github.com/holidayextras/react-tests-globals-setup) which solves this problem & is available on npm (`npm install react-tests-globals-setup --save-dev`).

### Shallow renderer
Sometimes we don't really need to test on the DOM so we can simply use the [shallow renderer](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) which is great for simple tests like:
```javascript
var renderOutput = shallowRender(<MyComponent />)
expect(result.type).toBe('div');
```

### Internal react lifecycle method calls
[React lifecycle methods](https://facebook.github.io/react/docs/component-specs.html#lifecycle-methods)

It isn't possible to test the internal methods on a react component as they are meant to remain as internal only calls (like a private methods in a classical programming example) and we don't really want to be testing ReactJS itself anyway!

What we *do* want to be testing however is that whatever we expect to have happened within these calls, has happened.

#### componentWillReceiveProps()

Rather than plagiarize a blog post... here's a good link that explains how to deal with this issue: [http://jaketrent.com/post/test-react-componentwillreceiveprops/](http://jaketrent.com/post/test-react-componentwillreceiveprops/)

Tl;dr: wrap your component in a parent where parent state controls child props, update state on parent to change props on the child, make sure child has a ref so you can test props on `parent.ref.childRef`. 

#### componentDidMount() / componentWillUnmount()

*componentDidMount()*: in order to test anything inside `componentDidMount`, you will first need to `spy` on the things you want to test first before rendering the component (see above on how to test the DOM). From this point, you can do the usual `spy.calledOnce()` etc like you normally would.
tl;dr: componentDidMount is only called after the component has been rendered.

*componentWillUnmount()*: this will only be called on DOM removal. You can force this using [React.unmountComponentAtNode](https://facebook.github.io/react/docs/top-level-api.html#reactdom.unmountcomponentatnode) but this *must be on the parent node* ("If no component was mounted in the container, calling this function does nothing").
eg: `React.unmountComponentAtNode(React.findDOMNode(child).parentNode);`

### Undefined / unexpected count of children
If you are testing for a specific number of children on `this.props.children` you must be aware that children that are not defined will be part of the count:
```javascript
var Parent = React.createClass({
	render: function() {

		var optionalListItemOne, optionalListItemTwo, optionalListItemthree;
		// Some logic here that sets the list items or leaves them as undefined

		return (
			<ul>
				{optionalListItemOne}
				{optionalListItemTwo}
				{optionalListItemThree}
			</ul>
		);
	}
});
```
Will return a count of `3`.


## State

The consensus (at Holiday Extras) is to ensure state is handled at the highest possible component in the heirarchy. Components should remain stateless where possible. In most cases the data required for a component should be passed in from a parent as a `prop`. This allows you to maintain state at a higher level and prevents multiple sources of truth from occurring.
