```js
//// 6.3 Binary tree
//// 6.3 Binary tree
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

const buildTree = list => list.reduce((acc, element) => insert(element, acc), EmptyTree);
const t1 = buildTree([2,4,6,7,9,33, 3, 5, 333, 98, 43, 21]);
t1
size(t1) // 12


// {
//   v: 7,
//   l: {
//     v: 4,
//     l: {
//       v: 2,
//       l: null,
//       r: { v: 3, l: null, r: null }
//     },
//     r: {
//       v: 6,
//       l: { v: 5, l: null, r: null },
//       r: null
//     }
//   },
//   r: {
//     v: 33,
//     l: {
//       v: 9,
//       l: null,
//       r: { v: 21, l: null, r: null }
//     },
//     r: {
//       v: 98,
//       l: { v: 43, l: null, r: null },
//       r: { v: 333, l: null, r: null }
//     }
//   }
// }

```
