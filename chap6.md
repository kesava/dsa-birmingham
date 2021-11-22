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

const buildTree = list => list.reduce((acc, element) => insert(element, acc), EmptyTree);

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

const binsort = arr => inorder(buildTree(arr));

binsort([...randomNumbers(100, 10000)])
// [ 101, 303, 389, 437, 813, 908, 909, 958, 1047, 1096, 1293, 1306, 1400, 1535, 2013, 2049, 2091, 2187, 2220, 2433, 2462, 2495, 2567, 2573, 2587, 2594, 2780, 2898, 3026, 3060, 3082, 3425, 3529, 3774, 3805, 4169, 4209, 4391, 4891, 4939, 5475, 5623, 5635, 5653, 5670, 5972, 5991, 5993, 6139, 6538, 6549, 6551, 6796, 6810, 6907, 7008, 7019, 7048, 7116, 7144, 7219, 7328, 7353, 7362, 7397, 7428, 7542, 7594, 7609, 7640, 7654, 7713, 7864, 7885, 7897, 8025, 8034, 8215, 8221, 8569, 8776, 8809, 8863, 8867, 9031, 9081, 9087, 9155, 9165, 9253, 9318, 9394, 9603, 9615, 9645, 9743, 9753, 9834, 9915, 9968 ]
```
