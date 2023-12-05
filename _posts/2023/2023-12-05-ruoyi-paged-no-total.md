---
title: 若以分页失效，显示总数为当前查询集合数量错误
date: 2023-12-04 09:47:12 UTC+8
categories: [Java, Library]
tags: [java, 若以分页]     # TAG names should always be lowercase
# author: 1
---

修改 Controller 代码如下所示
```java
@GetMapping("/list")
public TableDataInfo pagedList(LzPatrolRecordPagedListRequestVO lzPatrolRecordPagedListRequestVO) {
    // startPage();
    PageDomain pageDomain = TableSupport.buildPageRequest();
    Page<Object> page = PageHelper.startPage(pageDomain.getPageNum(), pageDomain.getPageSize());
    List<LzPatrolRecordPagedListResponseVO> responseVOList = lzPatrolRecordService.pagedList(lzPatrolRecordPagedListRequestVO);
    TableDataInfo dataTable = getDataTable(responseVOList);
    dataTable.setTotal(page.getTotal());
    return dataTable;
}
```