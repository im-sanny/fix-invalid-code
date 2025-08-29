# 🧱 Class 14: What is Scope

Scope বলতে code-এর সেই অংশকে বুঝায়, যেখানে একটি নির্দিষ্ট variable কে access করা যাবে।

## 📘 ক্লাসে ব্যবহৃত কোড

```go
package main

import "fmt"

var (
	a = 20
	b = 30
)

func add(x int, y int) {
	z := x + y
	fmt.Println(z)
}

func main() {
	p := 30
	q := 40

	add(p, q)

	add(a, b)

	add(a, p)

	add(b, z) // ❌ error!
}

```

## ✨ Code Explain

```go
var (
	a = 20
	b = 30
)
```

🔸 RAM-এ একটা জায়গায় এ variable গুলো রাখা হয় যেটাকে **global memory** বলা হয়।

🔸 `a` এবং `b` এই দুইটা RAM-এর global scope এ declare করা হয়েছে।

🔸 যেকোন ফাংশনের ভেতর থেকে এগুলোকে ব্যবহার করা যাবে।

> 🧠 যেসব ভ্যারিয়েবল `main()` বা অন্য কোনো ফাংশনের বাইরে declare করা হয় - সেগুলো global.

👉 `main()` এবং `add()` function ও RAM এর global scope এ থাকে (বুঝার সুবিধার্থে)।

### 💡 শুরুতে RAM এ যা থাকে

```plaintext
				    RAM
+------------------------------------------+
| 20 | 30 | ---- |---- |   |   |   |   |   |
+------------------------------------------+
   a    b   add()  main()

				 global
```

> Go প্রোগ্রামে `main()` ফাংশন না থাকলে, প্রোগ্রাম চলবে না।
> `main()` হচ্ছে execution শুরু করার জায়গা; ওটা ছাড়া Go বুঝতে পারে না কোথা থেকে শুরু করবে।

### `main()` Execution

```go

func main() {
	p := 30
	q := 40

	add(p, q)

	add(a, b)

	add(a, p)

	add(b, z)
}
```

🔸RAM-এ `main()` এর জন্য আলাদা জায়গা দখল করে।

🔸p এবং q হলো `main()` এর local variable।

🔸এগুলো `main()` এর বাইরে থেকে ব্যবহার করা যাবে না।

```plaintext
				    RAM
+------------------------------------------+
| 30 | 40 |  |  |  |  |  |  |  |  |  |  |  |
+------------------------------------------+
  p    q

  				main()
```

- `main()` এ `add()` function না থাকায় `add()` কে global এ খুঁজে।

### `add()` Execution

```go
func add(x int, y int) {
	z := x + y
	fmt.Println(z)
}
```

🔸 RAM-এ `add()` এর জন্য আলাদা জায়গা নেয়।

🔸 `x`, `y` & `z` হল `add()` এর local variable.

🔸 `x` এবং `y` হল `add()` function এর parameter.

🔸`add()` function শেষ হলে RAM থেকে এদের মুছে ফেলা হয়।

```plaintext
				    RAM
+------------------------------------------+
| 30 | 40 | 70 |  |  |  |  |  |  |  |  |  |
+------------------------------------------+
  x    y    z

  				add()
```

### ❌ যদি স্কোপ ভুলে যাও

```go
func add(x int, y int) {
	z := x + q
	fmt.Println(z)
}
```

🔸`q` variable `add()` এর local scope-এ নেই।

🔸`q` -> `main()` এর local variable হওয়ায় global scope এ পাওয়া যাবে না।

> Scope এর বাইরের variable use করলে `undefined` compilation error দিবে।

**Output:**

```go
# command-line-arguments
./main.go:11:11: undefined: q
```

## 🧠 Scope Rule

| কোথায় declare হয়েছে    | কোথা থেকে accessible                   |
| ---------------------- | -------------------------------------- |
| ফাংশনের বাইরে (global) | সব ফাংশন থেকে access করা যায়। ✅       |
| ফাংশনের ভিতরে (local)  | শুধু সেই ফাংশনের ভিতরেই accessible। ✅ |
| অন্য ফাংশনের ভিতর      | বাইরে থেকে access করা যায় না। ❌       |

[**Author :** @nazma98
**Date:** 2025-06-13
**Category:** interview-qa/class-wise ]
