## MyBatis
```java
一、创建mybatis-config.xml
        configuration根节点
        settings（使用mapUnderscoreToCamelCase）把下划线映射成大写字母
        typeAliases注册别名 <typeAlias alias="别名" type="类的完全限定名"/>
        environment数据库配置
        mappers注册mapper <mapper resource="mapper.xml的路径"/>
            
二、创建实体类所对应的Mapper.xml
        mapper根节点 <mapper namespace="所对应接口的完全限定名">
        select
        insert
        delete
        update
        
三、创建MyBatisUtil.java
        读取mybatis-config.xml Reader reader = Resources.getResouceAsReader("mybatis-cinfig.xml");
        创建SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(reader);
            (注：在此类中一般使用static来修饰SqlSessionFactory，因为我们只需要一个SqlSessionFactory，如
            果每次使用时都去创建一个SqlSessionFactory开销是极大的)
        创建一个静态方法返回sqlSessionFactory
        
四、创建实体类在这里不在赘述

五、创建对应Mapper接口
        (注：这里接口的完全限定名应对应到Mapper.xml的namespace，接口的中方法名对应到Mapper.xml中子节点的
        id名，如果需要传入多个参数的话，方法之一是使用注解对应到SQL语句中的占位)
        
六、具体调用
        创建SqlSession 使用MyBatisUtil中的静态方法获取sqlSessionFactory,
            SqlSession sqlSession = sqlSessionFactory().openSession();
        使用动态代理模式获取接口的实现类对象
            eg:StudengMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        使用studengMapper调用其中自定义的方法。
            (注：这里的事务不会自动提交，执行增删改操作后需要调用sqlSession.commit()手动提交事务)
        关闭sqlSession.close()
```