### AppInfo爬虫脚本

1. 爬虫框架（2days）：编写爬虫Demo。

2. 根据App包爬取App信息。

   - App包 { [AppPackage：包URL]，[AppName：App名称] }

   - App信息 { [Source：App信息来源]，[AppPackage：包URL]，[AppNameGrab：App名称爬虫]，[AppName：App名称]，[Category：App分类]，[AppCompany：App公司]，[ICON：App图标] } 

3. 爬取顺序：① 爬取豌豆荚 (../package-name)，② 爬取应用宝 (../appName=package-name)，③ 如果没有则插入null。

4. 写脚本（供运营使用）：爬取时启动脚本，待全部数据爬取完毕后关闭脚本。

> 爬虫框架选型，爬虫编写，脚本编写。

### prodmp 标签DMP平台

完成了：

- 使用 Angular Material 为趋势图添加日期范围组件。
- 使用 Apache POI 实现趋势数据导出为 Excel 表的功能。

学习了：

- 学习了 Angular Material UI框架
- 学习了 Apache POI 生成 Excel 文件。
- 学习了 Angular 文件下载。

> √：Angular Material，Apache POI，文件下载。


### admin - 企业管理

【20180925-20181113】

#### 总结

了解但未深入的知识点：

- Druid 数据源。
- SpringBoot AOP 实现日志功能。
- Form 表单的参数校验。
- Accordion 的 Lazy Load。
- DateBox 日期限制功能。

知识盲区：

- 微服务。
- Redis 缓存，RabbitMQ 消息队列。
- Shiro 权限管理，Solr 全局检索。
- Spring Cloud 生态圈。
- Java 新特性，Java 函数式编程。

UI关键组件：DateBox，Tree。

**总结**：

- 写接口是基本功，基本功要扎实，接口要可用性高、易读性强。

#### 产品模块树&标签树

完成了：

- 查询公司的模块树与标签树（以全树为基树，选中公司所选节点）。
- 查询用户的模块树与标签树（以公司树为基树，选中用户所选节点）。
- 产品模块树&标签树前端组件（eui-tree）。

学到了：

- 学习了 List => Map => Tree。
- 学习了 Ng-EasyUI（Tree）。

> √：ListConvertToTree，Tree。

#### 邮件服务模块

邮件服务模块（MailService）

完成了：

- 使用 JavaMailSender 实现邮件发送，使用 FreeMarker 模板生成Html邮件。
- 邮件发送：重置密码、解绑微信、授权用户。
- 异步发送邮件。
- 邮件日志记录（SentEmailLog），记录发送失败的异常信息。

学到了：

- 学习了使用 SpringBoot 邮件服务发送邮件。
- 学习了使用 FreeMarker 模板引擎生成 HTML。

> √：JavaMailSender，FreeMarker。

#### 产品时间模块

产品时间模块（ProductTime）

完成了：

- 使用 SpringBoot 开发产品时间模块（的后端接口（增删改查）。
- 使用 Angualr+EasyUI 开发产品时间模块前端组件（初始化/添加/修改/删除）。

学到了：

- 学习 SpringBoot，Spring Data JPA。
- 使用 [Swagger2](http://localhost:8080/swagger-ui.html#/) 测试 RESTful 接口。
- 学习了 Angular2（组件、服务、数据绑定、管道、路由，FormGroup等）。
- 学习了 Ng-EasyUI（DataGrid，Dialog，DateBox）。

> √：SpringBoot，Spring Data JPA，Swagger2，Angular2，Ng-EasyUI。

### 附代码

**ExcelUtils**：

```java
package com.qm.products.prodmp.utils;

import org.apache.poi.ss.usermodel.HorizontalAlignment;
import org.apache.poi.ss.usermodel.IndexedColors;
import org.apache.poi.ss.usermodel.VerticalAlignment;
import org.apache.poi.xssf.usermodel.*;

import java.io.IOException;
import java.io.OutputStream;
import java.util.Date;
import java.util.List;

public class ExcelUtils {

    /**
     * 为趋势图生成Excel.xlsx文件
     *
     * @param dataList Excel 数据
     * @param headers  Excel表头
     * @return XSSFWorkbook Excel工作簿对象
     */
    public static XSSFWorkbook create2007ExcelForTrend(List<Object[]> dataList, String[] headers) {
        // step1 创建一张空表
        XSSFWorkbook workbook = new XSSFWorkbook();
        XSSFSheet sheet = workbook.createSheet();
        int rowCount = dataList.size() + 1;
        int colCount = headers.length;
        for (int i = 0; i < rowCount; i++) {
            XSSFRow row = sheet.createRow(i);
            for (int j = 0; j < colCount; j++) {
                row.createCell(j);
            }
        }
        // step2 定义样式与数据格式
        XSSFDataFormat format = workbook.createDataFormat(); // 单元格数据格式
        // 定义表头样式
        XSSFCellStyle headerStyle = workbook.createCellStyle();
        headerStyle.setDataFormat(format.getFormat("@"));// 强制为文本形式
        headerStyle.setAlignment(HorizontalAlignment.CENTER); // 水平居中
        headerStyle.setVerticalAlignment(VerticalAlignment.CENTER); // 垂直居中
        headerStyle.setFillForegroundColor(IndexedColors.GOLD.getIndex());
        XSSFFont font = workbook.createFont();
        font.setBold(true);
        headerStyle.setFont(font);
        // 定义日期型样式
        XSSFCellStyle dateStyle = workbook.createCellStyle();
        dateStyle.setDataFormat(format.getFormat("yyyy/MM/dd"));
        dateStyle.setAlignment(HorizontalAlignment.LEFT);
        // 定义整数类型样式
        XSSFCellStyle integerStyle = workbook.createCellStyle();
        integerStyle.setDataFormat(format.getFormat("0"));
        integerStyle.setAlignment(HorizontalAlignment.RIGHT);
        // 定义小数数据类型样式
        XSSFCellStyle decimalStyle = workbook.createCellStyle();
        decimalStyle.setDataFormat(format.getFormat("0.00000"));
        decimalStyle.setAlignment(HorizontalAlignment.RIGHT);
        // 定义百分数类型样式
        XSSFCellStyle percentStyle = workbook.createCellStyle();
        percentStyle.setDataFormat(format.getFormat("0.00%"));
        percentStyle.setAlignment(HorizontalAlignment.RIGHT);
        // 定义文本类型样式
        XSSFCellStyle textStyle = workbook.createCellStyle();
        textStyle.setDataFormat(format.getFormat("@"));
        textStyle.setAlignment(HorizontalAlignment.LEFT);

        // step3 写入表头
        XSSFRow rowHeader = sheet.getRow(0);
        for (int i = 0; i < headers.length; i++) {
            XSSFCell cell = rowHeader.getCell(i);
            cell.setCellStyle(headerStyle);
            cell.setCellValue(headers[i]);
            sheet.setColumnWidth(i, (headers[i].length() + 2) * 2 * 256);
        }
        // step4 写入表格数据
        int rowNum = dataList.size();
        for (Object[] data : dataList) {
            XSSFRow row = sheet.getRow(rowNum);
            row.getCell(0).setCellStyle(textStyle);
            row.getCell(0).setCellValue(dataList.size() - rowNum + 1);
            sheet.setColumnWidth(0, 4 * 256);
            for (int j = 0; j < data.length; j++) {
                XSSFCell cell = row.getCell(j + 1);
                Boolean isDate = false; // data[j]是否为日期型
                Boolean isDecimal = false; // data[j]是否为小数型
                Boolean isInteger = false; // data[j]是否为整数
                Boolean isPercent = false; // data[j]是否为百分数
                if (data[j] != null || "".equals(data[j])) {
                    // 判断data[j]是否为日期型
                    isDate = data[j] instanceof Date;
                    //判断data[j]是否为整数型
                    isInteger = String.valueOf(data[j]).matches("^([0-9]{1,})$");
                    //判断data[j]是否为浮点型
                    isDecimal = String.valueOf(data[j]).matches("^([0-9]{1,}[.][0-9]*)$");
                    // 判断data[j]是否为百分数：最后一列是百分数
                    if (j == data.length - 1) {
                        isDecimal = false;
                        isPercent = true;
                    } else {
                        isPercent = false;
                    }
                }
                if (isDate) {
                    cell.setCellStyle(dateStyle);
                    cell.setCellValue(new Date(((Date) data[j]).getTime()));
                    sheet.setColumnWidth(j + 1, 12 * 256);
                } else if (isInteger) {
                    cell.setCellStyle(integerStyle);
                    cell.setCellValue(Integer.parseInt(String.valueOf(data[j])));
                } else if (isDecimal) {
                    cell.setCellStyle(decimalStyle);
                    cell.setCellValue(Double.parseDouble(String.valueOf(data[j])));
                } else if (isPercent) {
                    cell.setCellStyle(percentStyle);
                    cell.setCellValue(Double.parseDouble(String.valueOf(data[j])) / 100);
                } else {
                    cell.setCellStyle(textStyle);
                    cell.setCellValue(data[j] == null ? "" : String.valueOf(data[j]));
                }
            }
            rowNum--;
        }
        return workbook;
    }

    /**
     * 将 workbook 写入输出流中
     *
     * @param workbook Excel工作簿
     * @param out      输出流
     */
    public static void writeToOutputStream(XSSFWorkbook workbook, OutputStream out) {
        // step5 写入输出流中
        try {
            workbook.write(out);
            out.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                out.close();
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                try {
                    workbook.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

}
```

**Angular 文件下载**：

```java
downloadTotalTrend(name: string, tagName: string, selected: string, start: string, end: string): Promise<any> {
  const url = `/api/online/trend-total/download?name=${name}&tagName=${tagName}&company=${selected}&startDate=${start}&endDate=${end}`;
  return this.http.get(url, {responseType: 'blob'}).toPromise().then(res => {
    const a = document.createElement('a');
    const blob = new Blob([res], {'type': 'application/vnd.ms-excel'});
    a.href = URL.createObjectURL(blob);
    a.download = '标签线上数据统计' + end.replace(/-/g, '') + '.xlsx';
    a.click();
  });
}
```

**ListConvertToTree**：

```java
/**
 * ModuleList转换为节点Map
 *
 * @param moduleList 模块List
 * @param checkedIds 选中项的ID序列（以","分割）
 * @return List<TreeNode>
 */
private List<TreeNode> convertToTree(List<Module> moduleList, String checkedIds) {
  TreeNode root = new TreeNode(0, "模块选择", -1);
  Map<Long, TreeNode> map = new HashMap<>();
  // step 1.构造Map<id, node>：遍历List，将所有元素封装为树节点放入Map中
  if (moduleList != null && moduleList.size() > 0) {
     map.put(root.getId(), root);
     for (Module module : moduleList) {
        if (module.getParent() == null) {
           map.put(module.getId(), new TreeNode(module.getId(), module.getName(), root.getId()));
        } else {
           map.put(module.getId(), new TreeNode(module.getId(), module.getName(), module.getParent().getId()));
        }
     }
  } else {
     root.setText("暂无可用标签");
     map.put(root.getId(), root);
  }
  // step 2.连接节点：遍历Map，将所有子节点添加到父节点下
  for (Map.Entry<Long, TreeNode> entry : map.entrySet()) {
     // 如果父节点存在，则添加到父节点下；
     if (map.containsKey(entry.getValue().getParentId())) {
        TreeNode parentNode = map.get(entry.getValue().getParentId());
        if (parentNode.getChildren() == null) {
           parentNode.setChildren(new ArrayList<>());
           parentNode.setState("closed");  // 设为未展开状态
        }
        parentNode.getChildren().add(entry.getValue());
     }
  }
  // step 3.设置选中状态：[-1] 全选；[null] 全不选；其他 部分选。
  if (checkedIds != null && !StringUtils.isEmpty(checkedIds)) {
     if ("-1".equals(checkedIds)) {
        // [-1]：全选
        for (Map.Entry<Long, TreeNode> entry : map.entrySet()) {
           entry.getValue().setCheckState("checked");
        }
     } else {
        // 部分选
        for (Map.Entry<Long, TreeNode> entry : map.entrySet()) {
           List<String> checkedIdList = Arrays.asList(checkedIds.split(","));
           if (checkedIdList.contains(entry.getKey().toString())) {
              entry.getValue().setCheckState("checked");
              // 若选中的是子节点且其父节点不在选中序列中，则父节点设为半选中状态。
              if (entry.getValue().getParentId() != -1 && !checkedIdList.contains(String.valueOf(entry.getValue().getParentId()))){
                 map.get(entry.getValue().getParentId()).setCheckState("indeterminate");
              }
              // 若选中的是父节点，则同时选中其子节点。
              if (entry.getValue().getChildren() != null) {
                 for (TreeNode child : entry.getValue().getChildren()) {
                    child.setCheckState("checked");
                 }
              }
           }
        }
     }
  }
  List<TreeNode> tree = new ArrayList<>();
  tree.add(root);
  return tree;
}
```