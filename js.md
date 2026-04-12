# Module 4: Performance & Optimization — Complete Exam Guide

---

## Debouncing & Throttling

### Differences & Use Cases

| Feature | Debounce | Throttle |
|---------|----------|----------|
| **Executes** | After user **stops** for `delay` ms | At most once per `interval` ms |
| **Resets timer?** | ✅ On every call | ❌ No |
| **First call** | Delayed | Usually immediate |
| **Guaranteed rate** | No | Yes (every interval) |
| **Best for** | Search input, form validation, resize end | Scroll, analytics, API rate limit |

```
Debounce:  ──call──call──call──call── ...300ms silence... ──exec
           All earlier calls cancelled. Only the last one fires.

Throttle:  ──call──call──call──call──call──call──
              ↓                       ↓
             exec                    exec       (one per interval)
```

### Implementation from Scratch

#### Debounce
```js
function debounce(fn, delay) {
  let timerId = null;

  return function (...args) {
    clearTimeout(timerId);              // cancel previous timer
    timerId = setTimeout(() => {
      fn.apply(this, args);             // execute after silence
    }, delay);
  };
}

// Usage:
const search = debounce((query) => {
  console.log("Searching:", query);
}, 300);

search("h");     // cancelled
search("he");    // cancelled
search("hello"); // ✅ executes after 300ms of silence
```

#### Throttle
```js
function throttle(fn, interval) {
  let lastTime = 0;

  return function (...args) {
    const now = Date.now();
    if (now - lastTime >= interval) {
      lastTime = now;
      fn.apply(this, args);             // execute at most once per interval
    }
  };
}

// Usage:
const onScroll = throttle(() => {
  console.log("Scroll position:", window.scrollY);
}, 200);  // fires at most every 200ms during scroll
```

#### Throttle with Trailing Call
```js
function throttleWithTrailing(fn, interval) {
  let lastTime = 0;
  let timer = null;

  return function (...args) {
    const now = Date.now();
    const remaining = interval - (now - lastTime);

    if (remaining <= 0) {
      clearTimeout(timer);
      lastTime = now;
      fn.apply(this, args);
    } else if (!timer) {
      timer = setTimeout(() => {
        lastTime = Date.now();
        timer = null;
        fn.apply(this, args);  // trailing: fires the last queued call
      }, remaining);
    }
  };
}
```

### Real-World Scenarios

```js
// Search-as-you-type → DEBOUNCE
input.addEventListener("input", debounce(fetchResults, 300));

// Scroll tracking → THROTTLE
window.addEventListener("scroll", throttle(updateProgress, 100));

// Window resize → DEBOUNCE (fire after user stops resizing)
window.addEventListener("resize", debounce(recalculateLayout, 250));

// Button click → THROTTLE (prevent double-submit)
button.addEventListener("click", throttle(submitForm, 2000));
```

---

## Memory Management & Garbage Collection

### Stack vs Heap

| Stack | Heap |
|-------|------|
| Stores **primitives** and **references** | Stores **objects**, arrays, functions |
| Fixed size, fast access | Dynamic size, slower access |
| Automatically cleaned (function scope) | Cleaned by garbage collector |
| LIFO (last-in, first-out) | No particular order |

```js
let a = 10;                // primitive → stored on STACK
let b = a;                 // copies the VALUE (independent)
b = 20;                    // a is still 10

let obj1 = { x: 1 };      // object → stored on HEAP, reference on STACK
let obj2 = obj1;           // copies the REFERENCE (both point to same object)
obj2.x = 99;               // obj1.x is also 99 (same object!)
```

### Garbage Collection Basics

JS uses **mark-and-sweep**: the GC starts from "roots" (global, stack), marks everything reachable, and sweeps (frees) everything unmarked.

```js
function example() {
  const data = { large: new Array(1000000) };
  // `data` is reachable during function execution

  return data.large.length;
}
// After example() returns, `data` is unreachable → GC can free it
```

### Memory Leaks (Closures, DOM References, Timers)

#### 1. Closure Leaks
```js
function createLeak() {
  const hugeArray = new Array(1000000).fill("data");

  return function() {
    // This closure holds a reference to hugeArray
    // Even if we never use hugeArray, it can't be GC'd!
    console.log("I'm leaking memory");
  };
}
const leak = createLeak();  // hugeArray stays in memory forever
```

#### 2. Forgotten Timers
```js
// ❌ LEAK: interval keeps running and holding references
const data = loadData();
setInterval(() => {
  processData(data);         // `data` can never be GC'd
}, 1000);

// ✅ FIX: clear when done
const id = setInterval(() => { ... }, 1000);
clearInterval(id);           // allow GC
```

#### 3. Detached DOM References
```js
// ❌ LEAK: DOM element removed but JS still holds reference
const btn = document.getElementById("myBtn");
document.body.removeChild(btn);
// `btn` variable still references the detached DOM node → can't be GC'd

// ✅ FIX:
btn = null;                  // release the reference
```

#### 4. Event Listeners
```js
// ❌ LEAK: listeners hold references to closures/DOM
element.addEventListener("click", handler);
// Even after element is removed, handler closure persists

// ✅ FIX:
element.removeEventListener("click", handler);
```

### Optimization Strategies
- Set unused variables to `null`
- Clear timers/intervals when not needed
- Use `WeakMap`/`WeakSet` for caches (allows GC of keys)
- Remove event listeners when components unmount
- Avoid global variables

---

## Cloning, Equality & Immutability

### Shallow vs Deep Copy

```js
const original = { a: 1, nested: { b: 2 } };

// SHALLOW COPY — copies top-level, shares nested references
const shallow = { ...original };
shallow.a = 99;             // original.a is still 1 ✅
shallow.nested.b = 99;      // original.nested.b is ALSO 99 ❌ (shared reference!)

// DEEP COPY — fully independent clone
const deep = JSON.parse(JSON.stringify(original));
deep.nested.b = 99;         // original.nested.b is still 2 ✅
```

### Object.assign, Spread Operator

Both create **shallow** copies:

```js
// Object.assign
const copy1 = Object.assign({}, original);

// Spread operator
const copy2 = { ...original };

// For arrays:
const arrCopy = [...originalArray];
const arrCopy2 = originalArray.slice();
```

### Deep Cloning Techniques

```js
// Method 1: JSON (simple but limited)
const deep1 = JSON.parse(JSON.stringify(obj));
// ❌ Fails with: functions, undefined, Date, Map, Set, circular refs, Symbol

// Method 2: structuredClone (modern, recommended)
const deep2 = structuredClone(obj);
// ✅ Handles: Date, Map, Set, ArrayBuffer, circular refs
// ❌ Fails with: functions, DOM nodes, Symbol

// Method 3: Recursive (interview favorite)
function deepClone(value, seen = new Map()) {
  if (value === null || typeof value !== "object") return value;
  if (seen.has(value)) return seen.get(value);     // circular ref!

  const clone = Array.isArray(value) ? [] : {};
  seen.set(value, clone);

  for (const key in value) {
    if (value.hasOwnProperty(key)) {
      clone[key] = deepClone(value[key], seen);
    }
  }
  return clone;
}
```

### Value vs Reference Comparison

```js
// PRIMITIVES — compared by VALUE
5 === 5;              // true
"hi" === "hi";        // true

// OBJECTS — compared by REFERENCE (memory address)
{} === {};            // false (two different objects)
[] === [];            // false
const a = { x: 1 };
const b = a;
a === b;              // true (same reference)

// Deep equality check:
function isEqual(a, b) {
  if (a === b) return true;
  if (typeof a !== "object" || typeof b !== "object") return false;
  if (a === null || b === null) return false;
  const keysA = Object.keys(a), keysB = Object.keys(b);
  if (keysA.length !== keysB.length) return false;
  return keysA.every(key => isEqual(a[key], b[key]));
}
```

### Immutable Patterns

```js
// Object.freeze — shallow immutability
const config = Object.freeze({ host: "localhost", port: 3000 });
config.host = "other";     // silently fails (or throws in strict mode)

// Spread for immutable updates
const state = { count: 1, name: "A" };
const newState = { ...state, count: state.count + 1 }; // new object, count = 2

// Array immutable operations
const arr = [1, 2, 3];
const added = [...arr, 4];           // [1, 2, 3, 4] — new array
const removed = arr.filter(x => x !== 2); // [1, 3] — new array
const updated = arr.map(x => x === 2 ? 99 : x); // [1, 99, 3] — new array
```

---

## Memoization & Caching

### Concept & Benefits

Memoization stores the results of function calls so repeated calls with the same arguments return instantly:

```
First call:   add(5) → computes 5+5 = 10, stores cache[5] = 10
Second call:  add(5) → finds cache[5], returns 10 immediately (no computation)
```

### Implementing Memoization

```js
function memoize(fn) {
  const cache = new Map();

  return function (...args) {
    const key = JSON.stringify(args);    // convert args to cache key
    if (cache.has(key)) return cache.get(key);

    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Usage:
const expensiveFn = memoize((n) => {
  console.log("Computing...");
  return n * n;
});

expensiveFn(5);  // "Computing..." → 25
expensiveFn(5);  // 25 (no log — cached!)
```

### Cache Invalidation Basics

```js
// Time-based invalidation:
function memoizeWithTTL(fn, ttlMs) {
  const cache = new Map();
  return function (...args) {
    const key = JSON.stringify(args);
    const entry = cache.get(key);
    if (entry && Date.now() - entry.time < ttlMs) return entry.value;

    const value = fn.apply(this, args);
    cache.set(key, { value, time: Date.now() });
    return value;
  };
}

// Size-based (LRU) — evict oldest when cache is full
// WeakMap-based — let GC handle cleanup when keys are dereferenced
```

---

## JS Weird Parts (Advanced Edge Cases)

### Type Coercion Quirks

```js
// == triggers coercion, === does not
"5" == 5;            // true  (string coerced to number)
"5" === 5;           // false (different types)

// Weird coercions:
[] == 0;             // true  ([] → "" → 0)
[] == false;         // true  ([] → "" → 0, false → 0)
"" == false;         // true  (both → 0)
null == undefined;   // true  (special rule)
null === undefined;  // false

// + operator — concatenation vs addition
"5" + 3;             // "53"  (string wins)
5 + "3";             // "53"  (string wins)
5 - "3";             // 2     (- only works with numbers)
true + true;         // 2     (true → 1)
[] + [];             // ""    (both → "")
[] + {};             // "[object Object]"
{} + [];             // 0     ({} is parsed as empty block, +[] → 0)
```

### Hoisting Behavior

```js
// var is hoisted (declaration, not value)
console.log(a);      // undefined (not ReferenceError)
var a = 5;

// let/const are hoisted but in "Temporal Dead Zone" (TDZ)
console.log(b);      // ❌ ReferenceError: Cannot access 'b' before initialization
let b = 5;

// Function declarations are fully hoisted
greet();             // ✅ works!
function greet() { console.log("Hi"); }

// Function expressions are NOT fully hoisted
sayHi();             // ❌ TypeError: sayHi is not a function
var sayHi = function() { console.log("Hi"); };
```

### `==` vs `===`

```js
// === (strict equality) — NO coercion, must match type AND value
1 === 1;       // true
1 === "1";     // false (different types)

// == (loose equality) — coerces types, then compares
1 == "1";      // true ("1" → 1)

// RULE: Always use === unless you specifically need coercion
// The only useful == case: value == null (checks null AND undefined)
if (value == null) { ... }  // same as: value === null || value === undefined
```

### NaN, undefined, null Differences

```js
// undefined — variable declared but not assigned
let x;
console.log(x);           // undefined
typeof undefined;          // "undefined"

// null — intentional absence of value
let y = null;
typeof null;               // "object" (famous JS bug, never fixed)

// NaN — "Not a Number" (but typeof is "number" 🤯)
typeof NaN;                // "number"
NaN === NaN;               // false (NaN is not equal to ANYTHING, including itself)
Number.isNaN(NaN);         // true  ← only reliable check
isNaN("hello");            // true  ← coerces first, unreliable
Number.isNaN("hello");     // false ← no coercion, reliable

// Comparisons:
null == undefined;         // true (special rule)
null === undefined;        // false
null == 0;                 // false
undefined == 0;            // false
NaN == NaN;                // false
```

### Weird Interview Edge Cases

```js
// typeof quirks
typeof [];                // "object" (arrays are objects)
typeof null;              // "object" (legacy bug)
typeof function(){};      // "function"
typeof NaN;               // "number"
typeof undefined;         // "undefined"
typeof "hello";           // "string"

// Truthy/Falsy
// Falsy values (only 8): false, 0, -0, 0n, "", null, undefined, NaN
// EVERYTHING else is truthy, including:
Boolean([]);              // true  (empty array is truthy!)
Boolean({});              // true  (empty object is truthy!)
Boolean("0");             // true  (non-empty string)
Boolean("false");         // true  (non-empty string)

// parseInt quirks
parseInt("10abc");        // 10 (parses what it can)
parseInt("abc10");        // NaN (can't start with non-digit)
parseInt(0.000001);       // 0
parseInt(0.0000001);      // 1 (!) — 0.0000001 → "1e-7" → parseInt("1e-7") → 1

// Floating point
0.1 + 0.2 === 0.3;       // false (0.30000000000000004)
// Fix: Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON
```

---

## Quick Reference

```
Debounce:  wait for silence → execute once
Throttle:  execute at most once per interval

Shallow copy: { ...obj }, Object.assign
Deep copy:    structuredClone(), recursive clone, JSON parse/stringify

Memoize:  cache results by arguments, return cached on repeat calls

Falsy:   false, 0, -0, 0n, "", null, undefined, NaN (8 total)
Truthy:  everything else (including [], {}, "0", "false")

typeof null  →  "object"   (bug)
typeof NaN   →  "number"   (weird)
NaN === NaN  →  false       (unique)
```

---

*End of Module 4*
