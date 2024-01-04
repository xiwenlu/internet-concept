# 郭秀代码

[Gx_mst大咖](https://weibo.com/u/6010854753?is_all=1)

## 递归
```Java
public  List<Power> parentPower(List<Power> list){
    List<Power>  childList=new ArrayList<Power>();
    for (int i = 0; i < list.size(); i++) {
        Power menus = list.get(i);  		
        List<Power> findTree = poService.queryPower(menus.getId());
        if(findTree.size()>0){  
            List<Power> getchirdMenu = getchirdMenu(findTree);
            menus.setNodes(getchirdMenu);
            childList.add(menus);
        }else{
            childList.add(menus);
        }
    }
    return  childList;
}

public class Power {
    private int  id;
    private String text;
    private String url;
    private int pid;
    private State state;
    private List<Power> nodes;
｝
```

## redis分页
```Java
@Autowired
    private ShardedJedisPool shardedJedisPool;

    public Object queryProduct(Integer offset, Integer limit) {
        //获取jedis
        ShardedJedis jedis = shardedJedisPool.getResource();
        //做标记
        Boolean exists = false;
        do {
            //判断productList是否存在
            exists = jedis.exists("productList");
            //不存在
            if (!exists) {
                //只允许1个请求
                if (jedis.setnx("queryProduct", "1") == 1) {
                    //设置请求的过期时间
                    jedis.expire("queryProduct", 20 * 60);
                    //查询数据库的HQL语句
                    StringBuffer hql = new StringBuffer(
                            "SELECT NEW MAP(p.id AS id) "
                                    + "FROM Product p,Origin o,Product_Origin po "
                                    + " WHERE p.id=po.pid AND o.id=po.oid ");
                    //查询数据库
                    List<Map<String, Object>> queryProduct = (List<Map<String, Object>>) productDao.queryProduct(hql.toString());
                    //循环数据添加到redis中
                    for (Map<String, Object> map : queryProduct) {
                        jedis.lpush("productList", map.get("id").toString());
                    }
                    //设置过期时间
                    jedis.expire("productList", 300 * 650 + new Random().nextInt(100));
                    //删除零时的请求
                    jedis.del("queryProduct");
                } else {
                    try {
                        //别的请求进入的时候让他等待
                        Thread.sleep(50);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            }

        } while (!exists);
        //获取productList中的数据
        List<String> lrange = jedis.lrange("productList", offset, offset + limit - 1);
        //返回页面的数据
        List<JSONObject> jsonobject = new ArrayList<JSONObject>();
        //遍历productList集合获取id
        for (String id : lrange) {
            Boolean b = false;
            do {
                //判断product_是否存在
                b = jedis.exists("product_" + id);
                //product不存在
                if (!b) {
                    //根据id查询product
                    StringBuffer hql = new StringBuffer(
                            "SELECT NEW MAP(p.id AS id ,p.name AS pname,o.name AS oname) "
                                    + "FROM Product p,Origin o,Product_Origin po "
                                    + " WHERE p.id=po.pid AND o.id=po.oid  AND p.id = " + id);
                    //product信息
                    Map map = (Map) productDao.getProductByid(hql.toString());
                    jedis.set("product_" + map.get("id"), JSONObject.toJSONString(map));
                    //设置过期时间
                    jedis.expire("product_" + map.get("id"), 300 * 650 + new Random().nextInt(100));
                }
            } while (!b);
            //获取product完整信息
            String string = jedis.get("product_" + id);
            //添加返回的集合当中
            jsonobject.add(JSONObject.parseObject(string));
        }
        JSONObject json = new JSONObject();
        //总条数
        json.put("total", jedis.llen("productList"));
        //每页条数
        json.put("rows", jsonobject);
        return json;
    }
    
    @Override
    public void delProduct(Integer id) {

        Product p = new Product();
        p.setId(id);
        productDao.delProduct(p);
        StringBuffer hql = new StringBuffer("DELETE FROM Product_Origin WHERE pid = " + id);
        productDao.delProduct_Origin(hql.toString());

        //获取jedis
        ShardedJedis jedis = shardedJedisPool.getResource();
        //删除List集合中的产品ID
        jedis.lrem("productList", 1, id.toString());
        //删除产品的完整信息
        jedis.del("product_" + id);
    }
    
    @Override
    public void updateProduct(Integer pid, String pname, Integer oid) {
        Product p = new Product();
        p.setId(pid);
        p.setName(pname);
        productDao.updateProduct(p);

        StringBuffer hql = new StringBuffer("DELETE FROM Product_Origin WHERE pid = " + pid);
        productDao.delProduct_Origin(hql.toString());

        Product_Origin po = new Product_Origin();
        po.setOid(oid);
        po.setPid(pid);
        productDao.addeProduct_Origin(po);

        //获取jedis
        ShardedJedis jedis = shardedJedisPool.getResource();

        //只需要删除就可以，查询的时候他会进行判断
        jedis.del("product_" + pid);

    }

    @Override
    public void addProduct(String pname, Integer oid) {

        Product p = new Product();
        p.setName(pname);
        Integer pid = productDao.addProduct(p);

        Product_Origin po = new Product_Origin();
        po.setOid(oid);
        po.setPid(pid);
        productDao.addeProduct_Origin(po);

        Map<String, Object> map = new HashMap<String, Object>();
        map.put("id", pid);
        map.put("pname", pname);

        ShardedJedis jedis = shardedJedisPool.getResource();

        //往产品集合存刚新增产品ID
        jedis.lpush("productList", pid.toString());

        //不用往redis添加商品详细信息,做查询的时候判断到不存在的时候会自己去数据库查询放入缓存
    }


```
