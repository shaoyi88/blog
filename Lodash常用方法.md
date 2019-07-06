## Array常用方法

- compact —— 去除数组假值(false, null, 0, "", undefined,NaN)
- uniq —— 去除数组重复值
```
_.compact([0, 1, false, 2, '', 3]); // => [1, 2, 3]

_.uniq([2, 1, 2]); // => [2, 1]
```

- difference —— 求数组差集
- intersection —— 求数组交集
- union —— 求数组并集
```
_.difference([1, 2], [2, 3]); // => [1]

_.intersection([1, 2], [2, 3]); // => [2]

_.union([1, 2], [2, 3]); // => [1, 2, 3]
```

- reverse —— 数组反转
```
_.reverse([1, 2, 3]); // => [3, 2, 1]
```

## Object常用方法
- assign —— 分配来源对象的可枚举属性到目标对象上，相同属性会覆盖
- defaults —— 分配来源对象的可枚举属性到目标对象上，相同属性不会覆盖
```
_.assign({ 'a': 0 }, { 'a': 1, 'b': 2}); // => { 'a': 1, 'b': 2 }

_.defaults({ 'a': 0 }, { 'a': 1, 'b': 2}); // => { 'a': 0, 'b': 2 }
```

- get —— 根据object对象的path路径获取值
```
var object = { 'a': [{ 'b': { 'c': 3 } }] };
_.get(object, 'a[0].b.c'); // => 3
_.get(object, 'a.b.c', 'default'); // => 'default'
```

- pick —— 获取object对象中指定的属性
```
var object = { 'a': 1, 'b': '2', 'c': 3 };
_.pick(object, ['a', 'c']); // => { 'a': 1, 'c': 3 }
```

## String常用方法
- escape —— 转义string中的 "&", "<", ">", '"', "'", 和 "`" 字符为HTML实体字符
- unescape —— escape反向版
- escapeRegExp —— 转义 RegExp 字符串中特殊的字符 "^", "$", "", ".", "*", "+", "?", "(", ")", "[", "]", "{", "}", 和 "|" in .
```
_.escape('fred, barney, & pebbles');
// => 'fred, barney, &amp; pebbles'

_.unescape('fred, barney, &amp; pebbles');
// => 'fred, barney, & pebbles'

_.escapeRegExp('[lodash](https://lodash.com/)');
// => '\[lodash\]\(https://lodash\.com/\)'
```

- truncate —— 截断string字符串，如果字符串超出了限定的最大值。 被截断的字符串后面会以 omission 代替，omission 默认是 "..."
```
_.truncate('hididdlyho', {
  'length': 5,
  'omission': '...'
}); // hi...
```