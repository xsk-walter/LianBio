###### 每日一练

###### 一、数组：存在重复元素

```javascript
// 1、通过Set去重后长度是否变化
Array.from(new Set(nums).length != nums.length)
// 2、通过哈希表；对于数组中的每个元素，我们将它插入到哈希表中。如果插入一个元素时发现已经存在，则说明有重复
let set = new Set()
for (const k of nums) {
    if (set.has(k)) {
        return true
    }
    set.add(k)
}
return false
```

###### 二、数组：只出现一次数字，其他都是两次(非空正整数)

```javascript
// 1、mySelf: 利用findIndex方法，若出现一次，则从前后查找的该元素下标之和等于数组的长度
let str = 0
let len = nums.length - 1
let cache = JSON.parse(JSON.stringify(nums))
let reves = nums.reverse()
for (const k of nums) {
    let index1 = cache.findIndex(item => item === k)
    let index2 = reves.findIndex((item) => item === k)
    if ((index1 + index2) === len) {
        return k
    }
}
// 2、利用异或运算符， ^=; 只对于正整数，两个相同的整数 异或 运算后为 0；
let result = 0
for(let k of nums) {
    result ^= k
}
return result
```

###### 三、多数元素 - 元素次数>n/2次数

```javascript
// myself: 通过obj统计元素出现次数，然后遍历判断次数大于n/2的元素
var majorityElement = function (nums) {
  let obj = {}
  let str = ''
  let len = nums.length / 2
  for (let k of nums) {
    if (!obj[k]) {
      obj[k] = 0
    }
    obj[k]++
  }
  for (let key in obj) {
    if (obj[key] > len) {
      str = key
    }
  }
  return str
}
// 3.1 投票法：定义首个元素为众数，遍历数组，元素相同则count++，否则count--，若count的值为0，则将下位元素设置为众数，结束后则众数就是大于n/2次数
var majorityElement1 = function (nums) {
    let cache = nums[0]
    let count = 0

    nums.forEach((item, index) => {
        !count && (cache = (index > (nums.length - 2) || index == 0) ?  item : nums[index + 1])
        cache === item ? count++ : count--
    })

    return cache
}

// 优化后：
var majorityElement2 = function(nums) {
    let cache = nums[0], count = 1
    // 利用for循环避免首位需要单独判断问题
    for (let i = 1; i < nums.length; i++) {
        count === 0 && (cache = nums[i])
        cache === nums[i] ? count++ : count--
    }
    return cache
}
```

###### 四、三数之和

```javascript
// 4.1 myself bug-leetcode超出时间限制
var threeSum = function (nums) {
  let arr = []
  let newArr = []
  let result = []

  // 将所有的组合取出
  nums.forEach((item1, index1) => {
    nums.forEach((item2, index2) => {
      nums.forEach((item3, index3) => {
        if (index1 != index2 && index2 != index3 && index1 != index3)
          arr.push([item1, item2, item3])
      })
    })
  })

  // 元素排序
  arr.forEach((item) => {
    item = item.sort((a, b) => a - b)
  })

  // 去重 - 需要将数组转换成JSON字符串去重
  arr.forEach((item) => {
    if (!newArr.includes(JSON.stringify(item))) {
      newArr.push(JSON.stringify(item))
    }
  })

  // 取出符合要求的数组
  newArr.forEach((item) => {
    let val = JSON.parse(item) // 将JSON字符串解析成数组
    //  求和
    let res = val.reduce((pre, cur) => {
      return pre + cur
    })
    if (res === 0) {
      result.push(val)
    }
  })

  return result
}

// 3.2 排序 + 双指针： 大佬的模板
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum1 = function (nums) {
  if (nums.length < 3) {
    return []
  }
  // 从小到大排序
  const arr = nums.sort((a, b) => a - b)
  // 最小值大于 0 或者 最大值小于 0，说明没有无效答案
  if (arr[0] > 0 || arr[arr.length - 1] < 0) {
    return []
  }

  const n = arr.length
  const res = []
  for (let i = 0; i < n; i++) {
    // 如果当前值大于 0，和右侧的值再怎么加也不会等于 0，所以直接退出
    if (nums[i] > 0) {
      return res
    }
    // 当前循环的值和上次循环的一样，就跳过，避免重复值
    if (i > 0 && arr[i] === arr[i - 1]) {
      continue
    }
    // 双指针
    let l = i + 1
    let r = n - 1
    while (l < r) {
      const temp = arr[i] + arr[l] + arr[r]
      if (temp > 0) {
        r--
      }
      if (temp < 0) {
        l++
      }
      if (temp === 0) {
        res.push([nums[i], nums[l], nums[r]])
        // 跳过重复值
        while (l < r && nums[l] === nums[l + 1]) {
          l++
        }
        // 同上
        while (l < r && nums[r] === nums[r - 1]) {
          r--
        }
        l++
        r--
      }
    }
  }
  return res
}
```



###### 技术点

1、for of(ES6新增循环方法) 和 for in 的区别和作用

* for of 对数组的遍历；无法循环遍历对象
* 遍历输出结果不同：
  * for ..in..循环遍历的是数组的索引；for...of...循环遍历的是数组的值
  * for...in会遍历自定义属性（arr.name = '数组'）

2、forEach如何完全退出循环

3、js的运算符：按位异或 ^

* 异或：表示当两个数的二进制表示，进行异或运算时，当前位的两个二进制表示不同则为1,相同则为0,，该方法广泛推广用来统计一个数的1的位数
* 位运算：表示运算时把数字用二进制表示，

4、投票法：选出元素出现次数最多的元素

5、数组去重十二种

6、双指针