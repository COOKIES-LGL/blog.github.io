## 一些常用的 JS 方法

### 1、Copy URL 地址

方式一

```javascript
/**
 * @description 拷贝地址
 * @param {string} url 地址
 **/
export const copyUrl = (url, type = 1) => {
  if (type == 1) {
    navigator.clipboard
      .writeText(url)
      .then(() => {
        console.log("复制成功");
      })
      .catch((err) => {
        console.log("复制失败", err);
      });
    return;
  }
};
```

方式二

```javascript
/**
 * @description 拷贝地址
 * @param {string} url 地址
 **/
export const copyUrl = (url) => {
  let input = document.createElement("input");
  input.value = url;
  document.body.appendChild(input);
  input.select();
  document.execCommand("copy");
  document.body.removeChild(input);
};
```

### 2、处理文件大小展示

```javascript
/**
 * @description 计算文件大小
 * @param {number} fileSize 文件大小
 * @return {string} 计算后的文件大小值
 **/
export const name = (fileSize = 0) => {
  if (!fileSize) return null;
  let size = 0,
    unit = "";
  if (fileSize > 1073741824) {
    size = (fileSize / 1073741824).toFixed(4) * 1;
    unit = "GB";
  } else if (fileSize > 1048576) {
    size = (fileSize / 1048576).toFixed(4) * 1;
    unit = "MB";
  } else if (fileSize > 1024) {
    size = (fileSize / 1024).toFixed(4) * 1;
    unit = "KB";
  } else {
    size = fileSize;
    unit = "B";
  }
  return size + unit;
};
```

### 3、获取音频/视频时长

```javascript
/**
 * @description 获取音/视频时长
 * @param {string/object} file 文件地址/对象
 * @return {number} 媒体文件时长
 **/
export const getMediaDuration = (file) => {
  return new Promise((resolve) => {
    // 是否是文件对象
    let hasFile = file instanceof File;
    let fileUrl = hasFile ? URL.createObjectURL(file) : file;
    // 创建对象
    let audioElement = new Audio(fileUrl);
    // 监听对象
    audioElement.addEventListener("loadedmetadata", () => {
      let time = Math.floor(audioElement.duration) || 0;
      resolve(time);
    });
    // 加载失败
    audioElement.addEventListener("error", () => {
      resolve(0);
    });
  });
};
```

### 4、下载文件

```javascript
/**
 * @description 下载文件
 * @param {string} fileUrl 文件链接地址
 **/
export const downloadFile = (fileUrl) => {
  const iframe = document.createElement("iframe");
  // 防止影响页面布局
  iframe.style.display = "none";
  iframe.style.height = 0;
  iframe.src = fileUrl;
  document.body.appendChild(iframe);
  // 加载完成后移除创建的标签
  iframe.onload = function () {
    document.body.removeChild(iframe);
  };
};
```

### 5、获取地址栏参数

方式一

```javascript
/**
 * @description 获取地址栏参数
 * @param {string} key 参数名
 * @return {string} 参数值
 **/
export const queryParams = (key) => {
  if (!key) return null;
  let url = new URL(location.href);
  let value = url.searchParams.get(key);
  return value;
};
```

方式二

```javascript
/**
 * @description 获取地址栏参数
 * @param {String} key 要取值的参数
 * @return {String} 获取的值
 *  */
export const getQueryString = (name) => {
  let reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
  let r = window.location.search.substring(1).match(reg);
  if (r != null) return decodeURI(r[2]);
  return null;
};
```

### 6、params 参数处理

```javascript
/**
 * @description 参数处理
 * @param {Object} params  参数
 */
export const tansParams = (params) => {
  let result = "";
  for (const propName of Object.keys(params)) {
    const value = params[propName];
    var part = encodeURIComponent(propName) + "=";
    if (value !== null && value !== "" && typeof value !== "undefined") {
      if (typeof value === "object") {
        for (const key of Object.keys(value)) {
          if (value[key] !== null && value[key] !== "" && typeof value[key] !== "undefined") {
            let params = propName + "[" + key + "]";
            var subPart = encodeURIComponent(params) + "=";
            result += subPart + encodeURIComponent(value[key]) + "&";
          }
        }
      } else {
        result += part + encodeURIComponent(value) + "&";
      }
    }
  }
  return result;
};
```

```javascript
new KeyframeEffect(target, keyframes);
new KeyframeEffect(target, keyframes, options);
new KeyframeEffect(source);
// Element.animate;
// https://juejin.cn/post/7023903552946995214
```

### 7、查找当前滚动元素

```javascript
// 查找当前滚动元素
function findScroller(element) {
  element.onscroll = function () {
    console.log(element);
  };
  Array.from(element.children).forEach(findScroller);
}
findScroller(document.body);
```

### 8、window.matchMedia()用法

- js 的媒体查询

```javascript
// 定义媒体查询条件
const mediaQueryString = "(max-width: 600px)";

// 创建媒体查询对象
const mediaQueryList = window.matchMedia(mediaQueryString);

// 检查当前视口是否匹配媒体查询条件
if (mediaQueryList.matches) {
  console.log("视口宽度小于等于600px");
} else {
  console.log("视口宽度大于600px");
}
```

### 9、两日期之间相差天数

```javascript
const dayDiff = (date1, date2) => Math.ceil(Math.abs(date1.getTime() - date2.getTime()) / 86400000);

dayDiff(new Date("2021-10-21"), new Date("2022-02-12"));
// Result: 114
```

### 10、getEnumOptions 从枚举值获取下拉选项列表

```javascript
export const getOptionsFromEnum = (enumParams: object) => {
 if (!enumParams) return []

 const storeSet = new Set()
 const returnList = Object.entries(enumParams).reduce((acc, [key, value]) => {
   if (storeSet.has(value.toString())) {
     acc.push({ label: key, value })
   } else {
     storeSet.add(key)
   }
   return acc
 }, [] as any)
 return returnList
}
```

### 11、使用符号 \*，引入文件中的所有值，默认和具名。

如果我们有以下文件：

```js
// info.js
export const name = "Lydia";
export const age = 21;
export default "I love JavaScript";
// index.js
import * as info from "./info";
console.log(info);
```

将会输出以下内容

```json
{
  "default": "I love JavaScript",
  "name": "Lydia",
  "age": 21
}
```

### 12、千分位格式化金钱

```javascript
const ThousandNum = (num) => num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
const money = ThousandNum(20190214);
// money => "20,190,214"
```

### 13、检测非空参数

```js
function IsRequired() {
  throw new Error("param is required");
}
function Func(name = IsRequired()) {
  console.log("I Love " + name);
}
Func(); // "param is required"
Func("You"); // "I Love You"
```

### 14、调试模式给所有元素添加边框

```js
[].forEach.call($$("*"), (dom) => {
  dom.style.outline = "1px solid #" + (~~(Math.random() * (1 << 24))).toString(16);
});
```

### 模板函数

模板函数是一种特殊的函数调用形式，它允许你定义一个函数来处理模板字符串。这个函数的第一个参数是一个 TemplateStringsArray，它包含了模板字符串中被插值表达式分割成的多个字符串部分。而后续的参数则是这些插值表达式的结果。

```js
// 定义标记模板函数
function tag(strings, ...values) {
  // strings 是一个 TemplateStringsArray
  console.log(strings); // 输出 TemplateStringsArray

  // 遍历 TemplateStringsArray
  strings.forEach((str, index) => {
    console.log(`String part ${index}: ${str}`);
  });

  // values 是一个包含插值表达式结果的数组
  console.log(values); // 输出插值表达式的结果数组
}

// 调用标记模板函数
const name = "Alice";
const place = "Wonderland";
tag`Hello, ${name}! Welcome to ${place}.`;
// 输出
// String part 0: Hello,
// String part 1: ! Welcome to
// String part 2: .
// (2) ['Alice', 'Wonderland']
```

### runWithRetry 失败重试函数

```js
interface Options {
  retries?: number
  interval?: number
  exponentialBackoff?: boolean
}

const DEFAULT_OPTIONS = {
  retries: 3,
  interval: 500,
  exponentialBackoff: true
}

export default function runWithRetry<R>(fn: () => Promise<R>, options?: Options) {
  const innerOptions = {
    ...DEFAULT_OPTIONS,
    ...options
  }

  const { retries, interval, exponentialBackoff } = innerOptions
  return new Promise<R>((resolve, reject) => {
    fn()
      .then(res => {
        resolve(res)
      })
      .catch(error => {
        if (retries === 0) {
          reject(error)
          return
        }

        setTimeout(() => {
          runWithRetry(fn, {
            retries: retries - 1,
            interval: exponentialBackoff ? interval * 2 : interval
          }).then(resolve, reject)
        }, interval)
      })
  })
}

```
