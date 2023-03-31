title: 使用mybatis进行分页查询
author: 远方
tags:
  - LeetCode
  - 算法
categories:
  - LeetCode破局攻略
date: 2016-01-01 19:20:00
---
```java
public PageUtils queryPage(Map<String, Object> params, Long catelogId) {
        if(catelogId==0){
            IPage<AttrGroupEntity> page = this.page(new Query<AttrGroupEntity>().getPage(params),
                    new QueryWrapper<AttrGroupEntity>());
            return new PageUtils(page);
        }
//        从参数中获取一个key,key是检索关键字,用户查询的数据
        String key=(String) params.get("key");
//        select * from pms_attr_group where catelog_id=? and(attr_group_id = key or attr_group_name like %?%)
        QueryWrapper<AttrGroupEntity> wrapper = new QueryWrapper<AttrGroupEntity>().eq("catelog_id", catelogId);
        if(!StringUtils.isEmpty(key)){
//            and接受一个consumer,函数式编程
            wrapper.and(obj-> obj.eq("attr_group_id",key).or().like("attr_group_name", key));
        }
        IPage<AttrGroupEntity> page = page(new Query<AttrGroupEntity>().getPage(params),
                wrapper);

        return new PageUtils(page);
    }
```



在这行代码中，有一些需要注意的点：

1. this.page（）,可以使用page（），这个方法是在IService接口中进行定义了的。

    

    ```java
    default <E extends IPage<T>> E page(E page, Wrapper<T> queryWrapper) {
            return this.getBaseMapper().selectPage(page, queryWrapper);
        }
    
        default <E extends IPage<T>> E page(E page) {
            return this.page(page, Wrappers.emptyWrapper());
        }
    ```

    所以可以通过this直接调用、使用。

2. 在这个page方法中需要传入连个参数，一个是查询的表，需要new query出来，还有一个就是查询的条件，wrapper。最后通过分装类进行返回数据（包含记录数以及传输的数据等信息）

​		

```java
public class PageUtils implements Serializable {
	private static final long serialVersionUID = 1L;
	/**
	 * 总记录数
	 */
	private int totalCount;
	/**
	 * 每页记录数
	 */
	private int pageSize;
	/**
	 * 总页数
	 */
	private int totalPage;
	/**
	 * 当前页数
	 */
	private int currPage;
	/**
	 * 列表数据
	 */
	private List<?> list;
	
	/**
	 * 分页
	 * @param list        列表数据
	 * @param totalCount  总记录数
	 * @param pageSize    每页记录数
	 * @param currPage    当前页数
	 */
	public PageUtils(List<?> list, int totalCount, int pageSize, int currPage) {
		this.list = list;
		this.totalCount = totalCount;
		this.pageSize = pageSize;
		this.currPage = currPage;
		this.totalPage = (int)Math.ceil((double)totalCount/pageSize);
	}

	/**
	 * 分页
	 */
	public PageUtils(IPage<?> page) {
		this.list = page.getRecords();
		this.totalCount = (int)page.getTotal();
		this.pageSize = (int)page.getSize();
		this.currPage = (int)page.getCurrent();
		this.totalPage = (int)page.getPages();
	}

	public int getTotalCount() {
		return totalCount;
	}

	public void setTotalCount(int totalCount) {
		this.totalCount = totalCount;
	}

	public int getPageSize() {
		return pageSize;
	}

	public void setPageSize(int pageSize) {
		this.pageSize = pageSize;
	}

	public int getTotalPage() {
		return totalPage;
	}

	public void setTotalPage(int totalPage) {
		this.totalPage = totalPage;
	}

	public int getCurrPage() {
		return currPage;
	}

	public void setCurrPage(int currPage) {
		this.currPage = currPage;
	}

	public List<?> getList() {
		return list;
	}

	public void setList(List<?> list) {
		this.list = list;
	}
	
}
```

