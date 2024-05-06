---
title: elementui表格的表头增加悬浮提示
date: 2023-11-02 11:50:24
tags: element-ui
categories: element-ui相关
---

1、elementui的表格表头由于过长，会导致显示不全。通过悬浮提示以显示全部信息。

```
<el-table :data="tableData" height="320">
<el-table-column min-width="120px" prop="valueA">
    <template #header>
        <el-tooltip :content="content1" placement="top-start" effect="dark">
            <div>
                {{ Name1 }}
                <br/>
                {{ Name2 }}
            </div>
        </el-tooltip>
    </template>
</el-table-column>
</el-table>
```

https://element.eleme.cn/#/zh-CN/component/tooltip
