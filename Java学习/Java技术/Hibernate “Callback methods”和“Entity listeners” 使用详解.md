## *Hibernate Callback methods*和*Entity listeners*使用详解
   *Callback methods*和Entity Listeners是Hibernate特别有用的特性，有时候会带来很多意想不到的功效哦！所以这里花点时间整理一下关于Callback methods和Entity Listeners的特性和使用方法，供大家查阅。
   Callback methods顾名思义：“回调方法”，作用在Entity类中，结合@Entity。Hibernate支持通过注解和xml的方式轻松对Entity定义回调方法，个性化数据的增删改查。
   
### Hibernate支持的回调注解
**@PrePersist**
Executed before the entity manager persist operation is actually executed or cascaded. This call is synchronous with the persist operation.
在数据持久化到数据库之前执行

**@PreRemove**
Executed before the entity manager remove operation is actually executed or cascaded. This call is synchronous with the remove operation.
执行数据删除之前调用

**@PostPersist**
Executed after the entity manager persist operation is actually executed or cascaded. This call is invoked after the database INSERT is executed.
在执行数据插入之后调用

**@PostRemove**
Executed after the entity manager remove operation is actually executed or cascaded. This call is synchronous with the remove operation.
在执行数据删除之后调用

**@PreUpdate**
Executed before the database UPDATE operation.
在执行数据更新之前调用

**@PostUpdate**
Executed after the database UPDATE operation.
在执行数据更新之后调用

**@PostLoad**
Executed after an entity has been loaded into the current persistence context or an entity has been refreshed.
在数据从数据库加载并且赋值到当前对象后调用

**使用场景**
假设我们持久化的`Entity`都需要保存`createdTime`字段。这个字段比较特殊，在保存数据之前获取当前时间，赋值并保存。
传统的做法：

```
entity.setCreatedTime(new Date());
entityDao.save(entity);
```
使用`Callback methods`的做法
在创建`entity`的`model`类中使用`@PrePersist`，因为我们是在保存数据库之前给其赋值

```
@Entity(...)
public class EntityModel{
  ...
  private Date createdTime;
  @PrePersist
  protected void onCreate() {    
    createdTime= new Date();
  }
}
```
这样我们在业务处理的时候，只需处理业务相关的属性就可以了！
其他回调方法的用法类似，根据场景选择不同的回调就可以了。

### 关于EntityListeners
上面介绍了`Callback methods`，`EntityListeners`其实是定义了多个`Callback methods。`
接上面的示例，加入我们定义的所有的`Entity`都要保存`createdTime`属性，那么就可以定义一个`EntityListener`（一个`Entity`支持多个`EntityListener`的定义），将回调方法定义在其中，然后将`Listener`指定给Entity即可。
示例：
首先我们定义一个名为`CreatedTimePersistentListener`的类

```
public class CreatedTimePersistentListener{
  @PrePersist
  protected void onCreate(Object object) {
    try {
      BeanUtils.setProperty(object, "createdTime", new Date());
    }cache(Exception e){
      //....
    }
  }
}
```
使用`apache`的`BeanUtils`工具类，通过反射的方式，将`createdTime`属性赋值给`object`对象。（`object`对象必须包含`createdTime`属性）

然后通过`@EntityListeners`注解，作用给指定的`Entity`

```
@EntityListeners({CreatedTimePersistentListener.class})
public class EntityModel{
}
```
只要加了`@EntityListeners({CreatedTimePersistentListener.class})`的`Entity`都会默认在保存数据之前执行`createdTime`的赋值。

综合来说，“Callback methods”和“Entity listeners” 使用方法很简单，却非常有用，使我们的代码更容易组织和维护！

