<ul id="tree" class="ztree"></ul><article class='markdown-body'>
#Lingx技术文档
##代码

    public Map<String, Object> execute(Map<String, Object> param,String appkey) throws Exception {
		Map<String,Object> ret=this.getDefaultResultMap();
		if(this.checkIsNull(param, "technician_id","技师ID",ret))return ret;
		//int technician_id=this.getTechnicianId(param.get("token").toString());
		List<Map<String,Object>> list=this.jdbcTemplate.queryForList("select "+ITechnicianService.TECHNICIAN_PUBLIC_FIELD+" from tim_technician where id=?",param.get("technician_id"));
		
		if(list.size()>0){
			this.addResult("datas", formatTechnician(list.get(0),this.jdbcTemplate), ret);
		}else{
			this.addResult("datas",new HashMap(), ret);
		}
		return ret;
	}

> 在劳而无功 城
##测试1
有志者事竟成
##测试2
有声有色
#第二部
##章一
123
123
213123

213
13
21
312
3
12
3

213

123
123
123

2

1232

23

2323


23
##章二
123

23
23
12
4
2
42
4
24
2
4
2



23
232
3
23
2
3
> 
 </article>
<script type="text/javascript" src="../javascripts/jquery-1.4.4.min.js"></script>
<script type="text/javascript" src="../javascripts/jquery.ztree.core-3.5.min.js"></script>
<script type="text/javascript" src="../javascripts/ztree_toc.js"></script>
<link rel="stylesheet" href="../stylesheets/zTreeStyle/zTreeStyle.css" type="text/css">

<SCRIPT type="text/javascript" >
 $(document).ready(function(){ $('#tree').ztree_toc({
 is_auto_number:true, 
is_highlight_selected_line:false,
documment_selector:'.markdown-body', 
ztreeStyle: { width:'260px', overflow: 'auto', position: 'fixed', 'z-index': 2147483647, border: '0px none', left: '0px', top: '0px' } 
}); });
</SCRIPT> 
