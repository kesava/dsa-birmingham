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
```