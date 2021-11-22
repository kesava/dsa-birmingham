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
```
The output would be as follows
```
{ spreadableList: [ 10, 8, 5, 3 ] }
{ i: 10 }
{ i: 8 }
{ i: 5 }
{ i: 3 }
10
{
  element: 8,
  list: {
    element: 5,
    list: {
      element: 3,
      list: null,
      [Symbol(Symbol.iterator)]: ƒ [Symbol.iterator]()
    },
    [Symbol(Symbol.iterator)]: ƒ [Symbol.iterator]()
  },
  [Symbol(Symbol.iterator)]: ƒ [Symbol.iterator]()
}
true
```
