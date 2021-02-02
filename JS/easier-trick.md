# 一些简单好用的JS语法
可以当做手册，也可以帮助初学者理解JS的逻辑。

原文：[JavaScript Functions That Will Make Your Life Much Easier.](https://dev.to/youssefzidan/javascript-functions-that-will-make-your-life-much-easier-1imh)

## randomNumber();
获取最大值和最小值之间的随机数

```
/**
 * Generating random integers between min and max.
 * @param {number} min Min number
 * @param {number} max Max Number
 */
export const randomNumber = (min = 0, max = 1000) =>
  Math.ceil(min + Math.random() * (max - min));

// Example
console.log(randomNumber()); // 97
```

## capitalize();
字符串首字母大写
```
/**
 * Capitalize Strings.
 * @param {string} s String that will be Capitalized
 */
export const capitalize = (s) => {
  if (typeof s !== "string") return "";
  return s.charAt(0).toUpperCase() + s.slice(1);
};

// Example
console.log(capitalize("cat")); // Cat
```

## truncate();
调整字符串长度
```
/**
 * Truncating a string...
 * @param {string} text String to be truncated
 * @param {number} num Max length of the `String` that will be truncated after
 */
export const truncate = (text, num = 10) => {
  if (text.length > num) {
    return `${text.substring(0, num - 3)}...`;
  }
  return text;
};

// Example
console.log(truncate("this is some long string to be truncated"));  

// this is...
```

## toTop();
返回顶部
```
/**
 * Scroll to top
 */
export const toTop = () => {
  window.scroll({ top: 0, left: 0, behavior: "smooth" });
};
```

## deepClone();
输入嵌套对象
```
/**
 * Deep cloning inputs
 * @param {any} input Input
 */
export const deepClone = (input) => JSON.parse(JSON.stringify(input));
```

## getURLParams();
从URL快速获取参数
```
/**
 * Get param name from URL.
 * @param {string} name
 */
export const getURLParams = (name) => new URLSearchParams(window.location.search).get(name);

// Example
console.log(getURLParams(id)) // 5
```

## getInnerHTML();
获取HTML
```
/**
 * Getting the inner `Text` of an `HTML` string
 * @param {string} str A string of HTML
 */
export const getInnerHTML = (str) => str.replace(/(<([^>]+)>)/gi, "");
```

## toggleStrNum();
返回“0”或“1”
```
/**
 *  returning "1" from "0" and the opposit.
 * @param {string} strNum String Number ex: "0", "1"
 */
export const toggleStrNum = (strNum) => (strNum === "0" ? "1" : "0");
```

## scrollToHide();
下滑时隐藏HTML元素
```
/**
 * Hide HTML element when scrolling down.
 * @param {string} id the `id` of an `HTML` element
 * @param {string} distance in px ex: "100px"
 */
export const scrollToHide = (id, distance) => {
  let prevScrollpos = window.pageYOffset;
  window.onscroll = () => {
    const currentScrollPos = window.pageYOffset;
    if (prevScrollpos > currentScrollPos) {
      document.getElementById(id).style.transform = `translateY(${distance})`;
    } else {
      document.getElementById(id).style.transform = `translateY(-${distance})`;
    }
    prevScrollpos = currentScrollPos;
  };
};
```

## runEvery();
指定时间运行脚本
```
/**
 * Call a function every specific time.
 */
export const runEvery = (
  func = () => console.log("Fired!"),
  hour = 12,
  mins = 0,
  interval = 300000,
) => {
  window.setInterval(() => {
    const now = new Date();
    if (now.getHours() >= hour && now.getMinutes() >= mins) {
      func();
    }
  }, interval);
};
```

## humanFileSize ();
将`字节`转化为常见的文件大小单位
```
/**
 * Converting Bytes to Readable Human File Sizes.
 * @param {number} bytes Bytes in Number
 */
export const humanFileSize = (bytes) => {
  let BYTES = bytes;
  const thresh = 1000;

  if (Math.abs(BYTES) < thresh) {
    return `${BYTES} B`;
  }

  const units = ["kB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB"];

  let u = -1;
  const r = 10 ** 1;

  do {
    BYTES /= thresh;
    u += 1;
  } while (Math.round(Math.abs(BYTES) * r) / r >= thresh && u < units.length - 1);

  return `${BYTES.toFixed(1)} ${units[u]}`;
};

// Example
console.log(humanFileSize(456465465)); // 456.5 MB
```

## getTimes();
每n分钟输出一次当前时间
```
/**
 * Getting an Array of Times + "AM" or "PM".
 * @param {number} minutesInterval
 * @param {number} startTime 
 */
export const getTimes = (minutesInterval = 15, startTime = 60) => {
  const times = []; // time array
  const x = minutesInterval; // minutes interval
  let tt = startTime; // start time
  const ap = ["AM", "PM"]; // AM-PM

  // loop to increment the time and push results in array
  for (let i = 0; tt < 24 * 60; i += 1) {
    const hh = Math.floor(tt / 60); // getting hours of day in 0-24 format
    const mm = tt % 60; // getting minutes of the hour in 0-55 format
    times[i] = `${`${hh === 12 ? 12 : hh % 12}`.slice(-2)}:${`0${mm}`.slice(-2)} ${
      ap[Math.floor(hh / 12)]
    }`; // pushing data in array in [00:00 - 12:00 AM/PM format]
    tt += x;
  }
  return times;
};

// Example
console.log(getTimes());
/* [
    "1:00 AM",
    "1:15 AM",
    "1:30 AM",
    "1:45 AM",
    "2:00 AM",
    "2:15 AM",
    "2:30 AM",
    // ....
    ]
*/
```

## setLocalItem(); & getLocalItem();
缓存将过期数据于`LocalStorage`中
```
/**
 * Caching values with expiry date to the LocalHost.
 * @param {string} key Local Storage Key
 * @param {any} value Local Storage Value
 * @param {number} ttl Time to live (Expiry Date in MS)
 */
export const setLocalItem = (key, value, ttl = duration.month) => {
  const now = new Date();
  // `item` is an object which contains the original value
  // as well as the time when it's supposed to expire
  const item = {
    value,
    expiry: now.getTime() + ttl,
  };
  localStorage.setItem(key, JSON.stringify(item));
};


/**
 * Getting values with expiry date from LocalHost that stored with `setLocalItem`.
 * @param {string} key Local Storage Key
 */
export const getLocalItem = (key) => {
  const itemStr = localStorage.getItem(key);
  // if the item doesn't exist, return null
  if (!itemStr) {
    return null;
  }
  const item = JSON.parse(itemStr);
  const now = new Date();
  // compare the expiry time of the item with the current time
  if (now.getTime() > item.expiry) {
    // If the item is expired, delete the item from storage
    // and return null
    localStorage.removeItem(key);
    return null;
  }
  return item.value;
};
```

## logFormattedStrings();
输入可读数据至控制台
```
/**
 * Logging formatted strings
 * @param {any} input
 */
export const logFormattedStrings = (input) => console.log(JSON.stringify(input, null, 4));

// Example 
logFormattedStrings({ fName: "Youssef", lName: "Zidan" });
/*
 {
   "fName": "Youssef",
   "lName": "Zidan"
 } 
*/
```
