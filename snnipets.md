## 异步执行器
```js
export default function runGenerator(gen) {
  let it = gen, ret;

  function next(preData) {
    ret = it.next(preData)

    if (!ret.done) {
      if (ret.value.constructor.name === 'Promise') {
        ret.value.then(next, (err) => {
          it.throw(err)
        });
      } else {
        try {
          next(ret.value);
        } catch (err) {
          it.throw(err)
        }
      }
    }
  }
  next();
}

```


