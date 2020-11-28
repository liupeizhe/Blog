---
title: uniapp-根据经纬度获取汉化地址
date: 2020-11-12
tags: ['uniapp']
---

方法如下：

```bash
/**
 * @export 根据经纬度获取汉化地址
 * @param {*} gnssLon
 * @param {*} gnssLat
 * @returns
 */
export function getCNAddress (gnssLon, gnssLat) {
	return new Promise(function(resolve) {
		var CN = ''
		var point = new plus.maps.Point(gnssLon, gnssLat)
		plus.maps.Map.reverseGeocode(point, {}, function(event) {
			if (gnssLon === 0 && gnssLat === 0) {
				CN = '位置获取失败'
			} else {
				CN = event.address // 转换后的地理位置
			}
			resolve(CN)
		}, function(e) {
      CN = '位置获取失败'
			resolve(CN)
		})
	})
	
}
```

要注意的是，以上代码要在真机运行，本地调试会报`plus`找不到的错误。

— END —
