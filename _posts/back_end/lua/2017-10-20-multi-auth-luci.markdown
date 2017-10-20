---
layout: post
title:  "LuCI中的多用户权限管理"
date:   2017-10-20 16:04:21 +0800
categories: lua luci
---

## LuCI中的多用户权限管理

在OpenWRT中通常使用LuCI实现网页方便用户进行配置，由于LuCI官方并不支持多用户多权限，
最近使用过程中需要，简单的实现了Multi-User功能。

### 基本原理

1. 添加multiUser配置文件，将允许登录的用户的username（用户名），password（密码），
level（权限等级）写入，按照CUI格式声明

2. 在登录时，page.sysauth会对用户进行权限验证，如果登录用户在page.sysauth中，进入页面，
否则显示空白页面。因此在此读取配置文件，将配置文件中声明的内容动态添加到page.sysauth中。

3. 在验证登录过程中，之前是通过系统进行用户名和密码验证，如果需要添加新的用户可以用useradd指令。
而目前改为不通过系统用户进行验证，而是通过multiUser配置文件进行验证，

4. 在显示页面的所有node中，通过判断当前用户的level，设置该node显示与否。


### 具体更改

1. luci/controller/admin/index.lua

	function index()
			local fs = require "nixio.fs"
			local UCI = require("luci.model.uci")
			local uci = UCI.cursor()
			local multiUser = { }


			local root = node()
			if not root.target then
					root.target = alias("admin")
					root.index = true
			end

			local page   = node("admin")
			page.target  = firstchild()
			page.title   = _("Administration")
			page.order   = 10
			-- page.sysauth = "root"
			page.sysauth = { "root" }
			if fs.access("/etc/config/multiUser") then
					multiUser = uci:get_all("multiUser")
					for k, v in pairs(multiUser) do
							if type(v) == "table" then
									for j, l in pairs(v) do
											if j == "user" then
													table.insert(page.sysauth, l)
											end
									end
							end
					end
			end
			page.sysauth_authenticator = "htmlauth"
			page.ucidata = true
			page.index = true

			-- Empty services menu to be populated by addons
			entry({"admin", "services"}, firstchild(), _("Services"), 40).index = true

			entry({"admin", "logout"}, call("action_logout"), _("Logout"), 90)
	end


2. luci/controller/admin/servicectl.lua

	function index()
	--      entry({"servicectl"}, alias("servicectl", "status")).sysauth = "root"
			entry({"servicectl"}, alias("servicectl", "status"))
			entry({"servicectl", "status"}, call("action_status")).leaf = true
			entry({"servicectl", "restart"}, call("action_restart")).leaf = true
	end

3. luci/dispatcher.lua

	function authenticator.htmlauth(validator, accs, default)                                    
		local user = http.formvalue("luci_username")                                         
		local pass = http.formvalue("luci_password")                                         
		local user_table = ""                                                                
		local pass_table = ""                                                                
		local level_table = ""                                                               
		local fs = require "nixio.fs"                                                        
		local UCI = require("luci.model.uci")                                                
		local uci = UCI.cursor()                                                             
		local multiUser = { }                                                                
		local file=io.open("/var/luci/shadow","w")                                           
		local levelfile = io.open("/var/luci/level", "w")                                    
																							 
		if fs.access("/etc/config/multiUser") then                                           
				multiUser = uci:get_all("multiUser")                                         
				for k, v in pairs(multiUser) do                                              
						if type(v) == "table" then                                           
								user_table = ""                                              
								pass_table = ""                                              
								level_talbe = ""                                             
								for j, l in pairs(v) do                                      
										if j == "user" then                                  
												user_table = l                               
										end                                                  
										if j == "pwd" then                                   
												pass_table = l                               
										end                                                  
										if j == "level" then                                 
												level_table = l                              
										end                                                  
								end                                                          
								if user_table == user and pass_table == pass then            
										if not file then                                     
												luci.util.exec("mkdir -p /var/luci/")        
												file = io.open("/var/luci/shadow", "w")      
										end                                                  
										if not levelfile then                                
												luci.util.exec("mkdir -p /var/luci/")        
												levelfile = io.open("/var/luci/level", "w")  
										end                                                  
										-- level = level_table                               
										file:write(user)                                     
										file:write("\n")                                     
										file:write(level_table)                              
										file:write("\n")                                     
										file:close()                                         
										levelfile:write(level_table)                         
										levelfile:close()                                    
										return user                                          
								end                                                          
						end                                                                  
				end                                                                          
		end                                                                                  
																							 
--[[    if user and validator(user, pass) then                                               
				return user                                                                  
		end                                                                                  
]]--                                                                                         
		if context.urltoken.stok then                                                        
				context.urltoken.stok = nil                                                  
																							 
				local cookie = 'sysauth=%s; expires=%s; path=%s/' %{                         
					http.getcookie('sysauth') or 'x',                                        
						'Thu, 01 Jan 1970 01:00:00 GMT',                                     
						build_url()                                                          
				}                                                                            
																							 
				http.header("Set-Cookie", cookie)                                            
				http.redirect(build_url())                                                   
		else                                                                                 
				require("luci.i18n")                                                         
				require("luci.template")                                                     
				context.path = {}                                                            
				http.status(403, "Forbidden")                                                
				luci.template.render("sysauth", {duser=default, fuser=user})                 
		end                                                                                  
																							 
		return false                                                                         
																								 
	end
	
	function dispatch(request) 
		
		...
		
		local levelfile = io.open("/var/luci/level", "r")                            
		if not levelfile then                                                        
				level = '2'                                                          
		else                                                                         
				level = levelfile:read("*a")                                         
		end                                                                          
																					 
		if level == "2" then                                                         
	--[[	
			for j, k in pairs(context.tree.nodes.admin.nodes.system.nodes) do    
					if j == "reboot" then                                        
							k.title = ''                                         
							k.leaf = true                                        
							k.target = ''                                        
					end                                                          
			end                                                                  
	]]--                                                                                         
				context.tree.nodes.admin.nodes.system.title = ""                     
				context.tree.nodes.admin.nodes.system.leaf=true                      
		end

### 参考

1. [csdn](http://blog.csdn.net/guyuehuanyu/article/details/520531)
2. [github](https://github.com/Hostle/openwrt-luci-multi-user)