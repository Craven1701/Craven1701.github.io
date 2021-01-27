---
title: python中的datetime date与timedelta
published: true
---

```python
    monthnum=str(list(calendar.month_abbr).index(x[:3]))
    formated=str(datetime.now().year)+'-'+formated+':00'
    Timewindow['From_f']=Timewindow['From'].map(lambda x : datetime.strptime(x,'%Y-%m-%d %H:%M:%S'))
    DayCount=(datetime.strptime(To,'%Y-%m-%d')- datetime.strptime(From,'%Y-%m-%d')).days+1
    week = datetime.strptime(x,"%Y-%m-%d").weekday()
	data['Date']=data['Date'].astype('datetime64')
	dateDelta=data.loc[index].Date-data.loc[index-1].Date
	ax.yaxis.set_major_formatter(dates.DateFormatter("%H:%M"))#y轴格式化
```

