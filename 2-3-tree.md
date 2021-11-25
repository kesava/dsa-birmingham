## `2-3 Tree`
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

const insertInto3nodeleaf = (t, v) => {
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

const insertInto222 = (t, v) => {
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

const insert = (t, v) => {
  if (isEmpty(t)) return make2nodeLeaf(v);
  if (is2nodeleaf(t)) {
    let {x} = t;
    return x < v ? make3nodeLeaf(x, v) : make3nodeLeaf(v, x);
  }
  if (is3nodeleaf(t)) return insertInto3nodeleaf(t, v);
  if (isNodeType(t, '222')) return insertInto222(t, v);
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
  if (isNodeType(t, '3232')) {
    let {x,y,l,m,r} = t;
    if (v <= x) return make3Node(x, y, (v < l.x) ? make3nodeLeaf(v, l.x) : make3nodeLeaf(l.x, v), m, r);
    if (v >= y) return make3Node(x, y, l, m, (v > r.x) ? make3nodeLeaf(r.x, v) : make3nodeLeaf(v, r.x) );
    if (v <= m.x) return make3Node(x,m.y,l, make3nodeLeaf(v,m.x), make3nodeLeaf(y, r.x));
    if (v >= m.y) return make3Node(x,v,l, m, make3nodeLeaf(y, r.x));
    return make3Node(x,m.y,l, make3nodeLeaf(m.x, v), make3nodeLeaf(y, r.x));
  }
  if (isNodeType(t, '3223')) {
    let {x,y,l,m,r} = t;
    if (v <= x) return make3Node(x, y, (v < l.x) ? make3nodeLeaf(v, l.x) : make3nodeLeaf(l.x, v), m, r);
    if (v <= y) return make3Node(x, y, l, (v > m.x) ? make3nodeLeaf(m.x, v) : make3nodeLeaf(v, m.x), r);
    if (v <= r.x) return make3Node(x, v, l, make3nodeLeaf(m.x, y), r);
    if (v >= r.y) return make3Node(x, r.x, l, make3nodeLeaf(m.x, y), make3nodeLeaf(r.y, v));
    return make3Node(x, r.x, l, make3nodeLeaf(m.x, y), make3nodeLeaf(v, r.y));
  }
  if (isNodeType(t, '3233')) {
    let {x,y,l,m,r} = t;
    if (v <= x) return make3Node(x, y, (v < l.x) ? make3nodeLeaf(v, l.x) : make3nodeLeaf(l.x, v), m, r);
    if (v <= y) return make2Node(v, make3Node(x, make2nodeLeaf(l.x), make2nodeLeaf(m.x)), make3Node(y, make2nodeLeaf(m.y), r));
    return make2Node(y, make3Node(x, make2nodeLeaf(l.x), m), make2Node(v, r, EmptyTree));
  }
      
}

const t1 = make2Node(5, make2nodeLeaf(2), make3nodeLeaf(7, 9));
t1
const t2 = insert(insert(insert(insert(insert(EmptyTree, 30), 70), 50), 10), 20)
t2
const t3 = insert(t2, 85);
t3
const t4 = insert(t3, 88);
console.log({ t4 });
const t5 = insert(t4, 68);
console.log({ t5 });
const t6 = insert(t5, 42);
console.log({ t6 });
/*

*/




```
