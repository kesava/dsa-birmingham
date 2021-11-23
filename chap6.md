## Binary Search Tree
```js
//// 6.3 Binary/Binary Search tree
const EmptyTree = null;
const MakeTree = (v, l, r) => ({
  v, l, r
});
const root = tree => {
  let {v, l, r} = tree;
  return v;
}
const left = tree => {
  let {v, l, r} = tree;
  return l;
}
const right = tree => {
  let {v, l, r} = tree;
  return r;
}

const isEmpty = t => t === EmptyTree;

const Leaf = (v) => MakeTree(v, EmptyTree, EmptyTree); 
const isLeaf = t => {
  if (isEmpty(t)) return false;
  let {v, l, r} = t;
  if (isNull(l) && isNull(r)) return true;
  return false;
}

const t = MakeTree(10, Leaf(3), Leaf(4));
const size = t => isEmpty(t) ? 0 : (1 + size(left(t)) + size(right(t)));
size(t) // 3

const insert = (x, t) => {
  if (isEmpty(t)) {
    return Leaf(x);
  } else {
    let {v, l, r} = t;
    if (x < root(t)) {
      return rotate(MakeTree(v, insert(x, l), r));
    } else {
      return rotate(MakeTree(v, l, insert(x, r)));
    }
  }
}

const del = (t, x) => {
  if (isEmpty(t)) {
    return t;
  } else {
    let {v, l, r} = t;
    if (x === v) {
      if (isEmpty(l)) {
        return r;
      } else if (isEmpty(r)) {
        return l;
      } else {
        return MakeTree(smallestNode(r), l, removeSmallestNode(r));
      }
    } else if (x < v) {
      return rotate(MakeTree(v, del(l, x), r));
    } else {
      return rotate(MakeTree(v, l, del(r, x)));
    }
  }
}

const smallestNode = t => {
  if (isEmpty(t)) return t;
  let {v, l, r} = t;
  if (isNull(l)) {
    return v;
  } else if (isLeaf(l)) {
    return root(l);
  } else {
    return smallestNode(l);
  }
}

const removeSmallestNode = t => {
  if (isEmpty(t)) return t;
  let {v, l, r} = t;
  if (isNull(l)) {
    return r;
  } else if (isLeaf(l)) {
    return MakeTree(v, EmptyTree, r);
  } else {
    return removeSmallestNode(l);
  }
}

const floor = (t, x) => {
  if (isEmpty(t)) return EmptyTree;
  let {v, l, r} = t;
  console.log({v,l,r})
  switch(true) {
    case (x === v): return x;
    case (x < v): return floor(l, x);
    case (x > v): 
      let fl = floor(r, x);
      return (x >= fl) ? fl : x
  }
}

const height = t => {
  if (isEmpty(t)) {
    return 0;
  } else {
    let {v, l, r} = t;
    return Math.max(height(l), height(r)) + 1;
  }
} 

const rotateRight = t => {
  if ((isEmpty(t)) || (isEmpty(left(t)))) {
    return t;
  }
  let {v, l, r: t3} = t;
  let {v: v1, l: t1, r: t2} = l;
  return MakeTree(v1, t1, MakeTree(v, t2, t3));
}

const rotateLeft = t => {
  if ((isEmpty(t)) || (isEmpty(right(t)))) {
    return t;
  }
  let {v, l : t1, r} = t;
  let {v: v1, l: t2, r: t3} = r;
  return MakeTree(v1, MakeTree(v, t1, t2), t3);
}

const rotate = t => {
  if (isEmpty(t)) return t;
  let {v, l, r} = t;
  if ((height(l) - height(r)) >= 2) {
    return rotateRight(t);
  }
  if ((height(r) - height(l)) >= 2) {
    return rotateLeft(t);
  }
  return t;
}

const flatten = arr => {
  let output = [];
  // if (!arr) return [];
  arr.forEach(element => {
    if (Array.isArray(element)) {
      output = output.concat(flatten(element));
    } else if (element !== null) {
      output.push(element);
    }
  });
  return output;
}

const isNull = t => t === null

const inorder = t => {
  if (isEmpty(t)) return t;
  let {v, l, r} = t;
  switch(true) {
    case (!isNull(l) && !isNull(r)):
      return flatten([inorder(l), v, inorder(r)]);
    case (!isNull(l)):
      return flatten([inorder(l), v]);
    case (!isNull(r)):
      return flatten([v, inorder(r)]);
    default:
      return v;
  } 
}

const randomNumbers = (n, max=100) => ({
    [Symbol.iterator]() {
      let i = 0;
      return {
        next() {
        if (i < n) {
          i++;
          return { value: Math.floor(Math.random() * max), done: false }
        } else {
          return { done: true }
        }
      }
    }
    }
});

const str = 'hello world and some more and then even more'



const buildTree = list => list.reduce((acc, element) => insert(element, acc), EmptyTree);
const t2 = buildTree([3,2,4,9])
t2
// {
//   v: 3,
//   l: { v: 2, l: null, r: null },
//   r: { v: 4, l: null, r: { v: 9, l: null, r: null } }
// }


smallestNode(t2) // 2

removeSmallestNode(t2)
// {
//   v: 3,
//   l: null,
//   r: { v: 4, l: null, r: { v: 9, l: null, r: null } }
// }

const findBinaryTree = (tree, element) => {
  if (isEmpty(tree)) return false;
  let {v, l, r} = tree;
  switch(true) {
    case (element === v): return true;
    case (element < v): return findBinaryTree(l, element);
    case (element > v): return findBinaryTree(r, element);
  }
}
const find = (list, element) => findBinaryTree(buildTree(list), element);
find([3,1,7,9,2,8], 3); // true
find([3,1,7,9,2,8], 6); // false
floor(buildTree([3,1,7,9,2,8]), 4);
const r3 = [...randomNumbers(100, 10000)];
const t3 = del(buildTree(r3), r3[1]);
r3.length // 100
inorder(buildTree(r3)).length // 100
inorder(t3).length // 99

const binsort = arr => inorder(buildTree(arr));
binsort([...randomNumbers(100, 10000)])
// [ 101, 303, 389, 437, 813, 908, 909, 958, 1047, 1096, 1293, 1306, 1400, 1535, 2013, 2049, 2091, 2187, 2220, 2433, 2462, 2495, 2567, 2573, 2587, 2594, 2780, 2898, 3026, 3060, 3082, 3425, 3529, 3774, 3805, 4169, 4209, 4391, 4891, 4939, 5475, 5623, 5635, 5653, 5670, 5972, 5991, 5993, 6139, 6538, 6549, 6551, 6796, 6810, 6907, 7008, 7019, 7048, 7116, 7144, 7219, 7328, 7353, 7362, 7397, 7428, 7542, 7594, 7609, 7640, 7654, 7713, 7864, 7885, 7897, 8025, 8034, 8215, 8221, 8569, 8776, 8809, 8863, 8867, 9031, 9081, 9087, 9155, 9165, 9253, 9318, 9394, 9603, 9615, 9645, 9743, 9753, 9834, 9915, 9968 ]
```

##`2-3 Tree`
```js
const make2Node = (x,l=null,r=null) => ({x,l,r});
const make3Node = (x,y,l=null,m=null,r=null) => ({ x,y,l,m,r});
const EmptyTree = null;
const isEmpty = t => t === EmptyTree;

const is2node = n =>  Reflect.has(n, 'x') && !Reflect.has(n, 'y');
const is3node = n =>  Reflect.has(n, 'x') && Reflect.has(n, 'y');

const is2nodeleaf = n => is2node(n) && (n.l === null) && !Reflect.has(n, 'm') && (n.r === null);
const is3nodeleaf = n => is3node(n) && (n.l === null) && (n.m === null) && (n.r === null);

const nodeTypes = ['222', '223', '232', '233', '3222', '3223', '3232', '3322', '3233', '3323', '3332', '3333'];

const isNodeType = (n, type)  => {
  if (isEmpty(n)) return false;
  if (is2nodeleaf(n)) return false;
  if (is3nodeleaf(n)) return false;
  
  if (is2node(n)) {
    let {x, l, r} = n;
    switch (type) {
      case '222': return is2node(l) && is2node(r);
      case '223': return is2node(l) && is3node(r);
      case '232': return is3node(l) && is2node(r);
      case '233': return is3node(l) && is3node(r);
    }
  } else if (is3node(n)) {
    let {x, y, l, m, r} = n;
    switch (type) {
      case '3222': return is2node(l) && is2node(m) && is2node(r);
      case '3223': return is2node(l) && is2node(m) && is3node(r);
      case '3232': return is2node(l) && is3node(m) && is2node(r);
      case '3322': return is3node(l) && is2node(m) && is2node(r);
      case '3233': return is2node(l) && is3node(m) && is3node(r);
      case '3323': return is3node(l) && is2node(m) && is3node(r);
      case '3332': return is3node(l) && is3node(m) && is2node(r);
      case '3333': return is3node(l) && is3node(m) && is3node(r);
    }
  }
  return false;
}

const make2nodeLeaf = x => make2Node(x, EmptyTree, EmptyTree);
const make3nodeLeaf = (x, y) => make3Node(x, y, EmptyTree, EmptyTree, EmptyTree);

const insert = (t, v) => {
  if (isEmpty(t)) return make2nodeLeaf(v);
  if (is2nodeleaf(t)) {
    let {x} = t;
    return x < v ? make3nodeLeaf(x, v) : make3nodeLeaf(v, x);
  }
  if (is3nodeleaf(t)) {
    let {x, y} = t;
    switch (true) {
      case ((x <= v) && (v <= y)): return make2Node(v, make2nodeLeaf(x), make2nodeLeaf(y));
      case ((v <= x) && (x <= y)): return make2Node(x, make2nodeLeaf(v), make2nodeLeaf(y)); 
      case ((v <= y) && (y <= x)): return make2Node(y, make2nodeLeaf(v), make2nodeLeaf(x)); 
      case ((y <= v) && (v <= x)): return make2Node(v, make2nodeLeaf(y), make2nodeLeaf(x));
      case ((y <= x) && (x <= v)): return make2Node(x, make2nodeLeaf(y), make2nodeLeaf(v)); 
      case ((x <= y) && (y <= v)): return make2Node(y, make2nodeLeaf(x), make2nodeLeaf(v)); 
    }
  }
  if (isNodeType(t, '222')) {
    let {x,l,r} = t;
    if (v < x) {
      if (v <= l.x) return make2Node(x, make3Node(v, l.x), r);
      if (v > l.x) return make2Node(x, make3Node(l.x, v), r);
    }
    if (v > x) {
      if (v <= r.x) return make2Node(x, l, make3Node(v, r.x));
      if (v > r.x) return make2Node(x, l, make3Node(r.x, v));
    }
  }
  if (isNodeType(t, '223')) {
    let {x, l, r} = t;
    if (v < x) {
      if (v <= l.x) return make2Node(x, make3Node(v, l.x), r); 
      if (v > l.x) return make2Node(x, make3Node(l.x, v), r);
    }
    if (v > x) {
      if (v <= r.x) return make3Node(x, v, l, make2nodeLeaf(r.x), make2nodeLeaf(r.y));
      if (v >= r.y) return make3Node(x, r.y, l, make2nodeLeaf(r.x), make2nodeLeaf(v));
      // else
      return make3Node(x, r.x, l, make2nodeLeaf(v), make2nodeLeaf(r.y));
    }
  }
  if (isNodeType(t, '232')) {
    let {x, l, r} = t;
    if (v < x) {
      if (v <= l.x) return make3Node(l.x, x, make2nodeLeaf(v), make2nodeLeaf(l.y), r);
      if (v >= l.y) return make3Node(l.y, v, make2nodeLeaf(l.x), make2nodeLeaf(x), r);
      // else
      return make3Node(v, x, make2nodeLeaf(l.x), make2nodeLeaf(l.y), r);
    }
    if (v > x) {
      if (v <= r.x) return make2Node(x, l, make3Node(v, r.x)); 
      if (v > r.x) return make2Node(x, l, make3Node(r.x, v));
    }
  }
  if (isNodeType(t, '3222')) {
    let {x,y,l,m,r} = t;
    if (v <= x) return make3Node(x,y, (v <= l.x) ? make3nodeLeaf(v,l.x) : make3nodeLeaf(l.x, v), m, r);
    if (v >= y) return make3Node(x,y,l,m,(v >= r.x) ? make3nodeLeaf(r.x, v) : make3nodeLeaf(v, r.x));
    return make3Node(x,y,l, (v <= m.x) ? make3nodeLeaf(v,m.x) : make3nodeLeaf(m.x, v), r);
  }
  if (isNodeType(t, '3322')) {
    let {x,y,l,m,r} = t;
    if (v <= x) {
      if (v <= l.x) return make3Node(l.y, y, make3nodeLeaf(v, l.x), make3nodeLeaf(x, m.x), r);
      if (v > l.y ) return make3Node(v, y, l, make3nodeLeaf(x, m.x), r);
      return make3Node(l.y, y, make3nodeLeaf(l.x, v), make3nodeLeaf(x, m.x), r);
    }
    if (v > y) return make3Node(x, y, l, m, (v > r.x) ? make3nodeLeaf(r.x, v) : make3nodeLeaf(v, r.x) );
    return make3Node(x,y,l, (v <= m.x) ? make3nodeLeaf(v,m.x) : make3nodeLeaf(m.x, v), r);
  }
}

const t1 = make2Node(5, make2nodeLeaf(2), make3nodeLeaf(7, 9));
t1
const t2 = insert(insert(insert(insert(insert(EmptyTree, 30), 70), 50), 10), 20)
t2
const t3 = insert(t2, 15);
t3
const t4 = insert(t3, 55);
t4



```
