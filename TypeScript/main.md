# 学习 TypeScript

1. [教程 1](https://www.tslang.cn/docs/home.html)
2. [教程 2](https://typescript.bootcss.com/)

## 快速开始

```bash
# npm 来源于 node.js 包
npm install -g typescript
```

## 简单的 Hello World

```typescript
function helloWorld(data: String) {
  console.log("hello World:" + data);
}
helloWorld("good");
```

将此文件保存为 `hello.ts`，然后

```bash
tsc hello.ts
```

即可得到目标 `XXX.js` 文件

## 接口 & 类

```typescript
interface People {
  name: String;
  age: Number;
}
function putInfo(people: People) {
  return "姓名:" + people.name + ", 年龄:" + people.age;
}
// student 实现了 People
let user = { name: "John", age: 12 };
document.body.innerHTML = putInfo(user);
/**
 * 学生类
 */
class Student implements People {
  name: String;
  age: Number;
  sex: Boolean;
  constructor(private _sex: Boolean, private people: People) {
    this.sex = _sex;
    this.name = people.name;
    this.age = people.age;
  }
  toString() {
    return {
      name: name,
      age: this.age,
      sex: this.sex,
    };
  }
}
let st = new Student(false, user);
console.log(st);
```

## 数据基本类型

- 布尔值: `boolean`
  - `true`
  - `false`
- 数字 : `number` （和JavaScript一样，TypeScript里的所有数字都是浮点数）
  - 6 (十进制)
  - 0xFFFFFF (十六进制)
  - 0b1010 (二进制)
  - 0o744 (八进制)
- 字符串 : `string`