- ```Typescript
  const appDiv = document.getElementById('app');
  if (appDiv) {
    appDiv.innerHTML = `<h1>Linked list traversal</h1>`;
  }
  
  function log(value: string) {
    const span = document.createElement('span');
    span.textContent = value + ', ';
    appDiv?.appendChild(span);
  }
  
  interface MyElement {
    name: string;
    render: () => null | MyElement[]
  }
  
  const a1: MyElement = {name: 'a1', render: () => null };
  const b1: MyElement = {name: 'b1', render: () => null};
  const b2: MyElement = {name: 'b2', render: () => null};
  const b3: MyElement = {name: 'b3', render: () => null};
  const c1: MyElement = {name: 'c1', render: () => null};
  const c2: MyElement = {name: 'c2', render: () => null};
  const d1: MyElement = {name: 'd1', render: () => null};
  const d2: MyElement = {name: 'd2', render: () => null};
  
  a1.render = () => [b1, b2, b3];
  b1.render = () => null;
  b2.render = () => [c1];
  b3.render = () => [c2];
  c1.render = () => [d1, d2];
  c2.render = () => null;
  d1.render = () => null;
  d2.render = () => null;
  
  class MyNode {
    instance: MyElement
    child: null | MyNode
    sibling: null | MyNode
    return: null | MyNode
    constructor(instance: MyElement) {
      this.instance = instance
      this.child = null
      this.sibling = null
      this.return = null
    }
  }
  
  const root = new MyNode(a1)
  
  function link(parent: MyNode, child: null | MyElement[]) {
    if (child === null) child = []
    parent.child = child.reduceRight((pre: null | MyNode, current) => {
      const node = new MyNode(current)
      node.return = parent
      node.sibling = pre
      return node
    }, null)
  
    return parent.child
  }
  
  function doWork(node: MyNode) {
    log(node.instance.name)
    const children = node.instance.render()
    return link(node, children)
  }
  
  function walk(o: MyNode) {
    let root = o
    let node = o
  
    while(true) {
      let child = doWork(node)
  
      if (child) {
        node = child
        continue
      }
  
      if (node === root) {
        return
      }
  
      while (!node.sibling) {
        if(!node.return || node.return === root) {
          return
        }
  
        node = node.return
      }
  
      node = node.sibling
  
    }
  }
  
  walk(root)
  ```