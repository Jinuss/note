---
categories:
- React
- Javascript
- antd
tags:
- React
- Javascript
- antd
---
表格合并的本质无非是行合并（`rowSpan`）或者列合并(`cellSpan`),以下示例为表格根据表格内容实现行合并
- 动态赋值：表格数据添加`rowSpan`
```js
/*
 * data:表格内容
 * dataIndex：对应单元格的索引值
*/
export const mergeTableCell = (data, dataIndex) => {
    return data.reduce((result, item) => {
        if (result.indexOf(item[dataIndex]) < 0) {
            result.push(item[dataIndex])
        }
        return result
    }, []).reduce((result, name) => {
        const children = data.filter(item => item[dataIndex] === name);
        result = result.concat(
            children.map((item, index) => ({
                ...item,
                rowSpan: index === 0 ? children.length : 0,
            }))
        )
        return result;
    }, [])
}
```
- 设置column属性

```js
 render(txt, row, index) => {
        return {
          children: row.wrpNm,
          props: {
            rowSpan: row.rowSpan,
          }
        }
      }
```