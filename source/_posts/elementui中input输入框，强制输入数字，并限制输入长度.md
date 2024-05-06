---
title: elementui中input输入框，强制输入数字，并限制输入长度
date: 2021-10-31 03:54:20
tags: elementui
categories: elementui相关
---

##### elementui中input输入框，强制输入数字，并限制输入长度：



```vue
<el-input v-model="scope.row.actRightNums"
          onkeyup="this.value=this.value.replace(/[^\d.]/g,'');"
          maxlength="2">
</el-input>
```

