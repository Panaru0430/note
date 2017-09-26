# 手札
### Q1：f (!!'0' == false) {console.log(true)}
*****
#### 结果：不会进入`if`代码块。
* 标准相等操作符 `==``!=`
  操作数类型相同：按照全等`===`的规则来比较
  操作数类型不同：会进行类型转换
    - 能转换成布尔的false值：数值`0`、空字符串`''`、`undefined`、`null`、`false`
    - 类型转换
        `Number` > `String`： 先把字符串转换成数字在进行比较
        `Boolean`：含有boolean值，`Boolean`会优先转换成`Number`类型，`true` 转换成num`1`, `false`转换成`0`,再进行比较
        `Object`: 转换成基本类型（①使用valueOf()②使用toString(),日期类的只会使用toString()）
            
            √ null == undefined  规范
            √ 8 == '8' => '8' 转 8  8 == 8  
            √ '1' == true => ① true 转 1  '1' == 1 ② '1' 转 1 1 == 1 
            × '2' == true => ① true 转 1  '2' == 1 ② '2' 转 2 2 == 1 
            √ [] == 0 => ① [].valueOf() [] == 0  ② [].toString() '' == 0 ③ '' 转 0 0 == 0
        
    
* 严格相等操作符`===``!==`
  需满足以下两个条件：- 操作数类型相同 - 操作数值相同
    
        × NaN与任何值都不相等，包括自身
            NaN === NaN ×
        × 操作数为null或undefined，不相等
            var a  => undefined
            var b  => undefined
            a === b ×
            null === undefined × 
        √ 操作数都为true或false，相等
        √ 0 === -0
        √ 操作数指向同一对象、数组或函数，相等
            var a = {}, b, c
            b = a
            c = a
            b === c √
            
        
        