![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Find All Databases Without A Recent Backup With SQL
**Post Date: September 3, 2015**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Find databases that haven't had a backup in over 1 day. ( 24 hours ). Here's some quick sql logic that will help you get that info. This logic assumes the databases that have been backed up at least once. Basically; they require an entry in the msdb..backupset table.
</p> 


## SQL-Logic
```SQL
use master;
set nocount on
 
select
'server' = upper(@@servername)
,   'database' = upper(bs.database_name)
,   'last backup' = cast (datediff(day, max(bs.backup_finish_date), getdate()) as varchar(10)) + ' Days Old'
,   'time of backup'    = replace(replace(replace(left(max(bs.backup_finish_date), 19),':', '-'), 'AM', 'am'), 'PM', 'pm') + ' ' + datename(dw, max(bs.backup_finish_date)) from
msdb.dbo.backupset bs
where
type = 'd'
group by
bs.database_name
having
(max(bs.backup_finish_date) < dateadd(hour, -24, getdate()))

```

[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

  
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

