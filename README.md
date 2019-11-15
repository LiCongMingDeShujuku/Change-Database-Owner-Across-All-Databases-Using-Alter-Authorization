![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 使用更改授权更改所有数据库中的数据库所有者
#### Change Database Owner Across All Databases Using Alter Authorization
**发布-日期: 2014年05月05日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
你注意到所有数据库所有者都设置为错误的帐户或个人，并且你需要一种简单的方法来检查所有数据库的数据库所有者。这是你可以获取所有数据库中的所有数据库所有者的运行小脚本。


## English
You noticed that all the database owners are set to the wrong account or person, and you need a simple way to check all databases for their database owners. Here’s alittle script you can run to get all database owners across all databases.

---
## Logic
```SQL
select
	'owner' 			= suser_sname(owner_sid)
,	'database name' 	= name
from
	sys.databases
where
	name not in ('master', 'model', 'msdb', 'tempdb') 
order by 
	database_id asc


```

现在你可以看到数据库中的所有者。如果你像大多数企业一样，你会注意到可能存在一些不一致的地方。接下来你在想，什么是最好的，最新的方式一次性更改所有数据库所有者？在这里你可以运行“alter authorization”语句，但是你不希望每次都要为每个数据库执行此操作。为什么不是所有数据库同时执行？当然不包括系统数据库。

So now you can see all the owners across the databases. Ok; so if you’re like most enterprises you’ll notice there’s some inconsistencies probably. Next you’re thinking; What’s the best, and most current way to change all the database owners in one go? Here’s where you can run the ‘alter authorization’ statement, but… you don’t want to do it for each database one at a time. Why not ALL databases? Excluding the system databases of course.
Here’s some logic to make that happen followed by the owner query above to verify. I threw in a 5 second delay between statements just for kicks.



```SQL
use master;
set nocount on
declare @change_database_owner varchar(max)
set 	@change_database_owner = ''
select 	@change_database_owner = @change_database_owner 
+	'alter authorization on database::[' + replace(name, '''', '''') + '] to MyNewDBowner;' 
+ 	char(10) from sys.databases where name not in ('master', 'model', 'msdb', 'tempdb') exec (@change_database_owner) --for xml path(''), type
 
waitfor delay '00:00:03'
 
select
	'owner' 		= suser_sname(owner_sid)
, 	'database name' = name
from
	sys.databases
where
	name not in ('master', 'model', 'msdb', 'tempdb') 
order by 
	database_id asc


```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

