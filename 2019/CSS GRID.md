# GRID 布局

## 属性
  - grid-template-columns
  - grid-template-rows
  - grid-area
  - order

### grid-template-columns
网格列，可接受参数`固定长度`，`百分比`，`比例值（fr）`，`auto`;
``` css
grid-template-columns: 60px 60px;

grid-template-columns: 1fr 60px;

grid-template-columns: 1fr 2fr;

grid-template-columns: 8ch auto;
```

### grid-template-rows
网格行，可接受参数与网格列相同;
``` css
grid-template-rows: auto;

grid-template-rows: 40px 4em 40px;

grid-template-rows: 1fr 2fr 1fr;

grid-template-rows: 3ch auto minmax(10px, 60px);
```

### grid-area
网格区域，参数接受四个参数，用`/`分割;
``` css
grid-area:  grid-row-start / grid-column-start / grid-row-end / grid-column-end;
```
