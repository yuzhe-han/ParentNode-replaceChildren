# ParentNode replaceChildren API

## tl;dr

This API, when invoked, replaces all children of the ParentNode with argument nodes passed-in.
  
## Spec
[Spec issue](https://github.com/whatwg/dom/issues/478)

[Spec](https://dom.spec.whatwg.org/#dom-parentnode-replacechildren)

## Background

Previously, there are a few different ways to replace a node's children with a new set of nodes. It causes confusion for developers on which option is the recommended or most performant.
A related [stackoverlow post](https://stackoverflow.com/questions/3955229/remove-all-child-elements-of-a-dom-node-in-javascript/3955238#3955238)
 describes the various options for clearing out a node's children.

For example:
1. Use node.innerHTML = '' to clear out node's children and call node.append(nodes);
```js
  ParentNode.innerHTML = '';
  ParentNode.append(addedNodes);
```

2. Loop through node's children and call node.removeChild() on each one. Then call node.append(nodes).
```js
  while(parentNode.lastChild) { 
    parentNode.removeChild(parentNode.lastChild);
  }
  parentNode.append(addedNodes);
```
 
This replaceChildren API enables web developers to easily replace node's children without spending cycles on which way is best.

## API

  * Add a `replaceChildren` API on `ParentNode`.
    * Replace all children of node with nodes, while replacing strings in nodes with equivalent Text nodes.
    * Throws a "HierarchyRequestError" DOMException if the constraints of the node tree are violated.

## Example

```html

<body>
<template id="appleOptions">
   <option value="X">Iphone X</option>
   <option value="Max">IPhone Max</option> 
</template>

<template id="googleOptions">
   <option value="p3">Pixel 3</option>
   <option value="p4">Pixel 4</option> 
</template>

<select id="sOS"> 
   <option value="">Select your phone OS</option>
   <option value="apple">Apple</option>
   <option value="google">Google</option> 
</select>
<br />
<select id="sPhone" required ></select>

<script>
    sOS.onchange = (event) => {
      let val = event.target.value;
    	if (val) {
    		sPhone.replaceChildren(
          document.getElementById(val+'Options').content.cloneNode(true)
        );
    	}
    	else {
    		sPhone.replaceChildren();
    	}
    }
</script>
</body>
```

### Polyfill
```js
(function() {	
  // polyfill for replaceChildren
  if( Node.prototype.replaceChildren === undefined) {
    Node.prototype.replaceChildren = function(addNodes) {
      while(this.lastChild) {
        this.removeChild(this.lastChild); 
      }
      if (addNodes !== undefined) {
        this.append(addNodes);
      }
    }
  }
}());
```

