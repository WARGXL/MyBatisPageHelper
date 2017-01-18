# MyBatisPageHelper

<div id="main">
	
<div id="post_detail">
<div class="post">
	<div class="postTitle">
		<a id="cb_post_title_url" href="http://www.cnblogs.com/digdeep/p/4608933.html">Mybatis 数据库物理分页插件 PageHelper</a>
	</div>
	
	<div class="postText">
		<div id="cnblogs_post_body"><p>以前使用ibatis/mybatis，都是自己手写sql语句进行物理分页，虽然稍微有点麻烦，但是都习惯了。最近试用了下mybatis的分页插件 PageHelper,感觉还不错吧。记录下其使用方法。</p>
<p><span style="color: #800000;"><strong>1.</strong> </span>引入依赖jar包：</p>
<div class="cnblogs_code">
<pre>    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">dependency</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">groupId</span><span style="color: #0000ff;">&gt;</span>com.github.pagehelper<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">groupId</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">artifactId</span><span style="color: #0000ff;">&gt;</span>pagehelper<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">artifactId</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">version</span><span style="color: #0000ff;">&gt;</span>3.7.5<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">version</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">dependency</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p><span style="color: #800000;"><strong>2.</strong> </span>配置分页拦截器</p>
<p>PageHelper的原理是基于拦截器实现的。拦截器的配置有两种方法，一种是在mybatis的配置文件中配置，一种是直接在spring的配置文件中进行：</p>
<p>1）在mybatis-config.xml文件中配置：</p>
<div class="cnblogs_code">
<pre>  <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">plugins</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> com.github.pagehelper为PageHelper类所在包名 </span><span style="color: #008000;">--&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">plugin </span><span style="color: #ff0000;">interceptor</span><span style="color: #0000ff;">="com.github.pagehelper.PageHelper"</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="dialect"</span><span style="color: #ff0000;"> value</span><span style="color: #0000ff;">="mysql"</span><span style="color: #0000ff;">/&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 该参数默认为false </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 设置为true时，会将RowBounds第一个参数offset当成pageNum页码使用 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 和startPage中的pageNum效果一样</span><span style="color: #008000;">--&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="offsetAsPageNum"</span><span style="color: #ff0000;"> value</span><span style="color: #0000ff;">="true"</span><span style="color: #0000ff;">/&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 该参数默认为false </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 设置为true时，使用RowBounds分页会进行count查询 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="rowBoundsWithCount"</span><span style="color: #ff0000;"> value</span><span style="color: #0000ff;">="true"</span><span style="color: #0000ff;">/&gt;</span>
        
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 设置为true时，如果pageSize=0或者RowBounds.limit = 0就会查询出全部的结果 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> （相当于没有执行分页查询，但是返回结果仍然是Page类型）
        &lt;property name="pageSizeZero" value="true"/&gt;</span><span style="color: #008000;">--&gt;</span>
        
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 3.3.0版本可用 - 分页参数合理化，默认false禁用 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 启用合理化时，如果pageNum&lt;1会查询第一页，如果pageNum&gt;pages会查询最后一页 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 禁用合理化时，如果pageNum&lt;1或pageNum&gt;pages会返回空数据 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="reasonable"</span><span style="color: #ff0000;"> value</span><span style="color: #0000ff;">="true"</span><span style="color: #0000ff;">/&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 3.5.0版本可用 - 为了支持startPage(Object params)方法 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 增加了一个`params`参数来配置参数映射，用于从Map或ServletRequest中取值 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 可以配置pageNum,pageSize,count,pageSizeZero,reasonable,不配置映射的用默认值 </span><span style="color: #008000;">--&gt;</span>
        <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> 不理解该含义的前提下，不要随便复制该配置 
        &lt;property name="params" value="pageNum=start;pageSize=limit;"/&gt;    </span><span style="color: #008000;">--&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">plugin</span><span style="color: #0000ff;">&gt;</span>
  <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">plugins</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p>这里要注意 &lt;plugins&gt; 在mybatis-config.xml文件中的位置，必须要符合 http://mybatis.org/dtd/mybatis-3-config.dtd 中指定的顺序：</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">&lt;!</span><span style="color: #ff00ff;">ELEMENT configuration (properties?, settings?, typeAliases?, typeHandlers?, <br />    objectFactory?, objectWrapperFactory?, plugins?, environments?, databaseIdProvider?, mappers?)</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p>不然会报错。</p>
<p>当然mybatis-config.xml的位置，我们需要在spring的配置文件中进行指定：</p>
<div class="cnblogs_code">
<pre>    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">bean </span><span style="color: #ff0000;">id</span><span style="color: #0000ff;">="sqlSessionFactory"</span><span style="color: #ff0000;"> class</span><span style="color: #0000ff;">="org.mybatis.spring.SqlSessionFactoryBean"</span><span style="color: #0000ff;">&gt;</span>
      <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="dataSource"</span><span style="color: #ff0000;"> ref</span><span style="color: #0000ff;">="dataSource"</span> <span style="color: #0000ff;">/&gt;</span>
      <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="configLocation"</span><span style="color: #ff0000;"> value</span><span style="color: #0000ff;">="classpath:config/mybatis-config.xml"</span> <span style="color: #0000ff;">/&gt;</span>
      <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="mapperLocations"</span><span style="color: #ff0000;"> value</span><span style="color: #0000ff;">="classpath*:config/mappers/**/*.xml"</span> <span style="color: #0000ff;">/&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">bean</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p>2）如果mybatis没有mybatis-config.xml文件，那么就只能直接在spring的配置文件中配置了：</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">&lt;</span><span style="color: #800000;">bean </span><span style="color: #ff0000;">id</span><span style="color: #0000ff;">="sqlSessionFactory"</span><span style="color: #ff0000;"> class</span><span style="color: #0000ff;">="org.mybatis.spring.SqlSessionFactoryBean"</span><span style="color: #0000ff;">&gt;</span>
  <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="dataSource"</span><span style="color: #ff0000;"> ref</span><span style="color: #0000ff;">="dataSource"</span><span style="color: #0000ff;">/&gt;</span>
  <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="mapperLocations"</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">array</span><span style="color: #0000ff;">&gt;</span>
      <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">value</span><span style="color: #0000ff;">&gt;</span>classpath:config/mapper/*.xml<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">value</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">array</span><span style="color: #0000ff;">&gt;</span>
  <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">property</span><span style="color: #0000ff;">&gt;</span>
  <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="typeAliasesPackage"</span><span style="color: #ff0000;"> value</span><span style="color: #0000ff;">="com.test.pojo"</span><span style="color: #0000ff;">/&gt;</span>
  <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="plugins"</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">array</span><span style="color: #0000ff;">&gt;</span>
      <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">bean </span><span style="color: #ff0000;">class</span><span style="color: #0000ff;">="com.github.pagehelper.PageHelper"</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">property </span><span style="color: #ff0000;">name</span><span style="color: #0000ff;">="properties"</span><span style="color: #0000ff;">&gt;</span>
          <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">value</span><span style="color: #0000ff;">&gt;</span><span style="color: #000000;">
            dialect=mysql
          </span><span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">value</span><span style="color: #0000ff;">&gt;</span>
        <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">property</span><span style="color: #0000ff;">&gt;</span>
      <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">bean</span><span style="color: #0000ff;">&gt;</span>
    <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">array</span><span style="color: #0000ff;">&gt;</span>
  <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">property</span><span style="color: #0000ff;">&gt;</span>
<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">bean</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p>到这里PageHelper所需要的配置已经完成，下面还需要在serviceImpl类中加入分页参数的代码：</p>
<p><strong><span style="color: #800000;">3.</span></strong> 向拦截器传递分页参数</p>
<p>我们首先看下不分页的serviceImpl代码：</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">    @Override
    </span><span style="color: #0000ff;">public</span> List&lt;User&gt;<span style="color: #000000;"> getUserByNoAndEmail(String no, String email) {
        Map</span>&lt;String, Object&gt; map = <span style="color: #0000ff;">new</span> HashMap&lt;&gt;<span style="color: #000000;">();
        map.put(</span>"no"<span style="color: #000000;">, no);
        map.put(</span>"email"<span style="color: #000000;">, email);
        </span><span style="color: #0000ff;">return</span> <span style="color: #0000ff;">this</span><span style="color: #000000;">.userMapper.getUserByNoAndEmail(map);
    }</span></pre>
</div>
<p>然后我们将它改造成使用PageHelper分页：</p>
<p>1）首先我们根据自己项目的情况，定义一个PageBean，来保存分页之后的结果，需要哪些属性，就加入哪些属性，具体可以参考源代码中的PageInfo类的定义，其实<strong>PageInfo是插件作者给我们自己定义自己的PageBean，提供的一个参考例子</strong>。PageInfo代码如下：</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('555ac12c-47d5-4fa4-87f9-777a52b939e7')"><img id="code_img_closed_555ac12c-47d5-4fa4-87f9-777a52b939e7" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_555ac12c-47d5-4fa4-87f9-777a52b939e7" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('555ac12c-47d5-4fa4-87f9-777a52b939e7',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_555ac12c-47d5-4fa4-87f9-777a52b939e7" class="cnblogs_code_hide">
<pre>@SuppressWarnings({"rawtypes", "unchecked"<span style="color: #000000;">})
</span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span> PageInfo&lt;T&gt; <span style="color: #0000ff;">implements</span><span style="color: #000000;"> Serializable {
    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">final</span> <span style="color: #0000ff;">long</span> serialVersionUID = 1L<span style="color: #000000;">;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">当前页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> pageNum;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">每页的数量</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> pageSize;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">当前页的数量</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> size;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">由于startRow和endRow不常用，这里说个具体的用法
    </span><span style="color: #008000;">//</span><span style="color: #008000;">可以在页面中"显示startRow到endRow 共size条数据"

    </span><span style="color: #008000;">//</span><span style="color: #008000;">当前页面第一个元素在数据库中的行号</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> startRow;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">当前页面最后一个元素在数据库中的行号</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> endRow;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">总记录数</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">long</span><span style="color: #000000;"> total;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">总页数</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> pages;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">结果集</span>
    <span style="color: #0000ff;">private</span> List&lt;T&gt;<span style="color: #000000;"> list;

    </span><span style="color: #008000;">//</span><span style="color: #008000;">第一页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> firstPage;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">前一页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> prePage;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">下一页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> nextPage;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">最后一页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> lastPage;

    </span><span style="color: #008000;">//</span><span style="color: #008000;">是否为第一页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">boolean</span> isFirstPage = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">是否为最后一页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">boolean</span> isLastPage = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">是否有前一页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">boolean</span> hasPreviousPage = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">是否有下一页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">boolean</span> hasNextPage = <span style="color: #0000ff;">false</span><span style="color: #000000;">;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">导航页码数</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> navigatePages;
    </span><span style="color: #008000;">//</span><span style="color: #008000;">所有导航页号</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span><span style="color: #000000;">[] navigatepageNums;

    </span><span style="color: #008000;">/**</span><span style="color: #008000;">
     * 包装Page对象
     *
     * </span><span style="color: #808080;">@param</span><span style="color: #008000;"> list
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">public</span> PageInfo(List&lt;T&gt;<span style="color: #000000;"> list) {
        </span><span style="color: #0000ff;">this</span>(list, 8<span style="color: #000000;">);
    }

    </span><span style="color: #008000;">/**</span><span style="color: #008000;">
     * 包装Page对象
     *
     * </span><span style="color: #808080;">@param</span><span style="color: #008000;"> list          page结果
     * </span><span style="color: #808080;">@param</span><span style="color: #008000;"> navigatePages 页码数量
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">public</span> PageInfo(List&lt;T&gt; list, <span style="color: #0000ff;">int</span><span style="color: #000000;"> navigatePages) {
        </span><span style="color: #0000ff;">if</span> (list <span style="color: #0000ff;">instanceof</span><span style="color: #000000;"> Page) {
            Page page </span>=<span style="color: #000000;"> (Page) list;
            </span><span style="color: #0000ff;">this</span>.pageNum =<span style="color: #000000;"> page.getPageNum();
            </span><span style="color: #0000ff;">this</span>.pageSize =<span style="color: #000000;"> page.getPageSize();

            </span><span style="color: #0000ff;">this</span>.total =<span style="color: #000000;"> page.getTotal();
            </span><span style="color: #0000ff;">this</span>.pages =<span style="color: #000000;"> page.getPages();
            </span><span style="color: #0000ff;">this</span>.list =<span style="color: #000000;"> page;
            </span><span style="color: #0000ff;">this</span>.size =<span style="color: #000000;"> page.size();
            </span><span style="color: #008000;">//</span><span style="color: #008000;">由于结果是&gt;startRow的，所以实际的需要+1</span>
            <span style="color: #0000ff;">if</span> (<span style="color: #0000ff;">this</span>.size == 0<span style="color: #000000;">) {
                </span><span style="color: #0000ff;">this</span>.startRow = 0<span style="color: #000000;">;
                </span><span style="color: #0000ff;">this</span>.endRow = 0<span style="color: #000000;">;
            } </span><span style="color: #0000ff;">else</span><span style="color: #000000;"> {
                </span><span style="color: #0000ff;">this</span>.startRow = page.getStartRow() + 1<span style="color: #000000;">;
                </span><span style="color: #008000;">//</span><span style="color: #008000;">计算实际的endRow（最后一页的时候特殊）</span>
                <span style="color: #0000ff;">this</span>.endRow = <span style="color: #0000ff;">this</span>.startRow - 1 + <span style="color: #0000ff;">this</span><span style="color: #000000;">.size;
            }
            </span><span style="color: #0000ff;">this</span>.navigatePages =<span style="color: #000000;"> navigatePages;
            </span><span style="color: #008000;">//</span><span style="color: #008000;">计算导航页</span>
<span style="color: #000000;">            calcNavigatepageNums();
            </span><span style="color: #008000;">//</span><span style="color: #008000;">计算前后页，第一页，最后一页</span>
<span style="color: #000000;">            calcPage();
            </span><span style="color: #008000;">//</span><span style="color: #008000;">判断页面边界</span>
<span style="color: #000000;">            judgePageBoudary();
        }
    }

    </span><span style="color: #008000;">/**</span><span style="color: #008000;">
     * 计算导航页
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> calcNavigatepageNums() {
        </span><span style="color: #008000;">//</span><span style="color: #008000;">当总页数小于或等于导航页码数时</span>
        <span style="color: #0000ff;">if</span> (pages &lt;=<span style="color: #000000;"> navigatePages) {
            navigatepageNums </span>= <span style="color: #0000ff;">new</span> <span style="color: #0000ff;">int</span><span style="color: #000000;">[pages];
            </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = 0; i &lt; pages; i++<span style="color: #000000;">) {
                navigatepageNums[i] </span>= i + 1<span style="color: #000000;">;
            }
        } </span><span style="color: #0000ff;">else</span> { <span style="color: #008000;">//</span><span style="color: #008000;">当总页数大于导航页码数时</span>
            navigatepageNums = <span style="color: #0000ff;">new</span> <span style="color: #0000ff;">int</span><span style="color: #000000;">[navigatePages];
            </span><span style="color: #0000ff;">int</span> startNum = pageNum - navigatePages / 2<span style="color: #000000;">;
            </span><span style="color: #0000ff;">int</span> endNum = pageNum + navigatePages / 2<span style="color: #000000;">;

            </span><span style="color: #0000ff;">if</span> (startNum &lt; 1<span style="color: #000000;">) {
                startNum </span>= 1<span style="color: #000000;">;
                </span><span style="color: #008000;">//</span><span style="color: #008000;">(最前navigatePages页</span>
                <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = 0; i &lt; navigatePages; i++<span style="color: #000000;">) {
                    navigatepageNums[i] </span>= startNum++<span style="color: #000000;">;
                }
            } </span><span style="color: #0000ff;">else</span> <span style="color: #0000ff;">if</span> (endNum &gt;<span style="color: #000000;"> pages) {
                endNum </span>=<span style="color: #000000;"> pages;
                </span><span style="color: #008000;">//</span><span style="color: #008000;">最后navigatePages页</span>
                <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = navigatePages - 1; i &gt;= 0; i--<span style="color: #000000;">) {
                    navigatepageNums[i] </span>= endNum--<span style="color: #000000;">;
                }
            } </span><span style="color: #0000ff;">else</span><span style="color: #000000;"> {
                </span><span style="color: #008000;">//</span><span style="color: #008000;">所有中间页</span>
                <span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = 0; i &lt; navigatePages; i++<span style="color: #000000;">) {
                    navigatepageNums[i] </span>= startNum++<span style="color: #000000;">;
                }
            }
        }
    }

    </span><span style="color: #008000;">/**</span><span style="color: #008000;">
     * 计算前后页，第一页，最后一页
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> calcPage() {
        </span><span style="color: #0000ff;">if</span> (navigatepageNums != <span style="color: #0000ff;">null</span> &amp;&amp; navigatepageNums.length &gt; 0<span style="color: #000000;">) {
            firstPage </span>= navigatepageNums[0<span style="color: #000000;">];
            lastPage </span>= navigatepageNums[navigatepageNums.length - 1<span style="color: #000000;">];
            </span><span style="color: #0000ff;">if</span> (pageNum &gt; 1<span style="color: #000000;">) {
                prePage </span>= pageNum - 1<span style="color: #000000;">;
            }
            </span><span style="color: #0000ff;">if</span> (pageNum &lt;<span style="color: #000000;"> pages) {
                nextPage </span>= pageNum + 1<span style="color: #000000;">;
            }
        }
    }

    </span><span style="color: #008000;">/**</span><span style="color: #008000;">
     * 判定页面边界
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> judgePageBoudary() {
        isFirstPage </span>= pageNum == 1<span style="color: #000000;">;
        isLastPage </span>= pageNum ==<span style="color: #000000;"> pages;
        hasPreviousPage </span>= pageNum &gt; 1<span style="color: #000000;">;
        hasNextPage </span>= pageNum &lt;<span style="color: #000000;"> pages;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> setPageNum(<span style="color: #0000ff;">int</span><span style="color: #000000;"> pageNum) {
        </span><span style="color: #0000ff;">this</span>.pageNum =<span style="color: #000000;"> pageNum;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPageNum() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> pageNum;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPageSize() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> pageSize;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getSize() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> size;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getStartRow() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> startRow;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getEndRow() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> endRow;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">long</span><span style="color: #000000;"> getTotal() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> total;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPages() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> pages;
    }

    </span><span style="color: #0000ff;">public</span> List&lt;T&gt;<span style="color: #000000;"> getList() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> list;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getFirstPage() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> firstPage;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPrePage() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> prePage;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getNextPage() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> nextPage;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getLastPage() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> lastPage;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">boolean</span><span style="color: #000000;"> isIsFirstPage() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> isFirstPage;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">boolean</span><span style="color: #000000;"> isIsLastPage() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> isLastPage;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">boolean</span><span style="color: #000000;"> isHasPreviousPage() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> hasPreviousPage;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">boolean</span><span style="color: #000000;"> isHasNextPage() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> hasNextPage;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getNavigatePages() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> navigatePages;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;">[] getNavigatepageNums() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> navigatepageNums;
    }

    @Override
    </span><span style="color: #0000ff;">public</span><span style="color: #000000;"> String toString() {
        </span><span style="color: #0000ff;">final</span> StringBuffer sb = <span style="color: #0000ff;">new</span> StringBuffer("PageInfo{"<span style="color: #000000;">);
        sb.append(</span>"pageNum="<span style="color: #000000;">).append(pageNum);
        sb.append(</span>", pageSize="<span style="color: #000000;">).append(pageSize);
        sb.append(</span>", size="<span style="color: #000000;">).append(size);
        sb.append(</span>", startRow="<span style="color: #000000;">).append(startRow);
        sb.append(</span>", endRow="<span style="color: #000000;">).append(endRow);
        sb.append(</span>", total="<span style="color: #000000;">).append(total);
        sb.append(</span>", pages="<span style="color: #000000;">).append(pages);
        sb.append(</span>", list="<span style="color: #000000;">).append(list);
        sb.append(</span>", firstPage="<span style="color: #000000;">).append(firstPage);
        sb.append(</span>", prePage="<span style="color: #000000;">).append(prePage);
        sb.append(</span>", nextPage="<span style="color: #000000;">).append(nextPage);
        sb.append(</span>", lastPage="<span style="color: #000000;">).append(lastPage);
        sb.append(</span>", isFirstPage="<span style="color: #000000;">).append(isFirstPage);
        sb.append(</span>", isLastPage="<span style="color: #000000;">).append(isLastPage);
        sb.append(</span>", hasPreviousPage="<span style="color: #000000;">).append(hasPreviousPage);
        sb.append(</span>", hasNextPage="<span style="color: #000000;">).append(hasNextPage);
        sb.append(</span>", navigatePages="<span style="color: #000000;">).append(navigatePages);
        sb.append(</span>", navigatepageNums="<span style="color: #000000;">);
        </span><span style="color: #0000ff;">if</span> (navigatepageNums == <span style="color: #0000ff;">null</span>) sb.append("null"<span style="color: #000000;">);
        </span><span style="color: #0000ff;">else</span><span style="color: #000000;"> {
            sb.append(</span>'['<span style="color: #000000;">);
            </span><span style="color: #0000ff;">for</span> (<span style="color: #0000ff;">int</span> i = 0; i &lt; navigatepageNums.length; ++<span style="color: #000000;">i)
                sb.append(i </span>== 0 ? "" : ", "<span style="color: #000000;">).append(navigatepageNums[i]);
            sb.append(</span>']'<span style="color: #000000;">);
        }
        sb.append(</span>'}'<span style="color: #000000;">);
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> sb.toString();
    }
}</span></pre>
</div>
<span class="cnblogs_code_collapse">PageInfo.java</span></div>
<p>因为PageInfo.java只是一个示例，所以他定义得有点重量级，属性有点多，我们可以参考它，定义适合我们自己的PageBean, 比如如下定义：</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('e5058794-d5c6-4e7e-962a-bbeb03af70a5')"><img id="code_img_closed_e5058794-d5c6-4e7e-962a-bbeb03af70a5" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_e5058794-d5c6-4e7e-962a-bbeb03af70a5" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('e5058794-d5c6-4e7e-962a-bbeb03af70a5',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_e5058794-d5c6-4e7e-962a-bbeb03af70a5" class="cnblogs_code_hide">
<pre><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span> PageBean&lt;T&gt; <span style="color: #0000ff;">implements</span><span style="color: #000000;"> Serializable {
    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">final</span> <span style="color: #0000ff;">long</span> serialVersionUID = 8656597559014685635L<span style="color: #000000;">;
    </span><span style="color: #0000ff;">private</span> <span style="color: #0000ff;">long</span> total;        <span style="color: #008000;">//</span><span style="color: #008000;">总记录数</span>
    <span style="color: #0000ff;">private</span> List&lt;T&gt; list;    <span style="color: #008000;">//</span><span style="color: #008000;">结果集</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> pageNum;    <span style="color: #008000;">//</span><span style="color: #008000;"> 第几页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> pageSize;    <span style="color: #008000;">//</span><span style="color: #008000;"> 每页记录数</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> pages;        <span style="color: #008000;">//</span><span style="color: #008000;"> 总页数</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">int</span> size;        <span style="color: #008000;">//</span><span style="color: #008000;"> 当前页的数量 &lt;= pageSize，该属性来自ArrayList的size属性</span>
    
    <span style="color: #008000;">/**</span><span style="color: #008000;">
     * 包装Page对象，因为直接返回Page对象，在JSON处理以及其他情况下会被当成List来处理，
     * 而出现一些问题。
     * </span><span style="color: #808080;">@param</span><span style="color: #008000;"> list          page结果
     * </span><span style="color: #808080;">@param</span><span style="color: #008000;"> navigatePages 页码数量
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">public</span> PageBean(List&lt;T&gt;<span style="color: #000000;"> list) {
        </span><span style="color: #0000ff;">if</span> (list <span style="color: #0000ff;">instanceof</span><span style="color: #000000;"> Page) {
            Page</span>&lt;T&gt; page = (Page&lt;T&gt;<span style="color: #000000;">) list;
            </span><span style="color: #0000ff;">this</span>.pageNum =<span style="color: #000000;"> page.getPageNum();
            </span><span style="color: #0000ff;">this</span>.pageSize =<span style="color: #000000;"> page.getPageSize();
            </span><span style="color: #0000ff;">this</span>.total =<span style="color: #000000;"> page.getTotal();
            </span><span style="color: #0000ff;">this</span>.pages =<span style="color: #000000;"> page.getPages();
            </span><span style="color: #0000ff;">this</span>.list =<span style="color: #000000;"> page;
            </span><span style="color: #0000ff;">this</span>.size =<span style="color: #000000;"> page.size();
        }
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">long</span><span style="color: #000000;"> getTotal() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> total;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> setTotal(<span style="color: #0000ff;">long</span><span style="color: #000000;"> total) {
        </span><span style="color: #0000ff;">this</span>.total =<span style="color: #000000;"> total;
    }

    </span><span style="color: #0000ff;">public</span> List&lt;T&gt;<span style="color: #000000;"> getList() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> list;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> setList(List&lt;T&gt;<span style="color: #000000;"> list) {
        </span><span style="color: #0000ff;">this</span>.list =<span style="color: #000000;"> list;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPageNum() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> pageNum;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> setPageNum(<span style="color: #0000ff;">int</span><span style="color: #000000;"> pageNum) {
        </span><span style="color: #0000ff;">this</span>.pageNum =<span style="color: #000000;"> pageNum;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPageSize() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> pageSize;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> setPageSize(<span style="color: #0000ff;">int</span><span style="color: #000000;"> pageSize) {
        </span><span style="color: #0000ff;">this</span>.pageSize =<span style="color: #000000;"> pageSize;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPages() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> pages;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> setPages(<span style="color: #0000ff;">int</span><span style="color: #000000;"> pages) {
        </span><span style="color: #0000ff;">this</span>.pages =<span style="color: #000000;"> pages;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getSize() {
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> size;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">void</span> setSize(<span style="color: #0000ff;">int</span><span style="color: #000000;"> size) {
        </span><span style="color: #0000ff;">this</span>.size =<span style="color: #000000;"> size;
    }
    
}</span></pre>
</div>
<span class="cnblogs_code_collapse">PageBean.java</span></div>
<p><strong>因为分页查询结果返回的是一个 Page 对象，而 Page 对象继承自ArrayList，但是如果我们直接返回ArrayList的话，在一些场景下回遇到问题，比如在JSON处理Page类型的结果时，会被当成List来JSON格式化，会丢弃 Page 对象的所有扩展属性</strong>，所以这里我们要<strong>将分页的结果 Page 类型转换成我们自己定义的 PageBean. 我们自己定义的PageBean没有继承ArrayList，而是包含一个List属性来保存分页结果</strong>。所以避免前面的问题。</p>
<p>2）修改 serviceImpl中的代码：</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">    @Override
    </span><span style="color: #0000ff;">public</span> PageBean&lt;User&gt;<span style="color: #000000;"> getUserByNoAndEmail(String no, String email) {
        Map</span>&lt;String, Object&gt; map = <span style="color: #0000ff;">new</span> HashMap&lt;&gt;<span style="color: #000000;">();
        map.put(</span>"no"<span style="color: #000000;">, no);
        map.put(</span>"email"<span style="color: #000000;">, email);
        
        PageHelper.startPage(PaginationContext.getPageNum(), PaginationContext.getPageSize());
        List</span>&lt;User&gt; list = <span style="color: #0000ff;">this</span><span style="color: #000000;">.userMapper.getUserByNoAndEmail(map);
        </span><span style="color: #0000ff;">return</span> <span style="color: #0000ff;">new</span> PageBean&lt;User&gt;<span style="color: #000000;">(list);
    }</span></pre>
</div>
<p>我们只需要使用 PageHelper.startPage(pageNum, pageSize); 函数来指定 pageNum(第几页) 和 pageSize(每页显示几条记录) 两个参数。然后调用原来的查询，就进行了分页。最后将返回的List，转换成 PageBean类型的结果即可。前台页面就可以根据PageBean中包括的属性来进行分页显示了。</p>
<p>上面的 <span style="color: #000000;">PaginationContext</span> 是基于 ThreadLocal 来传递分页参数的一个工具类，其实现如下：</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('c03e273c-a17e-4bf3-8c08-c8a2f1c0723b')"><img id="code_img_closed_c03e273c-a17e-4bf3-8c08-c8a2f1c0723b" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_c03e273c-a17e-4bf3-8c08-c8a2f1c0723b" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('c03e273c-a17e-4bf3-8c08-c8a2f1c0723b',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_c03e273c-a17e-4bf3-8c08-c8a2f1c0723b" class="cnblogs_code_hide">
<pre><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">class</span><span style="color: #000000;"> PaginationContext {
    </span><span style="color: #008000;">//</span><span style="color: #008000;"> 定义两个threadLocal变量：pageNum和pageSize</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">static</span> ThreadLocal&lt;Integer&gt; pageNum = <span style="color: #0000ff;">new</span> ThreadLocal&lt;Integer&gt;();    <span style="color: #008000;">//</span><span style="color: #008000;"> 保存第几页</span>
    <span style="color: #0000ff;">private</span> <span style="color: #0000ff;">static</span> ThreadLocal&lt;Integer&gt; pageSize = <span style="color: #0000ff;">new</span> ThreadLocal&lt;Integer&gt;();    <span style="color: #008000;">//</span><span style="color: #008000;"> 保存每页记录条数</span>

    <span style="color: #008000;">/*</span><span style="color: #008000;">
     * pageNum ：get、set、remove
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPageNum() {
        Integer pn </span>=<span style="color: #000000;"> pageNum.get();
        </span><span style="color: #0000ff;">if</span> (pn == <span style="color: #0000ff;">null</span><span style="color: #000000;">) {
            </span><span style="color: #0000ff;">return</span> 0<span style="color: #000000;">;
        }
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> pn;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> setPageNum(<span style="color: #0000ff;">int</span><span style="color: #000000;"> pageNumValue) {
        pageNum.set(pageNumValue);
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> removePageNum() {
        pageNum.remove();
    }

    </span><span style="color: #008000;">/*</span><span style="color: #008000;">
     * pageSize ：get、set、remove
     </span><span style="color: #008000;">*/</span>
    <span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">int</span><span style="color: #000000;"> getPageSize() {
        Integer ps </span>=<span style="color: #000000;"> pageSize.get();
        </span><span style="color: #0000ff;">if</span> (ps == <span style="color: #0000ff;">null</span><span style="color: #000000;">) {
            </span><span style="color: #0000ff;">return</span> 0<span style="color: #000000;">;
        }
        </span><span style="color: #0000ff;">return</span><span style="color: #000000;"> ps;
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span> setPageSize(<span style="color: #0000ff;">int</span><span style="color: #000000;"> pageSizeValue) {
        pageSize.set(pageSizeValue);
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">static</span> <span style="color: #0000ff;">void</span><span style="color: #000000;"> removePageSize() {
        pageSize.remove();
    }
}</span></pre>
</div>
<span class="cnblogs_code_collapse">PaginationContext.java</span></div>
<p>实现了前台页面向ServiceImpl中传递分页参数： pageNum 和 pageSize.</p>
<p>当然从请求中获取分页参数pageNum和pageSize需要用到filter:</p>
<div class="cnblogs_code" onclick="cnblogs_code_show('ef37afaf-3d41-49f5-984c-83d090993873')"><img id="code_img_closed_ef37afaf-3d41-49f5-984c-83d090993873" class="code_img_closed" src="http://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif" alt="" /><img id="code_img_opened_ef37afaf-3d41-49f5-984c-83d090993873" class="code_img_opened" style="display: none;" onclick="cnblogs_code_hide('ef37afaf-3d41-49f5-984c-83d090993873',event)" src="http://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif" alt="" />
<div id="cnblogs_code_open_ef37afaf-3d41-49f5-984c-83d090993873" class="cnblogs_code_hide">
<pre><span style="color: #000000;">public class PageFilter implements Filter {
    public PageFilter() {}

    public void destroy() {}

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {

        HttpServletRequest httpRequest = (HttpServletRequest) request;

        PaginationContext.setPageNum(getPageNum(httpRequest));
        PaginationContext.setPageSize(getPageSize(httpRequest));

        try {
            chain.doFilter(request, response);
        }
        // 使用完Threadlocal，将其删除。使用finally确保一定将其删除
        finally {
            PaginationContext.removePageNum();
            PaginationContext.removePageSize();
        }
    }

    /**
     * 获得pager.offset参数的值
     * 
     * @param request
     * @return
     */
    protected int getPageNum(HttpServletRequest request) {
        int pageNum = 1;
        try {
            String pageNums = request.getParameter("pageNum");
            if (pageNums != null &amp;&amp; StringUtils.isNumeric(pageNums)) {
                pageNum = Integer.parseInt(pageNums);
            }
        } catch (NumberFormatException e) {
            e.printStackTrace();
        }
        return pageNum;
    }

    /**
     * 设置默认每页大小
     * 
     * @return
     */
    protected int getPageSize(HttpServletRequest request) {
        int pageSize = 10;    // 默认每页10条记录
        try {
            String pageSizes = request.getParameter("pageSize");
            if (pageSizes != null &amp;&amp; StringUtils.isNumeric(pageSizes)) {
                pageSize = Integer.parseInt(pageSizes);
            }
        } catch (NumberFormatException e) {
            e.printStackTrace();
        }
        return pageSize;
    }

    /**
     * @see Filter#init(FilterConfig)
     */
    public void init(FilterConfig fConfig) throws ServletException {}

}</span></pre>
</div>
<span class="cnblogs_code_collapse">PageFilter.java </span></div>
<p>PageFilter在web.xml中的配置：</p>
<div class="cnblogs_code">
<pre>    <span style="color: #008000;">&lt;!--</span><span style="color: #008000;"> pagination filter </span><span style="color: #008000;">--&gt;</span>
    <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">filter</span><span style="color: #0000ff;">&gt;</span>
          <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">filter-name</span><span style="color: #0000ff;">&gt;</span>PageFilter<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">filter-name</span><span style="color: #0000ff;">&gt;</span>
          <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">filter-class</span><span style="color: #0000ff;">&gt;</span>com.ems.filter.PageFilter<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">filter-class</span><span style="color: #0000ff;">&gt;</span>
      <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">filter</span><span style="color: #0000ff;">&gt;</span>
      <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">filter-mapping</span><span style="color: #0000ff;">&gt;</span>
          <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">filter-name</span><span style="color: #0000ff;">&gt;</span>PageFilter<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">filter-name</span><span style="color: #0000ff;">&gt;</span>
          <span style="color: #0000ff;">&lt;</span><span style="color: #800000;">url-pattern</span><span style="color: #0000ff;">&gt;</span>/*<span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">url-pattern</span><span style="color: #0000ff;">&gt;</span>
      <span style="color: #0000ff;">&lt;/</span><span style="color: #800000;">filter-mapping</span><span style="color: #0000ff;">&gt;</span></pre>
</div>
<p>OK，到此，PageHelper的使用方法，基本结束。</p>
<p>PageHelper 项目地址：http://git.oschina.net/free/Mybatis_PageHelper</p>
<p>文档地址：http://git.oschina.net/free/Mybatis_PageHelper/blob/master/wikis/HowToUse.markdown</p></div><div id="MySignature"></div>
<div class="clear"></div>
<div id="blog_post_info_block">
<div id="BlogPostCategory"></div>
<div id="EntryTag"></div>
<div id="blog_post_info">
</div>
<div class="clear"></div>
<div id="post_next_prev"></div>
</div>


	</div>
	
	<div class="postfoot">
		posted on <span id="post-date">2015-06-29 22:15</span> <a href='http://www.cnblogs.com/digdeep/'>digdeep</a> 阅读(<span id="post_view_count">...</span>) 评论(<span id="post_comment_count">...</span>)  <a href ="https://i.cnblogs.com/EditPosts.aspx?postid=4608933" rel="nofollow">编辑</a> <a href="#" onclick="AddToWz(4608933);return false;">收藏</a>
	</div>
</div>
<script type="text/javascript">var allowComments=true,cb_blogId=207682,cb_entryId=4608933,cb_blogApp=currentBlogApp,cb_blogUserGuid='03c9a2db-e87a-e411-b908-9dcfd8948a71',cb_entryCreatedDate='2015/6/29 22:15:00';loadViewCount(cb_entryId);</script>

</div><a name="!comments"></a><div id="blog-comments-placeholder"></div><script type="text/javascript">var commentManager = new blogCommentManager();commentManager.renderComments(0);</script>
<div id='comment_form' class='commentform'>
<a name='commentform'></a>
<div id='divCommentShow'></div>
<div id='comment_nav'><span id='span_refresh_tips'></span><a href='javascript:void(0);' onclick='return RefreshCommentList();' id='lnk_RefreshComments' runat='server' clientidmode='Static'>刷新评论</a><a href='#' onclick='return RefreshPage();'>刷新页面</a><a href='#top'>返回顶部</a></div>
<div id='comment_form_container'></div>
<div class='ad_text_commentbox' id='ad_text_under_commentbox'></div>
<div id='ad_t2'></div>
<div id='opt_under_post'></div>
<div id='ad_c1' class='c_ad_block'></div>
<div id='under_post_news'></div>
<div id='ad_c2' class='c_ad_block'></div>
<div id='under_post_kb'></div>
<div id='HistoryToday' class='c_ad_block'></div>
<script type='text/javascript'>
    fixPostBody();
    setTimeout(function () { incrementViewCount(cb_entryId); }, 50);
    deliverAdT2();
    deliverAdC1();
    deliverAdC2();    
    loadNewsAndKb();
    loadBlogSignature();
    LoadPostInfoBlock(cb_blogId, cb_entryId, cb_blogApp, cb_blogUserGuid);
    GetPrevNextPost(cb_entryId, cb_blogId, cb_entryCreatedDate);
    loadOptUnderPost();
    GetHistoryToday(cb_blogId, cb_blogApp, cb_entryCreatedDate);   
</script>
</div>


</div>
