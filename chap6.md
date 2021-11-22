```
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
```
