# TIL

# Sat Sep 29 2018

## 1. Today I learned

### Tenary Operator

one *tenary* operator, which takes the form ? : x : y. It's a terse, single-line ***if*** statement that chooses between two expressions, depending on the result of a third one.

- Syntax

```js
condition ? expr1 : expr2

```

Parameters

`condition (or conditions)`

An expression that evaluates to `true` or `false`

`expr1`, `expr2`

Expressions with values of any type		



- ex

```js
<script>
    const a = 10;
	const b = 3;

	const result = a > b ? "hello world" : "javascript";
	console.log(result);//hello world
</script>
```

