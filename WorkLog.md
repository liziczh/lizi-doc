### QM_Admin

自定义日期组件。

树节点半选中状态。

操作日志记录测试。

表单参数校验。

---

Accordion 的 Lazy Load。

DateBox 与 Calendar组件结合实现日期限制功能：自定义组件，组件注入。

#### 产品模块树&标签树

我完成了：

- 查询公司的模块树与标签树（以全树为基树，选中公司所选节点）。

- 查询用户的模块树与标签树（以公司树为基树，选中用户所选节点）。

- 产品模块树&标签树前端组件（eui-tree）。

我学到了：

- 学习了 List => Map => Tree。
- 学习了 EasyUI for Angular（Tree）。

> √：Tree。

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

#### 邮件服务模块

邮件服务模块（MailService）

我完成了：

- 使用 JavaMailSender 实现邮件发送，使用 FreeMarker 模板生成Html邮件。
- 邮件发送：重置密码、解绑微信、授权用户。
- 异步发送邮件。
- 邮件日志记录（SentEmailLog），记录发送失败的异常信息。

我学到了：

- 学习了使用 SpringBoot 邮件服务发送邮件。
- 学习了使用 FreeMarker 模板引擎生成 HTML。

> √：JavaMailSender，FreeMarker。

#### 产品时间模块

产品时间模块（ProductTime）

我完成了：

- 使用 SpringBoot 开发产品时间模块（的后端接口（增删改查）。
- 使用 Angualr2+EasyUI 开发产品时间模块前端组件（初始化/添加/修改/删除）。

我学到了：

- 学习 SpringBoot，Spring Data JPA。
- 使用 [Swagger2](http://localhost:8080/swagger-ui.html#/) 测试 RESTful 接口。
- 学习了 Angular2（组件、服务、数据绑定、管道、路由，FormGroup等）。
- 学习了 EasyUI for Angular（DataGrid，Dialog，DateBox）。

> √：SpringBoot，Spring Data JPA，Swagger2，Angular2，EasyUI for Angular。