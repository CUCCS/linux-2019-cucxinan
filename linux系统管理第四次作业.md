#### 实验四

	###### 任务一 用bash编写一个图片批处理脚本，实现以下功能：
	
	- 使用工具
	
	  - imagemagick
	
	- 测试语句
	
	  - 支持对jpeg格式图片进行图片质量压缩
	
	    "bash job4/img.sh -d job4/img -q 50"
	
	  - 支持对jpeg/png/svg格式图片在保持原始宽高比的前提下压缩分辨
	
	    "bash job4/img.sh -d job4/img -r 50%x50%"
	
	  - 支持对图片批量添加自定义文本水印
	
	    "bash job4/img.sh -d job4/img -w "hello world" "
	
	  - 支持批量重命名（统一添加文件名前缀，不影响原始文件扩展名）
	
	    "bash job4/img.sh -d job4/img --prefix "haha"
	
	  - 支持将png/svg图片统一转换为jpg格式图片
	
	    "bash job4/img.sh -d job4/img -c "
	
	  - 支持多参数混合使用，如将png/svg图片统一转换为jpg格式图片后添加水印
	
	    "bash job4/img.sh -d job4/img -c -w  "hello world" "
　  帮助文档
	  - bash shell/image.sh -h  


	
	###### 任务二：用bash编写一个文本批处理脚本，对以下附件分别进行批量处理完成相应的数据统计任务：
	
	- 使用工具
	
	  - more
	  - awk 
	  - sort
	  - uniq
	
	- 测试语句
	
	  "- bash shell/world.sh"
	

	###### 任务三：用bash编写一个文本批处理脚本，对以下附件分别进行批量处理完成相应的数据统计任务：
	
	- 服务器日志统计结果测试语句如下
	
	  - 统计访问来源主机TOP 100和分别对应出现的总次数
	
	    "bash job4/web_log.sh -a"
	
	  - 统计访问来源主机TOP 100 IP和分别对应出现的总次数
	
	    "bash job4/web_log.sh -b"
	
	  - 统计最频繁被访问的URL TOP 100
	
	    "bash job4/web_log.sh -c"
	
	  - 统计不同响应状态码的出现次数和对应百分比
	
	    "bash job4/web_log.sh -d"
	
	  - 分别统计不同4XX状态码对应的TOP 10 URL和对应出现的总次数
	
	    "bash job4/web_log.sh -e"
	
	  - 给定URL输出TOP 100访问来源主机
	
	    "bash job4/web_log.sh -f "in24.inetnebr.com""
	
	- 帮助信息
	  - bash shell/web.sh -h
	
	###### travis ci 使用
	
	- 在travis中激活github的账号
	
	- travis运行的job log中，直接打印虚拟机输出的所有内容。