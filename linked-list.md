## Linked List
This is an implementation of a linked list using objects and iterors.
```javascript
const EmptyList = null;
const MakeList = (element, list) => ({
  element,
  list,
  [Symbol.iterator]() {
    let localValue = false;
    let currentN = list;
    let nextN = list;
    return {
      next: function () {
        if (!localValue) {
          localValue = true;
          return { value: element, done: false };
        } else if (nextN) {
          currentN = nextN[Symbol.iterator]();
          nextN = nextN.list;
          return currentN.next();
        } else {
          return { done: true };
        }
      }
    }
  }
});

const linkedList = MakeList(10, MakeList(8, MakeList(5, MakeList(3, EmptyList))));
const spreadableList = [...linkedList]
console.log({ spreadableList });
for(let i of linkedList) {
  console.log({ i })
}

const first = input => {
  let { element, list} = input;
  return element;
};

const rest = input => {
  let { element, list} = input;
  return list;
};

const isEmpty = input => {
  if (input === null) return true;
  return false;
}

first(linkedList)
rest(linkedList)
isEmpty(EmptyList)

const at = (list, i) => {
  let current = -1;
  while (!isEmpty(list)) {
    current++;
    if (current === i) {
      return list;
    }
    list = list.list;
  }
  return EmptyList;
}

at(linkedList, 3); // { element: 3, list: null }

const insertAt = (list, i, x) => {
  const head = list;
  let prev = list;
  let current = 0;
  while(!isEmpty(list)) {
    if (current === i) {
      if (i === 0) {
        return MakeList(x, list);
      } else {
        const temp = MakeList(x, list);
        prev.list = temp;
        return head;
      }
    }
    prev = list;
    list = list.list;
    current++;
  }
  if ((current === i) || (i === -1)) {
    const temp = MakeList(x, list);
    prev.list = temp;
    return head;
  }
  return head;
}

const deleteAt = (list, i) => {
  const head = list;
  let prev = list;
  let current = 0;
  while(!isEmpty(list)) {
    if (current === i) {
      if (i === 0) {
        return list.list;
      } else {
        prev.list = list.list;
        return head;
      }
    }
    prev = list;
    list = list.list;
    current++;
  }
  return head;
}

const a1 = [...insertAt(linkedList, 3, 22)] // [ 10, 8, 5, 22, 3 ]
const a2 = [...deleteAt(linkedList, 2)]
console.log({ a1, a2 })

const concat = (l1, l2) => {
  if (isEmpty(l2)) {
    return l1;
  } else if (isEmpty(l1)) {
    return l2;
  } else {
    return concat(insertAt(l1, -1, first(l2)), rest(l2));
  }
}

console.log({ concat: [...concat(MakeList(3, MakeList(4, MakeList(5, EmptyList))), MakeList(7, MakeList(8, EmptyList)))] }); // { concat: [ 3, 4, 5, 7, 8 ] }

const sum = list => {
  if (isEmpty(list)) {
    return 0;
  } else {
    return first(list) + sum(rest(list));
  }
};

const sumTailRecur = list => {
  const sumHelper = (acc, l) => isEmpty(l) ? acc : sumHelper(acc + first(l), rest(l));
  return sumHelper(0, list);
};

sum(MakeList(3, MakeList(4, MakeList(5, EmptyList)))) // 12
sumTailRecur(MakeList(3, MakeList(4, MakeList(5, EmptyList)))) // 12

const prod = list => {
  if (isEmpty(list)) {
    return 1;
  } else {
    return first(list) * prod(rest(list));
  }
};

const prodTailRecur = list => {
  const prodHelper = (acc, l) => isEmpty(l) ? acc : prodHelper(acc * first(l), rest(l));
  return prodHelper(1, list);
};

prod(MakeList(3, MakeList(4, MakeList(5, EmptyList)))) // 60
prodTailRecur(MakeList(3, MakeList(4, MakeList(5, EmptyList)))) // 60

const min = list => {
  if (!isEmpty(rest(list))) {
    return first(list) < min(rest(list)) ? first(list) : min(rest(list));
  } else {
    return first(list);
  }
}

min(MakeList(8, MakeList(4, MakeList(5, EmptyList)))) 

const max = list => {
  if (!isEmpty(rest(list))) {
    return first(list) > min(rest(list)) ? first(list) : max(rest(list));
  } else {
    return first(list);
  }
}

max(MakeList(8, MakeList(4, MakeList(5, EmptyList)))) 

const map = (list, fn) => {
  if (isEmpty(list)) {
    return EmptyList;
  } else {
    return MakeList(fn.apply(null, [first(list)]), map(rest(list), fn));
  }
}

[...map(MakeList(8, MakeList(4, MakeList(5, EmptyList))), x => x * x)]

const forEach = (list, fn) => {
  while(!isEmpty(list)) {
    fn.apply(null, [list.element]);
    list = list.list;
  }
}

forEach(MakeList(8, MakeList(4, MakeList(5, EmptyList))), x => console.log(x * x))

// TODO: need to revisit this, The output looks okay, but the iterator fails
const reverse = list => {
  const reverseHelper = (l, acc) => {
    if (isEmpty(l)) {
      return acc;
    } else {
      return reverseHelper(rest(list), concat(MakeList(first(list), EmptyList), acc));
    }
  }
  return reverseHelper(list, EmptyList);
}

// TODO: need to revisit this, The output looks okay, but the iterator fails
const take = (list, n) => {
  let current = 0;
  const takeHelper = (l, acc) => {
    if (isEmpty(l)) {
      return acc;
    } else {
      if (current === n) {
        return acc;
      } else {
        current++;
        return takeHelper(rest(l), concat(acc, MakeList(first(l), EmptyList)));
      }
    }
  }
  return (n > 0) ? takeHelper(list, EmptyList) : EmptyList;
}

[...take(MakeList(8, MakeList(4, MakeList(5, EmptyList))), 2)]

const drop = (list, n) => {
  let current = 0;
  const dropHelper = l => {
    if (isEmpty(l)) {
      return EmptyList;
    } else {
      if (current < n) {
        current++;
        return dropHelper(rest(l));
      } else {
        return l;
      }
    }
  }
  return (n == 0) ? list : dropHelper(list);
}

[...drop(MakeList(8, MakeList(4, MakeList(5, EmptyList))), 1)] // [4, 5]

const sublist = (from, count, list) => take(drop(list, from), count);

const l1 = MakeList(3, MakeList(6, MakeList(2, MakeList(8, MakeList(4, MakeList(5, EmptyList))))));
sublist(1, 4, l1) // 6, 2, 8, 4

const slice = (from, to, list) => drop(take(list, to), from)
slice(1, 5, l1) // 6, 2, 8, 4

const takeWhile = (list, fn) => {
  const helper = (l, acc) => {
    if (isEmpty(l)) {
      return acc;
    } else {
      if (!fn.apply(null, [first(l)])) {
        return acc;
      } else {
        return helper(rest(l), MakeList(first(l), acc));
      }
    }
  }
  return helper(list, EmptyList);
}

takeWhile(l1, x => (x > 2)) // 6, 3
```
